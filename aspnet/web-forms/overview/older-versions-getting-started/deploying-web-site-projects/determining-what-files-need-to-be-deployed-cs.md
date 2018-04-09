---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Dosyaları olması için gerekenler belirleniyor dağıtılan (C#) | Microsoft Docs
author: rick-anderson
description: Dosyaları geliştirme ortamından üretim ortamına dağıtılması için gerekenler kısmen olup ASP.NET uygulaması bize oluşturulduğuna bağlı...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: ff5f1d7d156efa12d97382db56211a07c43178fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Dosyaları olmak zorundadır belirleme (C#) dağıtılan
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Dosyaları geliştirme ortamından üretim ortamına dağıtılması için gerekenler kısmen olup ASP.NET uygulaması Web sitesi modeli veya Web uygulama modelini kullanarak oluşturulduğuna bağlı. Bu iki proje modelleri ve dağıtım projesi modeli nasıl etkilediği hakkında daha fazla bilgi edinin.


## <a name="introduction"></a>Giriş

ASP.NET web uygulaması dağıtma, ASP.NET ile ilgili dosyaları geliştirme ortamından üretim ortamına kopyalama kapsar. ASP.NET ile ilgili dosyaları ASP.NET web sayfası biçimlendirme ve kod ve istemci ve sunucu tarafı dosyalarını destekler. İstemci tarafı destek dosyaları, web sayfaları tarafından başvurulan ve doğrudan tarayıcıya - görüntüleri, CSS dosyaları ve JavaScript dosyaları, örneğin gönderilen bu dosyalarıdır. Sunucu tarafı destek dosyalarını sunucu tarafında bir isteği işlemek için kullanılan içerir. Bu yapılandırma dosyaları, web Hizmetleri, sınıf dosyaları, yazılan veri kümeleri ve LINQ için diğerlerinin yanı sıra SQL dosyaları içerir.

Genel olarak, tüm istemci-tarafı destek dosyalarını geliştirme ortamından üretim ortamına kopyalanmış olmalıdır, ancak hangi sunucu tarafı destek dosyalarını kopyalanan olup, açıkça sunucu tarafı kodu derlemeye (bir derlediğinizüzerindebağlıdır`.dll` dosya) veya otomatik olarak oluşturulan bu derlemeler sorunlarınız varsa. Bu öğretici, dosyaları açıkça otomatik olarak gerçekleşir bu derleme adımı sahip karşı bir bütünleştirilmiş kod derleme yapılırken dağıtılması için gerekenler vurgular.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Açık derleme otomatik derleme ile karşılaştırması

ASP.NET web sayfaları bildirim temelli biçimlendirme ve kaynak koduna ayrılır. HTML, bildirim temelli biçimlendirme bölümü içeren Web denetimleri ve veri bağlama söz dizimi; Visual Basic veya C# kod içinde yazılmış olay işleyicileri kod bölümü içerir. Biçimlendirme ve kodun bölümlerini genellikle farklı dosyalarıyla ayrılır: `WebPage.aspx` sırasında bildirim temelli biçimlendirmesini içeren `WebPage.aspx.cs` kodu barındırır.

Geçerli tarih ve saat Sayfa yüklediğinde, metin özelliği ayarlanmış bir etiket denetimi içeren Clock.aspx adlı bir ASP.NET sayfasının göz önünde bulundurun. Bildirim temelli biçimlendirme bölümü (içinde `Clock.aspx`) için bir etiket Web denetimi - biçimlendirme içerecektir`<asp:Label runat="server" id="TimeLabel" />` - kod bölümü sırasında (içinde `Clock.aspx.cs`) olması gereken bir `Page_Load` olay işleyicisi aşağıdaki kod ile:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Bu sayfa, sayfanın kod bölümü için bir istek hizmet vermek ASP.NET altyapısı için sırayla ( `WebPage.aspx.cs` dosyası) ilk derlenmiş gerekir. Bu derleme açıkça veya otomatik olarak ortaya çıkabilir.

Derleme açıkça olur sonra tüm uygulamanın kaynak kodu bir veya daha fazla derlemeye derlenmiş (`.dll` dosyaları) uygulama içinde bulunan `Bin` dizin. Elde edilen otomatik olarak oluşturulan daha sonra derleme otomatik olarak derlemedir, varsayılan olarak, olursa yerleştirilen `Temporary ASP.NET` bulunabilir dosyaları klasörü `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;sürüm&gt;*, Bu konum aracılığıyla yapılandırılabilir olsa da [ `<compilation>` öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde `Web.config`. Açık derleme ile bir derlemeye ASP.NET uygulama kodu derlemek için bazı işlemler yapması gerekir ve bu adımı dağıtımdan önce gerçekleşir. Kaynak ilk erişildiğinde otomatik derleme ile web sunucusu üzerinde derleme işlemi gerçekleşir.

Hangi derleme modeli bağımsız olarak kullanırsanız, tüm ASP.NET sayfaları biçimlendirme kısmı ( `WebPage.aspx` dosyaları) üretim ortamına kopyalanması gerekir. Açık derleme ile derlemelerde yukarı kopyalamanız gerekir. `Bin` klasörü, ancak gerekmez ASP.NET sayfaları kod bölümleri kopyalayın ( `WebPage.aspx.cs` dosyaları). Otomatik derleme ile böylece kodu varsa ve sayfayı ziyaret edildiğinde otomatik olarak derlenebilir kod bölümü dosyaları kopyalamanız gerekir. Her ASP.NET web sayfası biçimlendirme bölümünü içeren bir `@Page` yönerge özelliklere sahip olan sayfa ilişkili kodu zaten açıkça derlenmiş olduğu veya olup, otomatik olarak derlenmesi için gerekli olduğunu gösteriyor. Sonuç olarak, üretim ortamında ya da derleme modeli ile sorunsuz bir şekilde çalışabilir ve açık veya otomatik derleme kullanıldığını belirtmek için özel yapılandırma ayarları uygulamak gerekmez.

Tablo 1 otomatik derleme karşı açık derlemesini kullanırken dağıtmak için farklı dosyalar özetler. Derleme bağımsız olarak model kullandığınız not derlemelerde her zaman dağıtmalısınız `Bin` bu klasörü varsa klasör. `Bin` Klasör web uygulamaya özgü açık derlemesini modeli kullanılırken derlenmiş kaynak kodunu içeren derlemelerini içerir. `Bin` Dizini de içeren diğer projeler derlemelerden ve kullanarak açık kaynaklı veya üçüncü taraf derlemeler ve bu üretim sunucusunda olması gerekir. Bu nedenle, genel altın kural, kopyalama `Bin` dağıtırken üretim klasöre. (Otomatik derleme modelini kullanarak ve tüm dış derlemeler kullanmıyorsanız durumunda sizi bir `Bin` sorunsuz dizin -!)

| **Derleme modeli** | **Biçimlendirme bölümü dosyasını dağıtmak?** | **Kaynak kodu dosyasının dağıtabilir?** | **Derlemeleri dağıtma `Bin` dizin?** |
| --- | --- | --- | --- |
| Açık derleme | Evet | Hayır | Evet |
| Otomatik derleme | Evet | Evet | Evet (varsa) |

**Tablo 1:** dağıttığınız hangi dosyaların kullanılan derleme modelini bağlıdır.

## <a name="taking-a-trip-down-memory-lane"></a>Seyahat bellek yolundan alma

Hangi derleme yaklaşım kullanılır, kısmen nasıl Visual Studio'da ASP.NET uygulaması yönetilen bağlıdır. Bu yana. Dört farklı sürümlerini Visual Studio - Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 ve Visual Studio 2008 olmuştur 2000 yılında NET'in başlangıcı. Visual Studio .NET 2002 ve 2003 yönetilen kullanarak ASP.NET uygulamaları *Web uygulama projesi modeli*. Web uygulama projesi modelinin anahtar özellikleri şunlardır:

- Oluşma şekli proje tek proje dosyasında tanımlanan dosyalar. Proje dosyasında tanımlı değil tüm dosyaları bölümü web uygulamasının Visual Studio tarafından kabul edilmez.
- Açık derleme kullanır. Proje derleme derler projedeki kod dosyaları yerleştirilir tek bir derleme halinde `Bin` klasör.

Microsoft Visual Studio 2005 yayımlandığı zaman Web uygulama projesi modeli desteği bırakılan ve Web sitesi projesini modeliyle değiştirildi. Web sitesi projesini modeli kendisini aşağıdaki yollarla Web uygulama projesi modelden Ayrıştırılan:

- Proje dosyaları harfe dönüştüren bir tek proje dosyası yerine, dosya sistemi yerine kullanılır. Kısacası, web uygulama klasörü (veya klasörleri) içindeki tüm dosyalar, projenin bir parçası olarak kabul edilir.
- Visual Studio Proje derleme bir derlemede oluşturmaz `Bin` dizin. Bunun yerine, bir Web sitesi projesi oluşturma, derleme zamanı hataları bildirir.
- Otomatik derleme desteği. Kodu önceden derlenmiş (kesin compilation) olabilse de Web sitesi projeleri genellikle üretim ortamına, biçimlendirme ve kaynak kodu kopyalayarak dağıtılır.

Visual Studio 2005 Service Pack 1 yayımlandığı zaman Microsoft Web uygulama projesi modeli geri. Ancak, yalnızca Web sitesi projesini modeli desteklemek Visual Web Developer devam. İyi haber, bu sınırlamaya Visual Web Developer 2008 Service Pack 1 ile bırakıldı ' dir. Bugün, Visual Studio (ve Visual Web Developer) Web uygulama projesi modeli veya Web sitesi projesini modeli kullanarak ASP.NET uygulamaları oluşturabilirsiniz. Her iki modelleri kendi Artıları ve eksileri vardır. Başvurmak [Web Uygulama projeleri giriş: karşılaştırma Web sitesi projeleri ve Web Uygulama projeleri](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) iki model ve hangi proje modeli durumunuza en iyi şekilde çalışır karar vermenize yardımcı olacak bir karşılaştırması.

## <a name="exploring-the-sample-web-application"></a>Örnek Web uygulamasını keşfetme

Bu öğretici için karşıdan yükleme rehberi incelemeler adlı bir ASP.NET uygulaması içerir. Web sitesi birisi oluşturabilir hobi Web sitesi taklit eder kendi kitap paylaşmak için çevrimiçi toplulukla gözden geçirir. Bu ASP.NET web uygulaması oldukça basittir ve aşağıdaki kaynakları oluşur:

- `Web.config`, uygulamanın yapılandırma dosyası.
- Bir ana sayfa (`Site.master`).
- Yedi farklı ASP.NET sayfaları: 

    - ~`/Default.aspx`-sitenin giriş sayfası.
    - ~`/About.aspx` -bir "İlgili Site" sayfası.
    - ~`/Fiction/Default.aspx` -incelendi kurgu books listelendiği bir sayfa. 

        - ~`/Fiction/Blaze.aspx` -Richard Bachman Romanım gözden *Blaze*.
    - ~/`Tech/Default.aspx` -incelendi teknolojisi books listelendiği bir sayfa. 

        - ~/`Tech/CYOW.aspx`-gözden *oluşturma kendi Web sitenizi*.
        - ~/`Tech/TYASP35.aspx` -gözden *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*.
- Üç farklı CSS stilleri klasöründeki dosyaları.
- Dört bulunan - ASP.NET logo ve üç geçirilmiş books kapsar görüntülerini tarafından desteklenen - tüm resim dosyaları `Images` klasör.
- A `Web.sitemap` site haritası tanımlar ve menülerde görüntülemek için kullanılan dosya `Default.aspx` kök dizininde sayfaları ve `Fiction` ve `Tech` klasörler.
- Adlı bir sınıf dosyası `BasePage.cs` temel tanımlayan `Page` sınıfı. Bu sınıf işlevselliğini genişleten `Page` otomatik olarak ayarlayarak sınıfı `Title` site haritası sayfanın konumda temel özelliği. Buna koysalar herhangi bir ASP.NET arka plan kodu sınıfını genişleten `BasePage` (yerine `System.Web.UI.Page`) başlığını site haritası içindeki konumuna bağlı olarak bir değere ayarlamanız gerekir. Örneğin, görüntülerken ~ /`Tech/CYOW.aspx` sayfası, başlığı "Giriş: teknolojisi: oluşturmak bilgisayarınızı kendi Web sitesi için" ayarlanır.

Şekil 1 bir tarayıcıdan görüntülendiğinde Kitap incelemeleri Web sitesi ekran görüntüsü gösterilmektedir. Sayfa Burada gördüğünüz ~ /`Tech/TYASP35.aspx`, Kitap incelemeleri *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*. Sayfa ve sol sütunda menü üstündeki yayılan içerik haritası tanımlanan site haritası yapısı dayanır `Web.sitemap`. Görüntünün sağ üst köşedeki görüntüleri bulunan kitap kapak biri `Images` klasör. Web sitesinin görünüm geçişli stil sayfası kuralları stilleri klasöründeki CSS dosyaları tarafından kapsayıcı sayfa düzeni ana sayfada tanımlı sırada yazıyla aracılığıyla tanımlanmış `Site.master`.


[![Bir sınıflama başlıklarının üzerinde incelemeler Kitap incelemeleri Web sitesi sunar](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Şekil 1:** bir sınıflama başlıklarının üzerinde incelemeler Kitap incelemeleri Web sitesi sunar ([tam boyutlu görüntüyü görüntülemek için tıklatın](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Bu uygulama bir veritabanı kullanmaz; her gözden geçirme, uygulama içinde ayrı bir web sayfası olarak uygulanır. Bu öğretici (ve sonraki birkaç öğreticileri) bir veritabanı olmayan bir web uygulaması dağıtma aracılığıyla yol. Ancak, bir sonraki öğretici biz incelemeleri, okuyucu açıklamaları ve bir veritabanı içinde diğer bilgileri depolamak için bu uygulamayı iyileştirmek ve adımları doğru bir veri tabanlı web uygulamasını dağıtmak için yapılması gereken inceleyeceksiniz.

> [!NOTE]
> Bu öğreticileri ASP.NET uygulamaları bir web ana makine sağlayıcısı ile barındırma odaklanmanıza ve ASP gibi bulunabilecek konuları inceleyin değil. NET'in site haritası sistem ya da bir taban kullanarak `Page` sınıfı. Bu teknolojiler hakkında daha fazla bilgi ve öğretici kapsanan diğer konular hakkında daha fazla arka plan için her öğreticinin sonunda daha fazla bilgi bölümüne bakın.


Bu öğreticinin indirme web uygulamasının iki kopya varsa, her farklı bir Visual Studio Proje türü olarak uygulanan: BookReviewsWAP, Web uygulama projesi ve BookReviewsWSP, bir Web sitesi projesi. Her iki proje Visual Web Developer 2008 SP1 ile oluşturulmuş ve ASP.NET 3.5 SP1'i kullanın. Çalışmak için masaüstünüze içeriği unzipping tarafından bu projeleri başlatın. Web uygulaması projesi (BookReviewsWAP) açmak için BookReviewsWAP klasöre gidin ve çözüm dosyasına çift tıklayarak `BookReviewsWAP.sln`. Web sitesi projesini (BookReviewsWSP) açmak için Visual Studio'yu başlatın ve sonra Dosya menüsünden Web sitesini açın seçeneği, Gözat `BookReviewsWSP` , masaüstünüzdeki klasörünü ve Tamam'ı tıklatın.


Bu öğretici göz hangi dosyaların kalan iki bölümde üretim ortamına Uygulama dağıtırken kopyalamanız gerekir. Sonraki iki öğreticileri - *[bilgisayarınızı Site kullanarak FTP dağıtımı](deploying-your-site-using-an-ftp-client-cs.md)* ve *[dağıtma bilgisayarınızı Site kullanarak Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -için farklı yollar Göster Bu dosyaları bir web ana bilgisayar sağlayıcısına kopyalayın.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Web uygulaması projesi için dağıtmak için dosyaları belirleme

Web uygulama projesi modeli açık derlemesini kullanır - projenin kaynak kodu tek bir derlemeye uygulamayı derlediğinizde her zaman derlenmiştir. Bu derleme ASP.NET sayfaları arka plan kod dosyaları içerir (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`, vb.), yanı sıra `BasePage.cs` sınıfı. Sonuçta elde edilen derleme BookReviewsWAP.dll olarak adlandırılır ve uygulamanın içinde bulunan `Bin` dizin.

Şekil 2 Kitap incelemeleri Web uygulama projesi dosyaları gösterir.


[![Çözüm Gezgini'nde Web uygulaması projesi oluşturan dosyaları listeler](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Şekil 2**: Çözüm Gezgini'nde Web uygulaması projesi oluşturan dosyaların listeler


Bir ASP.NET uygulamasını dağıtmak için en son kaynak kodu derlemeye derleyebilir amacıyla uygulama oluşturarak Web uygulama projesi modeli start kullanarak geliştirmiştir. Ardından, aşağıdaki dosyaları üretim ortamına kopyalayın:

- Her ASP.NET için bildirim temelli biçimlendirme içeren dosyaları sayfasında, aşağıdaki gibi ~ /`Default.aspx`, ~ /`About.aspx`ve benzeri. Ayrıca, kopya tüm ana sayfalar ve kullanıcı denetimleri için bildirim temelli biçimlendirme.
- Derlemeler (`.dll` dosyaları) içinde `Bin` klasör. Program veritabanı dosyalarını kopyalamak gerekmez (`.pdb`) veya tüm XML dosyaları Bul `Bin` dizin.

ASP.NET sayfaları kaynak kodu dosyaları üretim ortamına kopyalayın gerekmez ya da kopyalama gerek `BasePage.cs` sınıf dosyası.

> [!NOTE]
> Şekil 2'de görüldüğü gibi `BasePage` sınıfı projedeki adlı klasöre yerleştirilen bir sınıf dosyası olarak gerçekleştirilen `HelperClasses`. Proje derlenmiş zaman kodda `BasePage.cs` dosya ASP.NET sayfaları arka plan kodu sınıfları yanı sıra tek bütünleştirilmiş koda derlenmiş `BookReviewsWAP.dll.` ASP.NET adlı özel bir klasör olan `App_Code` Web için sınıf dosyalarını tutmak için tasarlanmış Site projeleri. Kodda `App_Code` klasörü otomatik olarak derlenir ve bu nedenle Web Uygulama projeleri ile kullanılmamalıdır. Bunun yerine, uygulamanızın sınıf dosyaları adlı normal bir klasörde yerleştirmelisiniz `HelperClasses`, veya `Classes`, veya benzeri. Alternatif olarak, ayrı bir sınıf kitaplığı proje ile sınıf dosyaları yerleştirebilirsiniz.


ASP.NET ile ilgili biçimlendirme dosyaları ve derlemede kopyalama yanı sıra `Bin` klasörü, ayrıca diğer sunucu tarafı destek dosyalarını yanı sıra - görüntüler ve CSS dosyaları - istemci tarafı destek dosyalarını kopyalamak için ihtiyacınız `Web.config` ve `Web.sitemap`. Bu istemci ve sunucu tarafı dosyaları açık veya otomatik derleme kullanıp bağımsız olarak üretim ortamına kopyalanmasına gerek destekler.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>İçin Web sitesi projesi dosyaları dağıtmak için dosyaları belirleme

Web sitesi projesini modeli otomatik derleme, Web uygulama projesi modeli kullanılırken kullanılamaz bir özelliği destekler. Açık derleme ile bir derlemeye projenizin kaynak kodu derleme ve bu derleme üretim ortamına kopyalayın. Diğer taraftan, otomatik derleme ile sadece kaynak kodunu üretim ortamına kopyalayın ve onu isteğe bağlı çalışma zamanı tarafından gerektiği şekilde derlenir.

Visual Studio'da derleme menü seçeneği, Web Uygulama projeleri ve Web sitesi projeleri mevcuttur. Web Uygulama projeleri oluşturma derler projenin kaynak kodu bulunan tek bir derleme halinde `Bin` directory; bir Web sitesi projesini derleme zamanı hatalarını denetler, ancak tüm derlemelerde oluşturmaz oluşturma. Yapmanız gereken Web sitesi projesini modeli tüm kullanılarak geliştirilen bir ASP.NET uygulamasını dağıtmak için kopyalama üretim ortamına uygun dosyaları olmakla birlikte, t ilk yapı hiçbir derleme zamanı hataları olduğundan emin olmak için proje öneririz.

Şekil 3 Kitap incelemeleri Web sitesi projeyi oluşturan dosyaları gösterir.


 [![Çözüm Gezgini Web sitesi projesini oluşturan dosyaları listeler](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Şekil 3**: Solution Explorer Web sitesi projesini oluşturan dosyaların listeler


Bir Web sitesi projesini dağıtma, ASP.NET ile ilgili tüm dosyaların ASP.NET sayfaları, ana sayfalar ve kullanıcı denetimleri için işaretleme sayfaları birlikte, kod dosyaları içerir üretim ortamına - kopyalama içerir. Ayrıca BasePage.cs gibi herhangi bir sınıf dosyalarını kopyalamanız gerekir. Unutmayın `BasePage.cs` dosyasının bulunduğu `App_Code` Web sitesi projelerinde sınıfı dosyaları için kullanılan özel bir ASP.NET klasörü klasörü. Özel klasör üretimde, de sınıfı dosyalar olarak oluşturulmalıdır `App_Code` geliştirme ortamında klasörüne kopyalanmasını, için `App_Code` üretim klasörü.

ASP.NET biçimlendirme ve kaynak kodu dosyaları kopyalamaya ek olarak diğer sunucu tarafı destek dosyalarını yanı sıra - görüntüler ve CSS dosyaları - istemci tarafı destek dosyalarını kopyalamak etmeniz `Web.config` ve `Web.sitemap`.

> [!NOTE]
> Web sitesi projeleri açık derlemesini de kullanabilirsiniz. Bir sonraki öğretici bir Web sitesi projesini derleyebilir nasıl inceleyeceksiniz.


## <a name="summary"></a>Özet

Bir ASP.NET uygulaması dağıtımı için gerekli dosyaları geliştirme ortamından üretim ortamına kopyalama kapsar. ASP.NET uygulama kodu açıkça veya otomatik olarak derlenmiş üzerinde eşitlenmiş gereken dosyalar kesin kümesi bağlıdır. İşe derleme stratejisi, Web uygulama projesi modeli veya Web sitesi projesini modeli kullanarak ASP.NET uygulamasını yönetmek için Visual Studio yapılandırılmış tarafından etkilenir.

Web uygulama projesi modeli açık derlemesini kullanır ve tek bir derleme halinde projenin kodu derler `Bin` klasör. Dağıtırken uygulama, ASP.NET sayfaları biçimlendirme bölümünü ve içeriğini `Bin` klasörü gerekir gönderilen üretim ortamına; - kod dosyaları ve örneğin arka plan kodu sınıfları - uygulamanın kaynak kodunda gerek yoktur üretim ortamına kopyalanacak.

Öğreticiler gelecekte göreceğiz gibi bir Web sitesi projesini derleyebilir mümkün olmasına rağmen Web sitesi projesini modelini otomatik derleme varsayılan olarak, kullanır. Gerektirir otomatik derleme kullanan ASP.NET uygulaması dağıtma biçimlendirme bölümü *ve* üretim ortamına kaynak kodu kopyalanır. İlk kez istendiğinde kodu otomatik olarak üretim ortamına derlenir.

Şu dosyaları geliştirme ve üretim ortamlarını arasında eşitlenir gerekenleri inceleyen artık, biz web ana makine sağlayıcısı Kitap incelemeleri uygulamayı dağıtmak için hazır olursunuz.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET derleme genel bakış](https://msdn.microsoft.com/library/ms178466.aspx)
- [ASP.NET kullanıcı denetimleri](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [ASP inceleniyor. NET'in Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Web uygulaması projelerine giriş](https://msdn.microsoft.com/library/aa730880.aspx)
- [Ana sayfa öğreticileri](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Kod sayfaları arasında paylaşma](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Özel bir taban sınıf, ASP.NET sayfaları için arka plan kodu sınıfları kullanma](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005'ın Web sitesi proje sisteminin: nedir ve neden biz bunu?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [İzlenecek yol: bir Web sitesi projesini Visual Studio'da bir Web uygulaması projesi dönüştürme](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Önceki](asp-net-hosting-options-cs.md)
> [sonraki](deploying-your-site-using-an-ftp-client-cs.md)
