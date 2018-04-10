---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 0bfe9cdc215226457ccfafff2b85ace87325b91b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Genel bakış](#overview)
- [Yükleme notları](#installation-notes)
- [Yazılım gereksinimleri](#software-requirements)
- [Belgeler](#documentation)
- [Destek](#support)
- [Bir ASP.NET MVC 2 proje için ASP.NET MVC yükseltme 3 araçları güncelleştirme](#upgrading)
- [ASP.NET MVC 3 araçları güncelleştirme (12 Nisan 2011)](#tu-changes)

    - ["Denetleyici Ekle" iletişim kutusu artık görünümleri ve veri erişim kodu denetleyicileriyle iskele](#tu-AddControllerDialog)
    - [İyileştirmeler "ASP.NET MVC 3 yeni proje" iletişim kutusu](#tu-ImprovementsNewDialogBox)
    - [Proje şablonları artık Modernizr 1.7 içerir](#tu-Modernizr)
    - [Proje şablonları içerir jQuery, jQuery UI ve jQuery güncelleştirilmiş sürümleri doğrulama](#tu-UpdatedJQuery)
    - [Proje şablonları, ADO.NET Entity Framework 4.1 artık önceden yüklenmiş bir NuGet paketi olarak içerir.](#tu-EF)
    - [Proje şablonları JavaScript kitaplıklarını önceden yüklenmiş NuGet paketleri olarak içerir.](#tu-JavaScriptLibsNuget)
    - [Bilinen Sorunlar](#tu-KI)
- [ASP.NET MVC 3 RTM (13 Ocak 2011)](#MVC3RTM)

    - [Değişikliği: jQuery UI sürümü 1.8.7 için güncelleştirilmiştir.](#RTM-1)
    - [Değişikliği: ModelMetadataProvider geri DataAnnotationsModelMetadataProvider için varsayılan değiştirildi](#RTM-2)
    - [Sabit: boşluk sonuçlarda tersine içeren bir Razor ifade parçası yapıştırma](#RTM-3)
    - [Sabit: düzenleyicide açılan bir Razor dosyasını yeniden adlandırma söz dizimi renklendirme ve IntelliSense devre dışı bırakır](#RTM-4)
    - [Bilinen Sorunlar](#RTM-KI)
    - [Yeni değişiklikler](#RTM-BC)
- [ASP.NET MVC 3 Sürüm Adayı 2 (10 Aralık 2010)](#_Toc2)

    - [Proje şablonları jQuery 1.4.4, jQuery doğrulama 1.7 ve jQuery UI 1.8.6y UI 1.8.6 içerecek şekilde değiştirildi.](#_Toc2_1)
    - [Eklenen "AdditionalMetadataAttribute" sınıfı](#_Toc2_2)
    - [Geliştirilmiş görünüm yapı İskelesi](#_Toc2_3)
    - [Eklenen Html.Raw yöntemi](#_Toc2_3)
    - [Yeniden adlandırılmış "Controller.ViewModel" özelliğini ve "Görünüm"paketi "Görünüm" özelliğini](#_Toc2_4)
    - ["SessionStateAttribute" olarak yeniden adlandırıldı "ControllerSessionStateAttribute" sınıfı](#_Toc2_5)
    - [Yeniden adlandırılmış RemoteAttribute "Alanlar" özelliği "AdditionalFields"](#_Toc2_6)
    - ["AllowHtmlAttribute" için "SkipRequestValidationAttribute" yeniden adlandırıldı](#_Toc2_7)
    - [İlk yararlı hata iletisini görüntülemek üzere değiştirilen "Html.ValidationMessage" yöntemi](#_Toc2_8)
    - [Sabit @model belgeye boşluk eklememeyi bildirimi](#_Toc2_9)
    - [Görünüm altyapıları altyapısı özgü dosya adlarını desteklemek için eklenen "FileExtensions" özelliği](#_Toc2_10)
    - [Sabit "LabelFor" Yardımcısı "İçin" özniteliği için geçerli bir değer Yayma](#_Toc2_11)
    - [Model bağlama sırasında açık değerleri öncelik vermek için sabit "RenderAction" yöntemi](#_Toc2_12)
    - [Yeni değişiklikler](#_Toc2_BC)
    - [Bilinen Sorunlar](#_Toc2_KI)
- [ASP.NET MVC 3 Sürüm Adayı (9 Kas 2010)](#TOC_ASP_NET_3_RC)

    - [ASP.NET MVC 3 RC yeni özellikler](#_Toc276711785)
    - [NuGet Package Manager](#_Toc276711786)
    - ["Yeni Proje" Gelişmiş iletişim kutusu](#_Toc276711787)
    - [Oturumsuz denetleyicileri](#_Toc276711788)
    - [Yeni doğrulama öznitelikleri](#_Toc276711789)
    - ["LabelFor" ve "LabelForModel" yöntemleri için yeni aşırı yüklemeleri](#_Toc276711790)
    - [Alt eylem çıktı önbelleği](#_Toc276711791)
    - ["Görünümü Ekle" iletişim kutusunu geliştirmeleri](#_Toc276711792)
    - [Ayrıntılı istek doğrulama](#_Toc276711793)
    - [Yeni değişiklikler](#_Toc276711794)
    - [Bilinen Sorunlar](#_Toc276711795)
- [ASP. MVC 3 (6 eki 2010) Beta Notlar](#TOC_ASP_NET_3_Beta)

    - [ASP.NET MVC 3 Beta sürümündeki yeni özellikler](#0.1__Toc274034215)
    - [NuPack Paket Yöneticisi](#0.1__Toc274034216)
    - [Geliştirilmiş yeni proje iletişim kutusu](#0.1__Toc274034217)
    - [Kesinlikle belirtmek için basitleştirilmiş yol modelleri Razor görünümleri belirtilmiş.](#0.1__Toc274034218)
    - [Yeni ASP.NET Web sayfaları için yardımcı yöntemler desteği](#0.1__Toc274034219)
    - [Ek bağımlılık ekleme desteği](#0.1__Toc274034220)
    - [Örtük jQuery tabanlı Ajax yeni desteği](#0.1__Toc274034221)
    - [Örtük jQuery doğrulama için yeni destek](#0.1__Toc274034222)
    - [İstemci doğrulama ve örtük JavaScript için yeni uygulama çapında bayrakları](#0.1__Toc274034223)
    - [Yeni görünümler çalıştırmadan önce çalışan bir kod desteği](#0.1__Toc274034224)
    - [Yeni destek VBHTML Razor sözdizimi için](#0.1__Toc274034225)
    - [ValidateInputAttribute üzerinde daha ayrıntılı denetim](#0.1__Toc274034226)
    - [Yardımcıları alt çizgi için kısa çizgi anonim nesneleri kullanarak belirtilen HTML öznitelik adları için Dönüştür](#0.1__Toc274034227)
    - [Hata düzeltmeleri](#0.1__Toc274034228)
    - [Yeni değişiklikler](#0.1__Toc274034229)
    - [Bilinen Sorunlar](#0.1__Toc274034230)
- [Sorumluluk reddi](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Genel Bakış

Bu belgede, ASP.NET MVC 3 RTM sürümü Visual Studio 2010 için açıklanmaktadır. ASP.NET MVC Model-View-Controller (MVC) deseni kullanan Web uygulamaları geliştirmek için bir çerçevedir. ASP.NET MVC 3 Yükleyici aşağıdaki bileşenleri içerir:

- ASP.NET MVC 3 çalışma zamanı bileşenleri
- ASP.NET MVC 3 Visual Studio 2010 Araçları
- ASP.NET Web sayfaları çalışma zamanı bileşenleri
- ASP.NET Web sayfaları Visual Studio 2010 Araçları
- Microsoft .NET (NuGet) için Paket Yöneticisi
- Razor sözdizimi için destek sağlayan Visual Studio 2010 için bir güncelleştirme. (Ayrıntılar için Bilgi Bankası makalesi 2483190 bakın.)

ASP.NET MVC 3 her yayım öncesi sürümü için sürüm notları, tamamını şu URL'de ASP.NET Web sitesinde bulunabilir:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Yükleme notları

Web Platformu Yükleyicisi (Web PI) kullanarak ASP.NET MVC 3 RTM yüklemek için aşağıdaki sayfayı ziyaret edin:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternatif olarak, yükleyici ASP.NET MVC 3 RTM için Visual Studio 2010 için aşağıdaki sayfasından yükleyebilirsiniz:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 yüklenebilir ve yan yana çalıştırabilirsiniz ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET MVC 3 çalışma zamanı bileşenleri aşağıdaki yazılımı gerektirir:

- .NET framework sürüm 4. 

    ASP.NET MVC 3 Visual Studio 2010 Araçları aşağıdaki yazılımı gerektirir:
- Visual Studio 2010 veya Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

ASP.NET MVC için belgeleri aşağıdaki URL'yi adresindeki MSDN Web sitesinde kullanılabilir:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Öğreticiler ve diğer bilgileri ASP.NET MVC hakkında ASP.NET Web sitesinin şu URL'de MVC sayfasında mevcuttur:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Destek

Tam olarak desteklenen bir sürüme budur. Teknik destek alma hakkında daha fazla bilgi bulunabilir [Microsoft Support Web sitesi](https://support.microsoft.com/).

Ayrıca, ASP.NET topluluk üyeleri sık resmi olmayan desteği sağlamak mümkün olduğu ASP.NET MVC forumuna bu sürüm hakkında sorularınızı çekinmeyin:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Bir ASP.NET MVC 2 proje için ASP.NET MVC yükseltme 3 araçları güncelleştirme

ASP.NET MVC 3, ASP.NET MVC 3 ASP.NET MVC 2 uygulamaya yükseltme zamanı seçerken esneklik aynı bilgisayarda ASP.NET MVC 2 yan yana yüklenebilir.

El ile sürüm 3 varolan ASP.NET MVC 2 uygulamaya yükseltmek için aşağıdakileri yapın:

1. Yeni ve boş bir ASP.NET MVC 3 projesinin bilgisayarınızda oluşturun. Bu proje yükseltme için gereken bazı dosyaları içerir.
2. Aşağıdaki dosyalar ASP.NET MVC 3 projeden ASP.NET MVC 2 projenizin karşılık gelen konumuna kopyalayın. JQuery yeni dosya adı (jQuery-1.5.1.js) için hesap kitaplığa yönelik tüm başvuruları güncelleştirmeniz gerekir: 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /İçeriğe/Temalar/\*.\*
3. Kopya *paketleri* çözümün .sln dosyasını bulunduğu dizindir çözümünüzü kökündeki boş ASP.NET MVC 3 proje çözüme kök klasöründe.
4. ASP.NET MVC 2 projeniz alanları içeriyorsa, /Views/Web.config dosyaya Kopyala *görünümleri* her alanın klasör.
5. Her iki ASP.NET MVC 2 projenin Web.config dosyalarında, genel olarak aramak ve ASP.NET MVC sürüm değiştirin. Aşağıda bulabilirsiniz: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Aşağıdakiyle değiştirin:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Çözüm Gezgini'nde referansı silme *System.Web.Mvc* (hangi noktalarından dll sürüm 2), ardından bir başvuru ekleyin *System.Web.Mvc* (v3.0.0.0).
7. System.Web.WebPages.dll ve System.Web.Helpers.dll bir başvuru ekleyin. Bu derlemeler aşağıdaki klasörlerde bulunur: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. Çözüm Gezgini'nde proje adına sağ tıklayın ve projeyi seçin. Daha sonra yeniden proje adına sağ tıklayın ve Düzenle'yi seçin *ProjectName*.csproj.
9. Bulun *ProjectTypeGuids* öğesi ve {E53F8FEA-EAE0-44A6-8774-FFD645390401} {F85E285D-A4E0-4152-9332-AB1D724D3325} değiştirin.
10. Değişiklikleri kaydedin, projeye sağ tıklayın ve sonra projeyi yeniden yükle seçin.
11. Uygulamanın kök Web.config dosyasında aşağıdaki ayarlara eklemek *derlemeleri* bölümü. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. ASP.NET MVC 2 kullanılarak derlenmiş tüm üçüncü taraf kitaplıklar projeye başvuruda bulunuyorsa vurgulanmış aşağıdakileri ekleyin *bindingRedirect* altında uygulama kök Web.config dosyasında öğesine  *Yapılandırma* bölümü: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Değişiklikleri ASP.NET MVC 3 araçları güncelleştirme

Bu bölümde ASP.NET MVC 3 araçları güncelleştirme sürümde ASP.NET MVC 3 RTM sürümünden bu yana yapılan değişiklikleri açıklar.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>"Denetleyici Ekle" iletişim kutusu artık görünümleri ve veri erişim kodu denetleyicileriyle iskele

Yapı iskelesi hızlı bir şekilde bir denetleyici ve uygulamanız için görünümleri oluşturmanın bir yoludur. Kod oluşturulduktan sonra projenizin gereksinimlerini karşılayacak şekilde düzenleyebilirsiniz.

Başlatmak için *denetleyici Ekle* ASP.NET MVC 3, iletişim kutusunda sağ *denetleyicileri* klasöründe *Çözüm Gezgini*, tıklatın *Ekle*ve ardından *denetleyicisi*. İletişim kutusunda, ek yapı iskelesi seçenekleri sunmak için geliştirilmiştir.

![](mvc3-release-notes/_static/image1.png)

Varsayılan olarak kullanılabilir üç yapı iskelesi şablonu yok.

#### <a name="empty-controller"></a>Boş denetleyicisi

Bu şablon boş denetleyicisi dosyası oluşturur. Bu şablon değil denetimini eşdeğerdir *oluşturma, düzenleme, Ayrıntılar için Eylemler eklemek, senaryoları silmek* ASP.NET MVC, önceki sürümlerinde. Bu seçerseniz, başka hiçbir seçenek kullanılabilir.

#### <a name="controller-with-empty-readwrite-actions"></a>Boş okuma/yazma eylemleri ile denetleyicisi

Bu şablon tüm gerekli eylem yöntemleri ancak hiçbir uygulama kodu yöntemlere sahip bir denetleyici dosyası oluşturur. Bu şablon denetimini eşdeğerdir *oluşturma, düzenleme, Ayrıntılar için Eylemler eklemek, senaryoları silmek* ASP.NET MVC, önceki sürümlerinde. Bu seçerseniz, başka hiçbir seçenek kullanılabilir.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Okuma/yazma eylemleri ve görünümleri, Entity Framework kullanarak denetleyiciyle

Bu şablon, bir çalışma veri girişi kullanıcı arabirimi hızlı bir şekilde oluşturmanızı sağlar. Bir dizi ortak gereksinimleri ve aşağıdaki gibi senaryoları işleyen kodu oluşturur:

- *Veri erişimi*. Oluşturulan kod okur ve varlık bir veritabanına yazar. Varolan bir veri bağlamı sınıfı seçin veya yeni bir şablon izin verirseniz Entity Framework Code First yaklaşımda çalışan *DbContext* sınıfı. Ayrıca varolan bir seçerseniz Entity Framework veritabanı First veya Model First yaklaşımda çalışır *ObjectContext* sınıfı.
- *Doğrulama*. Oluşturulan kod ASP.NET MVC model bağlama ve meta veri özellik kullanır, böylece form gönderme modeli sınıfınız bildirilen kurallarına göre doğrulanır. Bu gibi yerleşik doğrulama kurallar içerir *gerekli* ve *StringLength* öznitelikler ve özel doğrulama kuralları.
- *Bir-çok ilişkileri*. Model sınıflarınızı arasında bir çok yabancı anahtar ilişkileri tanımlarsanız, oluşturulan kod ilgili varlıklar seçmek için aşağı açılır listeler oluşturur. Örneğin, Entity Framework Code First kurallarına aşağıdaki modeli sınıflar tanımlayabilir: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Ne zaman, ardından iskele bir denetleyici için *ürün* sınıfı, görünümlerini izni verdiği seçmelerini bir *kategori* her nesne *ürün* örneği.

    Bu şablon ek seçenekler sağlar *denetleyici Ekle* iletişim kutusu. İçin *Model sınıfı*, kullanıcılar oluşturmak veya düzenlemek için veri türünü belirleyen çözümünüzde, herhangi bir model sınıfı seçebilirsiniz:
- Entity Framework Code First kullanmak istiyorsanız, herhangi bir model sınıfı seçebilirsiniz.
- Entity Framework veritabanı First veya Entity Framework Model First kullanıyorsanız, kavramsal modelde tanımlanan bir varlık sınıfı seçmenizi emin olun.

İçin *veri bağlamı sınıfı*, bu seçimler yapabilirsiniz:

- Sınıfı, seçin varolan bir veri bağlamı Code First kullanıyorsanız ve istiyorsanız ** yeni veri bağlamı **. Bir veri bağlamı sınıfı, ardından sizin için oluşturulur.
- Code First kullanmasına ve varolan bir veri bağlamı sınıfı sahip olmasını istiyorsanız, burada seçin. Seçtiğiniz model sınıfı kalıcı hale getirmek için güncelleştirilir.
- Database First veya Model First kullanıyorsanız, nesne bağlamı sınıfı seçin.

Görünümleri için kullanmak istediğiniz görünüm altyapısı seçin veya herhangi bir görünüm iskele istemiyorsanız hiçbiri seçin.

Gelişmiş Optionsto seçebileceğiniz daha fazla üretilmiş görünümleri seçeneklerini belirtin. Örneğin, düzeni veya kullanmak için ana sayfa seçebilirsiniz.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>İyileştirmeler "ASP.NET MVC 3 yeni proje" iletişim kutusu

Yeni ASP.NET MVC 3 proje oluşturmak için kullandığınız iletişim kutusu, aşağıda listelenen şekilde birden çok geliştirmeleri içerir.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Yeni "Intranet proje" şablonu

Yeni bir Intranet uygulaması Şablonu proje şablonu listesi içerir. Bu şablon, form kimlik doğrulaması yerine Windows kimlik doğrulaması kullanan bir web uygulaması oluşturmaya yönelik ayarları içerir. Bir intranet uygulaması bir proje şablonu kapsüllenmiş bazı IIS ayarlarını gerektirdiğinden, şablon IIS'de iş proje şablonu yapma hakkında yönergeler içeren bir benioku dosyası içerir. Belgelerine şu URL'de MSDN Web sitesinde yeni bir Intranet uygulaması şablonu kullanılabilir:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Proje şablonları etkin HTML5 sunulmuştur

Yeni Proje iletişim kutusu artık proje şablonları HTML5 özgü özellikleri eklemek için bir seçenek içerir. Neden olan Yeni HTML5 içeren oluşturulacak görünümleri seçeneğini belirleyerek `<header>`, `<footer>`, ve `<navigation>` öğeleri.

Tarayıcıların önceki sürümleri HTML5 özgü etiketler desteklemeyen unutmayın. Bu sınırlama adres için Modernizr kitaplığına bir başvuru HTML5 proje şablonları içerir. (Sonraki bölüme bakın.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Proje şablonları artık Modernizr 1.7 içerir

Modernizr henüz bu özellikleri desteklemeyen tarayıcılarda CSS 3 ve HTML5 için destek sağlayan bir JavaScript kitaplıktır. Bu kitaplık şablonları ASP.NET MVC 3 projelerinin önceden yüklenmiş bir NuGet paketi olarak dahil edilir. Modernizr hakkında daha fazla bilgi için bkz: [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Proje şablonları içerir jQuery, jQuery UI ve jQuery güncelleştirilmiş sürümleri doğrulama

Proje şablonları artık jQuery komut aşağıdaki sürümleri şunlardır:

- jQuery 1.5.1
- jQuery doğrulama 1,8
- jQuery UI 1.8.11

Bu kitaplıklar önceden yüklenmiş NuGet paketleri dahil edilir.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Proje şablonları, ADO.NET Entity Framework 4.1 artık önceden yüklenmiş bir NuGet paketi olarak içerir.

ADO.NET Entity Framework 4.1 Code First özelliği içerir. Önce var olan veritabanı ilk ve Model First desenler için bir alternatif sağlayan bir ADO.NET Entity Framework için yeni bir geliştirme deseni kodudur.

Kod önce Visual Basic veya C# ile yazılmış POCO sınıfları ("düz eski CLR nesnelerini") kullanarak modelinizi tanımlama geçici odaklanmıştır. Bu sınıfların varolan bir veritabanına eşlenebilir veya bir veritabanı şeması oluşturmak için kullanılabilir. Ek yapılandırma kullanarak tarafından sağlanan *DataAnnotations* öznitelikleri veya fluent API kullanılarak.

Kod Firstwith ASP.NET MVC kullanarak belgelerine ASP.NET Web sitesinde aşağıdaki URL'ler edinilebilir:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Proje şablonları JavaScript kitaplıklarını önceden yüklenmiş NuGet paketleri olarak içerir.

Yeni bir ASP.NET MVC 3 projesi oluşturduğunuzda, proje daha önce bahsedilen JavaScript dosyaları (örneğin, Modernizr kitaplık) bunları yükleyerek içeriyor doğrudan komut dosyalarını proje şablonu betikler klasörüne eklemek yerine NuGet kullanma içeriği. Bu komut dosyalarını yeni sürümleri yayımlandığında komut dosyalarını en son sürüme güncelleştirmek için NuGet kullanmanıza olanak sağlar.

Örneğin, yeni jQuery sürümler sıklığını göz önüne alındığında, proje şablona dahil jQuery sürümü belirli bir noktada güncel olacaktır. JQuery yüklü bir NuGet paketi olarak dahil olduğundan, jQuery daha yeni sürümlerinde kullanılabilir olduğunda ancak, size NuGet iletişim kutusunda bildirilecek.

JQuery dosya adında sürüm numarasını içerdiğinden, jQuery en son sürüme güncelleştirilmesi de güncelleştirilmesini gerektirir `<script>` yeni dosya adını kullanmak için jQuery dosyaya başvuruda bulunan etiketi. En son sürümlerine daha kolay güncelleştirilebilmesi için diğer dahil betik kitaplıkları betik adı sürüm numarası dahil etmeyin.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- Bazı durumlarda, hata iletisi "yükleme (0x80070643) hata koduyla başarısız oldu" yükleme başarısız olabilir. Bu soruna geçici bir çözüm hakkında daha fazla bilgi için bkz: [Bilgi Bankası makalesi 2531566](https://support.microsoft.com/kb/2531566).
- Bir denetleyici eklemek için yapı iskelesi varlık devralma Entity Framework içinde desteğinden varlıklar için iskele değil. Örneğin, bir taban verilen *kişi* tarafından devralınır sınıfı bir *Öğrenci* sınıfı, iskele *Öğrenci* sınıfı derlenmez oluşturulan kodda neden olur.
- Neden bir çözüm klasördeki yeni bir ASP.NET MVC 3 projesi oluşturma bir *NullReferenceException* hata. ASP.NET MVC 3 projesinin çözümü kök dizininde oluşturmak ve ardından çözüm klasörüne taşımak için geçici bir çözüm değildir.
- ReSharper yüklendiğinde Razor sözdizimi için IntelliSense çalışmaz. Yüklü ReSharper varsa ve ASP.NET MVC 3'te Razor IntelliSense desteği yararlanmak istediğiniz girişine bakın [Razor IntelliSense ve ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri'nın blogunda saptanacağını bunları birlikte bugün kullanmanın yolları.
- Yükleme sırasında EULA kabul iletişim kutusunda Lisans Koşulları'nı istenenden daha küçük bir pencerede görüntüler.
- Ne zaman düzenlemekte Razor Görünüm (.cshtml veya. *vbhtml* dosyası), görünüm. ASP.NET MVC 3 herhangi parçacıkları Razor görünümleri için içermez... ASP.NET MVC için bir kod parçacığı aspxselecting için parçacıkları gösterir
- ASP.NET MVC 3 Visual Web Developer Express için Visual Studio yüklü olduğu bir bilgisayara yükleyin ve Visual Studio daha sonra yüklemek, ASP.NET MVC 3 yeniden yüklemeniz gerekir. Visual Studio ve Visual Web Developer Express ASP.NET MVC 3 yükleyici tarafından yükseltilir bileşenleri paylaşır. ASP.NET MVC 3, Visual Studio için Visual Web Developer Express sahip ve ardından Visual Web Developer Express yükleyin bir bilgisayara yüklerseniz, aynı sorun geçerlidir.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>ASP.NET MVC 3 RTM değişiklikleri

Bu bölümde, değişiklikler ve hata düzeltmeleri RC2 sürümünden bu yana ASP.NET MVC 3 RTM sürümünde yapılan açıklanmaktadır.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Değişikliği: jQuery UI sürümü 1.8.7 için güncelleştirilmiştir.

Visual Studio için ASP.NET MVC proje şablonları, jQuery UI kitaplığının en son sürümünü içerecek şekilde güncelleştirildi. Şablonlar, kaynak dosyaları jQuery UI ilişkili CSS ve görüntü dosyaları gibi gerekli en az sayıda de içerir.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Değişikliği: ModelMetadataProvider geri DataAnnotationsModelMetadataProvider için varsayılan değiştirildi

ASP.NET MVC 3 sunulan RC2 sürümünü bir *CachedDataAnnotationsMetadataProvider* sınıfı, sağlanan mevcut en üstünde önbelleğe alma *DataAnnotationsModelMetadataProvider* olarak sınıf bir performans geliştirmesi. Ancak, değişikliği geri ve şu adresten edinilebilir MVC vadeli projesine taşınmış bazı hatalar bu uygulamasıyla raporlandı [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Sabit: boşluk sonuçlarda tersine içeren bir Razor ifade parçası yapıştırma

Bir Razor dosyasına boşluk içeren bir Razor ifadenin bir parçası yapıştırın, ASP.NET MVC 3'ün yayın öncesi sürümlerde elde edilen ifadesi ters çevrilir. Örneğin, aşağıdaki Razor kod bloğu göz önünde bulundurun:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Bu durumda metni "bir ilk param" ilk yöntemi seçerseniz ve ikinci yönteme bir bağımsız değişken olarak yapıştırın, sonuç aşağıdaki gibidir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Yapıştırma işlemi aşağıdaki sonuç olduğunu doğru davranıştır:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Böylece ifade doğru yapıştırma işlemi sırasında korunur RTM sürümünde bu sorun düzeltilmiştir.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Sabit: düzenleyicide açılan bir Razor dosyasını yeniden adlandırma söz dizimi renklendirme ve IntelliSense devre dışı bırakır

Çözüm Gezgini kullanırken, dosya Düzenleyicisi penceresinde açılmış bir Razor dosyasını yeniden adlandırma, sözdizimi vurgulama ve IntelliSense bu dosya için çalışmayı durdurmasına neden olur. Bu vurgulama böylece düzeltilmiştir ve IntelliSense, yeniden adlandırma işleminden sonra güncelleştirilir.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- NuGet Paket Yöneticisi konsolu açıkken Visual Studio 2010 SP1 Beta kapatırsanız, Visual Studio kilitleniyor ve yeniden başlatmayı dener. Bu sabit Visual Studio 2010 SP1 RTM sürümünde.
- ASP.NET MVC 3 yükleyici yalnızca NuGet Paket Yöneticisi ilk bir sürümünü yükleyebilirsiniz. İlk sürüm yükledikten sonra NuGet yüklenebilir ve Visual Studio Uzantı Yöneticisi'ni kullanarak güncelleştirilir. Yüklü NuGet zaten varsa, NuGet en son sürüme güncelleştirmek için Visual Studio uzantısı galerisine gidin.
- Neden bir çözüm klasördeki yeni bir ASP.NET MVC 3 projesi oluşturma bir *NullReferenceException* hata. ASP.NET MVC 3 projesinin çözümü kök dizininde oluşturmak ve ardından çözüm klasörüne taşımak için geçici bir çözüm değildir.
- Yükleyici tamamlamak için ASP.NET MVC önceki sürümlerinden daha uzun sürebilir. Visual Studio 2010 'un bileşenleri güncelleştirir olmasıdır.
- ReSharper yüklendiğinde Razor sözdizimi için IntelliSense çalışmaz. Yüklü ReSharper varsa ve ASP.NET MVC 3'te Razor IntelliSense desteği yararlanmak istediğiniz girişine bakın [Razor IntelliSense ve ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri'nın blogunda saptanacağını bunları birlikte bugün kullanmanın yolları.
- ASP.NET MVC 3'in Beta sürümüyle oluşturulmuş CCSHTML ve VBHTML görünümleri kendi yapı eylemi doğru olarak ayarlanmış sahip değilse, proje yayımlandığında bu görüntülemek sonuçla türleri göz ardı edilir. Bu dosyalar için yapı eylemi değeri "İçerik" olarak ayarlanmalıdır. ASP.NET MVC 3 RTM, yeni dosyalar için bu sorunu giderir ancak yayın öncesi sürümleriyle oluşturulan bir proje için var olan dosyaların ayarını düzeltmesini değil.
- ![](mvc3-release-notes/_static/image3.png)
- Yükleme sırasında EULA kabul iletişim kutusunda Lisans Koşulları'nı istenenden daha küçük bir pencerede görüntüler.
- Razor Görünüm (.cshtml dosyası) düzenlerken, Visual Studio denetleyicisi Git menü öğesi kullanılamaz ve hiçbir kod parçacıkları vardır.
- ASP.NET MVC 3 Visual Web Developer Express için Visual Studio yüklü olduğu bir bilgisayara yükleyin ve Visual Studio daha sonra yüklemek, ASP.NET MVC 3 yeniden yüklemeniz gerekir. Visual Studio ve Visual Web Developer Express ASP.NET MVC 3 yükleyici tarafından yükseltilir bileşenleri paylaşır. ASP.NET MVC 3, Visual Studio için Visual Web Developer Express sahip ve ardından Visual Web Developer Express yükleyin bir bilgisayara yüklerseniz, aynı sorun geçerlidir.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Yeni Değişiklikler

- ASP.NET MVC önceki sürümlerinde, filtreleri eylem bazı durumlar dışında istekte başına oluşturun. Bu davranış hiçbir zaman garantili davranışı ancak yalnızca bir uygulama ayrıntılarını ve sözleşme filtreleri için durum bilgisiz dikkate alınması gereken olmuştur. ASP.NET MVC 3'te filtreleri daha agresif önbelleğe alınır. Bu nedenle, örnek durumu yanlış depolayan herhangi bir özel eylem filtre bozuk olabilir.
- Özel durum filtreleri için yürütme sırasını aynı olan özel durum filtreleri değişti *sipariş* değeri. ASP.NET MVC 2 ve daha önceki sürümlerde, aynı olan denetleyicisinde özel durum filtreleri *sipariş* değeri gibi bir eylem yönteminin üzerindekiler eylem yöntemi özel durum filtreleri önce yürütülür. Özel durum filtreleri uygulandığında bu durum genellikle olacaktır belirtilen olmadan *sipariş* değeri. Böylece en belirli özel durum işleyici ilk yürütür ASP.NET MVC 3'te bu sırasını tersine çevrildi. Önceki sürümlerinde olduğu gibi *sipariş* özelliği açıkça belirtilen, filtreler belirtilen sırada çalıştırılır.
- Adlı yeni bir özellik *FileExtensions* eklendi *VirtualPathProviderViewEngine* temel sınıfı. ASP.NET (Ada göre değil) yoluyla bir görünüm göründüğünde, bu yeni özelliği tarafından belirtilen listesinde yer alan bir dosya uzantısı yalnızca görünümlerle olarak kabul edilir. Bir özel dosya uzantısı Web Form görünümleri için etkinleştirmek için bir özel derleme sağlayıcısı kayıtlı olduğu ve bir adı yerine bir tam yol kullanarak bu görünümler sağlayıcı başvuran uygulamalarında önemli bir değişiklik budur. Değerini değiştirmek için geçici bir çözüm değildir *FileExtensions* özel dosya uzantısını eklemeyi özelliği.
- Doğrudan uygulayan özel denetleyici üreteci uygulamaları *IControllerFactory* arabirimi, yeni bir uygulama sağlamalıdır *GetControllerSessionBehavior* edildi yöntemi Bu sürümde arabirimi eklendi. Genel olarak, bu, değil doğrudan bu arabirimi uygulayan ve bunun yerine, sınıfından türetilen önerilir *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>ASP.NET MVC 3 RC2 değişiklikleri

Bu bölümde ASP.NET MVC 3 RC2 sürümde RC sürümünden bu yana yapılan değişiklikler (yeni özellikleri ve hata düzeltmelerini) açıklanmaktadır.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Proje şablonları jQuery 1.4.4, jQuery doğrulama 1.7 ve jQuery UI 1.8.6 içerecek şekilde değiştirildi.

ASP.NET MVC 3 proje şablonları artık jQuery, jQuery doğrulama ve jQuery en son sürümleri dahil kullanıcı Arabirimi. jQuery UI yeni bir proje şablonları eklemeyi ve yararlı kullanıcı arabirimi pencere öğeleri sağlar. JQuery UI hakkında daha fazla bilgi için kendi giriş sayfasını ziyaret edin: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Eklenen "AdditionalMetadataAttribute" sınıfı

Kullanabileceğiniz *AdditionalMetadataAttribute* doldurmak için sınıf *ModelMetadata.AdditionalValues* model özelliğine sözlüğü.

Örneğin, bir görünüm modeli yalnızca yönetici görüntülenmesi gereken özellikleri olduğunu varsayalım. Bu model AdminOnly aşağıdaki örnekteki gibi bir değer olarak anahtar ve true kullanarak yeni özniteliğiyle ek açıklama:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Bir ürün görünüm modeli işlendiğinde bu meta veriler herhangi bir görüntü veya Düzenleyicisi şablonu için kullanılabilir hale getirilir. Bu size, meta veri bilgilerini yorumlamak için uygulama geliştiricisi olarak bağlıdır.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Geliştirilmiş görünüm yapı İskelesi

Şablon yardımcı yöntemler çağrıları gibi yapı iskelesi görünümleri için kullanılan T4 şablonları oluşturmak *EditorFor* gibi Yardımcıları yerine *TextBoxFor*. Görünüm Ekle iletişim kutusu bir görünümünü oluşturur, bu değişikliği veri ek açıklaması öznitelikleri biçiminde model meta verilerini desteği artırır.

Görünüm Ekle yapı iskelesi geliştirilmiş bir algılama ve kullanım kuralına göre modeli, birincil anahtar bilgileri de içerir. Örneğin, Görünüm Ekle iletişim kutusu, birincil anahtar değerine düzenlenebilir form alanı olarak iskele kurulmuş değil emin olmak için bu bilgileri kullanır.

Varsayılan düzenleme ve oluşturma şablonları için istemci doğrulama gerekli jQuery betikleri başvurular içerir.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Eklenen Html.Raw yöntemi

Varsayılan olarak, Razor altyapısı HTML olarak kodlar tüm değerleri görüntüleyin. Örneğin, böylece sayfa olarak görüntülenir aşağıdaki kod parçacığını Tebrik değişkeni içinde HTML kodlar `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Yeni *Html.Raw* yöntemi güvenli olması için içerik bilindiğinde kodlanmamış HTML görüntüleme, basit bir yol sağlar. Aşağıdaki örnek aynı dize görüntüler, ancak dize biçimlendirme olarak işlenir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Yeniden adlandırılmış "Controller.ViewModel" özelliğini ve "Görünüm"paketi "Görünüm" özelliğini

Daha önce *ViewModel* özelliği *denetleyicisi* için corresponded *Görünüm* bir görünüm özelliği. Bu özelliklerin her ikisi de erişim değerleri için bir yol sağlamak *ViewDataDictionary* dinamik özellik erişimcisini sözdizimini kullanarak nesne. Her iki özellik aynı Karışıklığı önlemek için ve daha tutarlı olarak adlandırılmıştır.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>"SessionStateAttribute" olarak yeniden adlandırıldı "ControllerSessionStateAttribute" sınıfı

*ControllerSessionStateAttribute* sınıf, ASP.NET MVC 3 RC sürümünde tanıtılmıştır. Özelliği, daha kısa olacak şekilde değiştirildi.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Yeniden adlandırılmış RemoteAttribute "Alanlar" özelliği "AdditionalFields"

*RemoteAttribute* sınıfının *alanları* özelliği kullanıcılar arasında bazı karışıklığa neden oldu. Bu özellik için yeniden adlandırma *AdditionalFields* kendi amacı açıklar.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>"AllowHtmlAttribute" için "SkipRequestValidationAttribute" yeniden adlandırıldı

*SkipRequestValidationAttribute* özniteliği adlandırıldı *AllowHtmlAttribute* daha iyi hedeflenen kullanım temsil etmek için.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>İlk yararlı hata iletisini görüntülemek üzere değiştirilen "Html.ValidationMessage" yöntemi

*Html.ValidationMessage* yöntemi sabit yalnızca ilk hata görüntüleme yerine ilk yararlı hata iletisini görüntülemek için.

Model bağlama sırasında *ModelState* sözlük modelden dahil olmak üzere bu özellik hakkında hata iletileri ile birden fazla kaynaktan doldurulabilir (bunu uygularsa *IValidatableObject* ), özellik erişilen sırasında oluşturulan özel durumları ve özelliğine uygulanan doğrulama öznitelikleri.

Zaman *Html.ValidationMessage* yöntemi bir doğrulama iletisi görüntüler, çünkü bunlar genellikle son kullanıcı için amaçlanmamıştır bir özel durum içeren model durumu girişleri atlar. Bunun yerine, yöntemi bir özel durum ile ilişkili değil ve bu iletiyi görüntüleyen bir ilk doğrulama ileti görünür. Bu tür bir ileti bulunursa, ilk özel durum ile ilişkili genel bir hata iletisi varsayılan olarak ayarlar.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Sabit @model belgeye boşluk eklememeyi bildirimi

Önceki sürümlerde <em>@model</em> bildiriminin en üstünde bir görünümün işlenmiş olan HTML çıktısı için boş bir satır eklenir. Böylece bildirimi boşluk sunmaz bu düzeltilmiştir.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Görünüm altyapıları altyapısı özgü dosya adlarını desteklemek için eklenen "FileExtensions" özelliği

Bir görünüm altyapısı aşağıdaki örnekteki gibi bir açık görünüm yolunu kullanarak görünüm döndürebilirsiniz:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

İlk Görünüm altyapısı görünüm işlemek her zaman çalışır. Varsayılan olarak, Web Forms görünüm altyapısı ilk görünüm altyapısıdır; Web formları altyapısı Razor görünüm işlenemez nedeniyle bir hata oluşur. Görünüm altyapısı artık sahip bir *FileExtensions* destekledikleri hangi dosya uzantılarını belirtmek için kullanılan özellik. Bu özellik, ASP.NET bir görünüm altyapısı dosya işlenip işlenmeyeceğini belirler denetlenir. Bu önemli bir değişiklik ve daha fazla ayrıntı dahil edilen [son değişiklikler](#_Toc2_BC) bu belgenin bölüm.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Sabit "LabelFor" Yardımcısı "İçin" özniteliği için geçerli bir değer Yayma

Bir hata where sabit *LabelFor* işlenen yöntemi bir *için* eşleşen öznitelik *giriş* öğenin *adı* yerine özniteliği kimliğini W3C göre *için* özniteliği eşleşmelidir *giriş* öğenin kimliği.

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Model bağlama sırasında açık değerleri öncelik vermek için sabit "RenderAction" yöntemi

Önceki sürümlerde için geçirilmiş açık değerler *RenderAction* yöntemi yoksaydı geçerli form değerleri yönelik bir alt eylem içinde model bağlama sırasında. Açık değerler önceliklidir model bağlama sırasında düzeltme sağlar.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Yeni Değişiklikler

- ASP.NET MVC önceki sürümleri, bazı durumlarda dışında istek başına eylem filtreleri oluşturuldu. Bu davranış hiçbir zaman garantili davranışı ancak yalnızca bir uygulama ayrıntılarını ve sözleşme filtreleri için durum bilgisiz dikkate alınması gereken olmuştur. ASP.NET MVC 3'te filtreleri daha agresif önbelleğe alınır. Bu nedenle, örnek durumu yanlış depolayan herhangi bir özel eylem filtre bozuk olabilir.
- Özel durum filtreleri için yürütme sırasını aynı olan özel durum filtreleri değişti *sipariş* değeri. ASP.NET MVC 2 ve daha önceki sürümlerde, aynı olan denetleyicisine özel durum filtreleri *sipariş* değeri gibi bir eylem yönteminin üzerindekiler eylem yöntemi özel durum filtreleri önce yürütüldü. Özel durum filtreleri uygulandığında bu durum genellikle olacaktır belirtilen olmadan *sipariş* değeri. Böylece en belirli özel durum işleyici ilk yürütür ASP.NET MVC 3'te bu sırasını tersine çevrildi. Önceki sürümlerinde olduğu gibi *sipariş* özelliği açıkça belirtilen, filtreler belirtilen sırada çalıştırılır.
- Adlı yeni bir özellik *FileExtensions* eklendi *VirtualPathProviderViewEngine* temel sınıfı. ASP.NET (Ada göre değil) yoluyla bir görünüm göründüğünde, bu yeni özelliği tarafından belirtilen listesinde yer alan bir dosya uzantısı yalnızca görünümlerle olarak kabul edilir. Bir özel dosya uzantısı Web Form görünümleri için etkinleştirmek için bir özel derleme sağlayıcısı kayıtlı olduğu ve bir adı yerine bir tam yol kullanarak bu görünümler sağlayıcı başvuran uygulamalarında önemli bir değişiklik budur. Değerini değiştirmek için geçici bir çözüm değildir *FileExtensions* özel dosya uzantısını eklemeyi özelliği.
- Doğrudan uygulayan özel denetleyici üreteci uygulamaları <em>IControllerFactory</em> arabirimi, yeni bir uygulama sağlamalıdır <em>GetControllerSessionBehavior</em>  <em>Bu sürümde arabirimi eklendi yöntemi</em>. Genel olarak, bu, değil doğrudan bu arabirimi uygulayan ve bunun yerine, sınıfından türetilen önerilir <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- ASP.NET MVC 3 yükleyici yalnızca NuGet Paket Yöneticisi ilk bir sürümünü yükleyebilirsiniz. İlk sürüm yükledikten sonra NuGet yüklenebilir ve Visual Studio Uzantı Yöneticisi'ni kullanarak güncelleştirilir. Yüklü NuGet zaten varsa, NuGet en son sürüme güncelleştirmek için Visual Studio uzantısı galerisine gidin.
- Neden bir çözüm klasördeki yeni bir ASP.NET MVC 3 projesi oluşturma bir *NullReferenceException* hata. ASP.NET MVC 3 projesinin çözümü kök dizininde oluşturmak ve ardından çözüm klasörüne taşımak için geçici bir çözüm değildir.
- Yükleyici tamamlamak için ASP.NET MVC önceki sürümlerinden daha uzun sürebilir. Visual Studio 2010 'un bileşenleri güncelleştirir olmasıdır.
- ReSharper yüklendiğinde Razor sözdizimi için IntelliSense çalışmaz. Yüklü ReSharper varsa ve ASP.NET MVC 3 RC2 Razor IntelliSense desteği yararlanmak istediğiniz girişine bakın [Razor IntelliSense ve ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri'nın blogunda saptanacağını bunları birlikte bugün kullanmanın yolları.
- ASP.NET MVC 3 Beta sürümü ile oluşturulan CSHTML ve VBHTML görünümleri kendi yapı eylemi doğru olarak ayarlanmış sahip değilse, proje yayımlandığında bu görüntülemek sonuçla türleri göz ardı edilir. *Yapı eylemi* bu dosyaları içeriği ayarlanmalıdır için bir değer ". ASP.NET MVC 3 RC2 yeni dosyalar için bu sorunu giderir ancak Beta sürümüyle oluşturulmuş bir proje için var olan dosyaların ayarını düzeltmesini değil.![](mvc3-release-notes/_static/image4.png)
- Yükleme sırasında EULA kabul iletişim kutusunda Lisans Koşulları'nı istenenden daha küçük bir pencerede görüntüler.
- Razor Görünüm (.cshtml dosyası) düzenlerken, Visual Studio denetleyicisi Git menü öğesi kullanılamaz ve hiçbir kod parçacıkları vardır.
- ASP.NET MVC 3 Visual Web Developer Express için Visual Studio yüklü olduğu bir bilgisayara yükleyin ve Visual Studio daha sonra yüklemek, ASP.NET MVC 3 yeniden yüklemeniz gerekir. Visual Studio ve Visual Web Developer Express ASP.NET MVC 3 yükleyici tarafından yükseltilir bileşenleri paylaşır. ASP.NET MVC 3, Visual Studio için Visual Web Developer Express sahip ve ardından Visual Web Developer Express yükleyin bir bilgisayara yüklerseniz, aynı sorun geçerlidir.
- Yüklü zaten varsa, ASP.NET MVC 3 RC 2'yi yükleme NuGet güncelleştirmez. NuGet yükseltme, Visual Studio Uzantı Yöneticisi'ni ve onu gitmek için kullanılabilir bir güncelleştirme olarak gösterilmesi gerekir. NuGet buradan en son sürüme yükseltebilirsiniz.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Sürüm Adayı

ASP.NET MVC Sürüm Adayı 9 Kasım 2010'da yayımlanmıştır.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>ASP.NET MVC 3 RC yeni özellikler

Bu bölümde tanıtılmıştır özellikleri açıklanmıştır ASP.NET MVC 3 RC sürümünden itibaren Beta sürümü.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet Paket Yöneticisi

ASP.NET MVC 3 NuGet paket (önceki adıyla NuPack bilinir), kitaplıklar ve araçlar Visual Studio projelerine ekleme bir tümleşik paket yönetim aracı olan Yöneticisi içerir. Bu araç, geliştiriciler bugün kendi kaynak ağacına bir kitaplık almak için attığınız adımlar otomatikleştirir.

Bir komut satırı aracı olarak, Visual Studio 2010 içinde tümleşik konsol penceresi Visual Studio bağlam menüsünden ve PowerShell cmdlet'leri kümesi olarak NuGet ile çalışabilirsiniz.

NuGet hakkında daha fazla bilgi için okuma [Nuget belgelerine](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>"Yeni Proje" Gelişmiş iletişim kutusu

Yeni bir proje oluşturduğunuzda, yeni proje iletişim kutusu artık görünüm altyapısının yanı sıra bir ASP.NET MVC proje türü belirtmenize olanak sağlar.

![](mvc3-release-notes/_static/image5.png)

Bu sürümde şablonları ve görünüm altyapıları iletişim kutusunda listelenen listesini değiştirmek için destek eklenmiştir.

Varsayılan Şablonlar aşağıda verilmiştir:

Boş. Varsayılan dizin yapısı ASP.NET MVC projeleri, ASP.NET MVC stilleri varsayılan ve varsayılan JavaScript dosyaları içeren bir komut dizini içeren bir Site.css dosyası dahil olmak üzere bir ASP.NET MVC proje dosyalarını en az bir kümesini içerir.

Internet uygulaması. Üyelik sağlayıcısının ASP.NET MVC ile nasıl kullanılacağını gösteren örnek işlevselliği içerir.

İletişim kutusunda görüntülenen proje şablonları listesini Windows Kayıt Defteri'nde belirtilir.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Oturumsuz denetleyicileri

Yeni *ControllerSessionStateAttribute* belirterek denetleyicileri için oturum durumu davranışı üzerinde daha fazla denetim sağlar bir [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) numaralandırma değeri.

Aşağıdaki örnek, bir denetleyici için tüm istekler için oturum durumu devre dışı bırakmak üzere gösterilmiştir.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Aşağıdaki örnek, salt okunur oturum durumunun tüm istekler için bir denetleyiciye nasıl ayarlanacağı gösterir.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Yeni doğrulama öznitelikleri

#### <a name="compareattribute"></a>CompareAttribute

Yeni *CompareAttribute* doğrulama özniteliği bir modelin iki farklı özelliklerinin değerlerini karşılaştırmanıza olanak sağlar. Aşağıdaki örnekte, *ComparePassword* özelliği eşleşmelidir *parola* geçerli olması için alan.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Yeni *RemoteAttribute* doğrulama özniteliği gerçek Doğrulama mantığı gerçekleştirir sunucuda bir yöntemi çağırmak istemci tarafı doğrulamasını etkinleştirir jQuery doğrulama eklentinin uzaktan Doğrulayıcı avantajlarından yararlanır.

Aşağıdaki örnekte, *kullanıcıadı* özelliğine sahip *RemoteAttribute* uygulanır. Bu özellik düzenleme Görünümü'ndeki düzenlerken, istemci doğrulama adlı bir eylem çağrısı *UserNameAvailable* üzerinde *UsersController* Bu alan doğrulamakta sınıfı.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Aşağıdaki örnek, ilgili denetleyicisi gösterir.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Varsayılan olarak, öznitelik uygulandığı özellik adı eylem yöntemine bir sorgu dizesi parametresi olarak gönderilir.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>"LabelFor" ve "LabelForModel" yöntemleri için yeni aşırı yüklemeleri

Yeni aşırı yüklemeleri için eklenmiştir *LabelFor* ve *LabelForModel* olanak tanıyan yöntemler etiket metni belirtin. Aşağıdaki örnek, bu aşırı kullanmayı gösterir.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Alt eylem çıktı önbelleği

*OutputCacheAttribute* kullanarak adında alt eylemlerin çıktı önbelleği destekleyen *Html.RenderAction* veya *Html.Action* yardımcı yöntemler. Aşağıdaki örnek, başka bir eylem çağıran bir görünümü gösterir.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate* eylem açıklama ile *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Bu kod çalıştığında, Html.Action("GetDate") çağrısı sonucu 100 saniye için önbelleğe alınır.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"Görünümü Ekle" iletişim kutusunu geliştirmeleri

Kesin türü belirtilmiş görünüm eklediğinizde, Görünüm Ekle iletişim kutusu artık önceki sürümlerde, çok sayıda çekirdek .NET Framework türleri gibi daha fazla geçerli olmayan türlerini filtreler. Ayrıca, liste şimdi sınıf adı ve değil türleri bulmayı kolaylaştırır tam olarak nitelenmiş tür adını göre sıralanır. Örneğin, tür adı şimdi aşağıdaki örnekte olduğu gibi görüntülenir:

ClassName (ad alanı)

Önceki sürümlerde, bunu aşağıdaki gibi görüntülendi:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Ayrıntılı istek doğrulama

*Hariç* özelliği *ValidateInputAttribute* artık yok. Bunun yerine, belirli özelliklerini bir model için model bağlama sırasında atlandı istek doğrulama için yeni kullanın *SkipRequestValidationAttribute*.

Örneğin, bir eylem yöntemi bir blog gönderisini düzenlemek için kullanılan varsayın:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Aşağıdaki örnek, bir blog gönderisini görünüm modeli gösterir.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Bir kullanıcı Description özelliği için bazı biçimlendirme gönderdiğinde, model bağlama nedeniyle istek doğrulama başarısız olur. İstek doğrulama blog yayını açıklaması için model bağlama sırasında devre dışı bırakmak için uygulama *SkipRequpestValidationAttribute* Bu örnekte gösterildiği gibi özelliğine:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Modelin her özelliği için istek doğrulamayı devre dışı bırakmak için alternatif olarak, uygulama *ValidateInputAttribute* değerini *false* eylem yöntemi için:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Yeni Değişiklikler

- Özel durum filtreleri için yürütme sırasını aynı olan özel durum filtreleri değişti *sipariş* değeri. ASP.NET MVC 2 ve daha önceki sürümlerde, aynı olan denetleyicisine özel durum filtreleri *sipariş* gibi bir eylem yönteminin üzerindekiler eylem yöntemi özel durum filtreleri önce yürütüldü. Özel durum filtreleri uygulandığında bu durum genellikle olacaktır belirtilen olmadan *sipariş* değeri. Böylece en belirli özel durum işleyici ilk yürütür ASP.NET MVC 3'te bu sırasını tersine çevrildi. Önceki sürümlerinde olduğu gibi *sipariş* özelliği açıkça belirtilen, filtreler belirtilen sırada çalıştırılır.
- Adlı yeni bir özellik eklenen *FileExtensions* için *VirtualPathProviderViewEngine* temel sınıfı. Bir görünüm içinde yer alan bir dosya uzantısı yalnızca bir görünümlerle yol (ve ada göre değil), bakarken bu yeni özelliği tarafından belirtilen liste olarak kabul edilir. Kullanıcılar web form görünümleri için bir özel dosya uzantısını etkinleştirmek için özel yapı sağlayıcıyı kaydettirin ve bir adı yerine bir tam yol kullanarak bu görünümler başvuran için önemli bir değişiklik budur. Değerini değiştirmek için geçici bir çözüm değildir *FileExtensions* özel dosya uzantısını eklemeyi özelliği.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- Yükleyici, Visual Studio 2010 'un bileşenleri güncelleştirdiğinden tamamlamak için ASP.NET MVC önceki sürümlerinden daha uzun sürebilir.
- Görünüm Ekle iskele astrongly seçerken görünüm iskelesini kurar salt yazılır özellikler belirtilmiş. Bunlar her zaman yapı iskelesi tarafından sayılır. Görünüm Ekle iletişim kutusu ayrıca salt okunur özellikler "Düzenle" veya "Oluştur" görünümü oluştururken iskelesini kurar. Salt okunur özellikler yalnızca görüntüleme ve liste görünümleri için iskele kurulmuş.
- ASP.NET MVC 3 Async CTP'nin yüklendiğinde, hata ayıklama işe yaramaz. ASP.NET MVC 3 yüklü yan yana Async CTP'nin sahip olamaz. Hata ayıklama onarmak için zaman uyumsuz CTP kaldırın. Daha fazla ayrıntı için okuma [bu blog gönderisine](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) ASP.NET MVC 3 RC tüm parçalarını kaldırma hakkında.
- Resharper yüklendiğinde razor IntelliSense çalışmaz. Yüklü ReSharper varsa ve ASP.NET MVC 3 okuyun RC, Razor IntelliSense desteği yararlanmak isterseniz [bu blog gönderisine](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) bunları birlikte bugün kullanmanın yollarını tartışmaktadır JetBrains gelen.
- Beta, ASP.NET MVC 3 ile oluşturulan CSHTML ve VBHTML görünümleri kendi yapı eylemi doğru hangi yayımlama atlar gerekmez. *Yapı eylemi* için bu dosyalar "İçerik" olarak ayarlanmalıdır. ASP.NET MVC 3 RC, yeni dosyalar için bu sorunu giderir ancak Beta ile oluşturulan bir proje için var olan dosyaların ayarını düzeltmesini değil.
- Yükleyici, Visual Studio 2010 'un bileşenleri güncelleştirdiğinden tamamlamak için ASP.NET MVC önceki sürümlerinden daha uzun sürebilir.
- Bir "Düzenle" kesinlikle seçerek görünüm iskelesini kurar yazıldığında Görünüm Ekle yapı iskelesi yalnızca özellikleri okuyun. Benzer şekilde, salt yazılır Özellikler "Görünen" görünümleri için iskele kurulmuş.
- Yükleme sırasında EULA kabul iletişim kutusunda Lisans Koşulları'nı istenenden daha küçük bir pencerede görüntüler.
- Visual Studio Async CTP'nin yükleme ASP.NET MVC yükleme tooling 3'ün bir parçası olarak dahil edilir Razor sürümüyle bir çakışmasına neden oluyor. Visual Studio Async CTP'nin ve Razor yayın aynı makinede yüklemeye çalışmayın emin olun.
- Razor Görünüm (.cshtml dosyası) düzenlerken, Visual Studio denetleyicisi Git menü öğesi kullanılamaz ve hiçbir kod parçacıkları vardır.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta 6 Ekim 2010'da yayımlanmıştır. Aşağıdaki Notlar Beta sürümüne özeldir ve tüm güncelleştirmeleri veya ASP.NET MVC 3 Sürüm Adayı'bölümünde yukarıdaki değişiklikleri tabidir.

## <a id="0.1__Toc274034215"></a>  Yeni Featuresin ASP.NET MVC 3 Beta

<a id="0.1__Default_validation_system"></a>Bu bölümde tanıtılmıştır özellikleri açıklanmıştır ASP.NET MVC 3 Beta sürümünde.

### <a id="0.1__Toc274034216"></a>  NuGet Paket Yöneticisi

ASP.NET MVC 3 ekleme kitaplıklar için bir tümleşik paket yönetim aracı ve araçları Visual Studio projeleri için NuGet Paket Yöneticisi içerir. Çoğunlukla, geliştiriciler bugün kendi kaynak ağacına bir kitaplık almak için attığınız adımlar otomatikleştirir.

Bir komut satırı aracı olarak, Visual Studio 2010 içinde tümleşik konsol penceresi Visual Studio bağlam menüsünden ve PowerShell cmdlet'leri kümesi olarak NuGet ile çalışabilirsiniz.

NuGet hakkında daha fazla bilgi için okuma [NuGet belgelerine](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Geliştirilmiş yeni proje iletişim kutusu

Yeni bir proje oluşturduğunuzda, yeni proje iletişim kutusu artık görünüm altyapısının yanı sıra bir ASP.NET MVC proje türü belirtmenize olanak sağlar.

![](mvc3-release-notes/_static/image6.png)

Bu sürümde şablonları ve görünüm altyapıları iletişim kutusunda listelenen listesini değiştirmek için destek dahil edilmez.

Varsayılan Şablonlar aşağıda verilmiştir:

Boş. Varsayılan dizin yapısı ASP.NET MVC projeleri, ASP.NET MVC stilleri varsayılan ve varsayılan JavaScript dosyaları içeren bir komut dizini içeren küçük bir Site.css dosyası dahil olmak üzere bir ASP.NET MVC proje dosyalarını en az bir kümesini içerir.

Internet uygulaması. ASP.NET MVC içindeki üyelik sağlayıcısının nasıl kullanılacağını gösteren örnek işlevselliği içerir.

### <a id="0.1__Toc274034218"></a>  Kesinlikle belirtmek için basitleştirilmiş yol modelleri Razor görünümleri belirtilmiş.

Kesin türü belirtilmiş Razor görünümleri için model türü belirtmek için yol yeni kullanarak basitleştirilmiştir @model CSHTML görünümler için yönerge ve @ModelType VBHTML görünümler için yönerge. ASP.NET MVC önceki sürümlerde Razor için kesin türü belirtilmiş bir modelin bu şekilde görünümleri belirtmeniz gerekir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

Bu sürümde, aşağıdaki söz dizimini kullanabilirsiniz:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Yeni ASP.NET Web sayfaları için yardımcı yöntemler desteği

Yeni ASP.NET Web sayfaları teknolojinin yaygın olarak kullanılan işlevler görünümleri ve denetleyicilerini ekleme için yararlı yardımcı yöntemler kümesi içerir. ASP.NET MVC 3 denetleyicileri ve görünümler içinde bu yardımcı yöntemler (uygun olduğunda) kullanılmasını destekler. Bu yöntemler System.Web.Helpers bütünleştirilmiş kodunda yer alır. Aşağıdaki tabloda, ASP.NET Web sayfaları yardımcı yöntemler bazılarını listeler.

| **Yardımcısı** | **Açıklama** |
| --- | --- |
| Grafik | Bir grafik görünümü içinde işler. Chart.ToWebImage, Chart.Save ve Chart.Write gibi yöntemler içerir. |
| Crypto | Güvenlik ve parolaların karma algoritmaları düzgün bir şekilde oluşturmak için karma kullanır. |
| WebGrid | Nesneler (genellikle, bir veritabanından veri) koleksiyonunu bir kılavuz işler. Disk belleği ve sıralama destekler. |
| WebImage | Bir görüntü oluşturur. |
| WebMail | Bir e-posta iletisi gönderir. |

Yardımcıları ve temel sözdizimi listeleyen bir hızlı başvuru konu kullanılabilir şu URL'de ASP.NET Razor sözdizimini belgelerinin bir parçası olarak:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Ek bağımlılık ekleme desteği

ASP.NET MVC 3 Preview 1 sürüm oluşturma, şu anki sürümde iki yeni ve dört mevcut Hizmetleri için destek eklendi ve bağımlılık çözümlemesi ve genel hizmet bulucu için geliştirilmiş destek içerir.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Hassas denetleyicisi örneklemesi için yeni IControllerActivator arabirimi

Yeni IControllerActivator arabirim denetleyicilerini bağımlılık ekleme nasıl oluşturulur üzerinden daha ayrıntılı denetim sağlar. Aşağıdaki örnek, arabirimi gösterir:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Denetleyici üreteci rolüne karşılaştırın. Bir denetleyici üreteci, denetleyici türü bulma hem Bu denetleyici türünün bir örneği başlatmasını sorumludur IControllerFactory arabiriminin bir uygulamasıdır.

Denetleyici etkinleştiricileri yalnızca denetleyici türünün bir örneği oluşturmak için sorumludur. Denetleyici türü araması gerçekleştirmek değil. Uygun denetleyici türü bulunduktan sonra denetleyici üreteçlerini IControllerActivator örneğine gerçek denetleyici örneği oluşturulmasını işlemek için temsilci olarak atanmalıdır.

DefaultControllerFactory sınıfı IControllerFactory örneğini kabul eden yeni bir oluşturucusu vardır. Bu, bağımlılık ekleme varsayılan denetleyici türü arama davranışını geçersiz kılmak zorunda kalmadan denetleyicisi oluşturmanın bu yönden yönetmek için uygulamanıza olanak sağlar.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Iservicelocator arabirimi Idependencyresolver ile değiştirilir

Topluluk geri bildirimi doğrultusunda, ASP.NET MVC 3 Beta sürümü, ASP.NET MVC ihtiyaçlarını belirli bir slimmed aşağı Idependencyresolver arabirim Iservicelocator arabirimi kullanımını değiştirilmiştir. Aşağıdaki örnek yeni bir arabirimi gösterir:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Bu değişikliğin bir parçası olarak ServiceLocator sınıfı ayrıca DependencyResolver sınıfı ile değiştirilmiştir. Bağımlılık çözümleyici kaydı ASP.NET MVC eski sürümleri için benzer:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Bu arabirim uygulamaları yalnızca istenen tür için kayıtlı hizmet sağlamak üzere temel alınan bağımlılık ekleme kapsayıcısına temsilci.

İstenen türün kayıtlı hizmeti olmadığında, ASP.NET MVC GetService null döndürmek için ve GetServices boş koleksiyon döndürmek için bu arabirimin uygulamaları; bekliyor.

Yeni DependencyResolver sınıfı yeni Idependencyresolver arabirimi ya da (Iservicelocator) genel hizmet bulucu arabirimini uygulayan sınıflar kaydetmek olanak sağlar. Genel hizmet bulucu hakkında daha fazla bilgi için bkz: [github'da CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Ayrıntılı Görünüm sayfası oluşturmada için yeni IViewActivator arabirimi

Yeni IViewPageActivator arabirimi görünüm sayfalarının bağımlılık ekleme nasıl oluşturulur üzerinde daha ayrıntılı denetim sağlar. Bu WebFormView örnekleri ve RazorView örnekleri için geçerlidir. Aşağıdaki örnek yeni bir arabirimi gösterir:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Bu sınıfların şimdi sağlayan nasıl ViewPage, ViewUserControl ve WebViewPage türleri örneği denetlemek için bağımlılık ekleme kullanın IViewPageActivator oluşturucu bağımsız değişken kabul edin.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Var olan hizmetleri için yeni bağımlılık çözümleyici desteği

Yeni sürüm bağımlılık çözünürlük desteği aşağıdaki hizmetleri içerir:

- Model doğrulama sağlayıcılarının. Bağımlılık çözümleyiciyi ModelValidatorProvider uygulayan sınıflar kaydedilebilir ve sistem bunları istemci ve sunucu tarafı doğrulamayı desteklemek için kullanır.
- Model meta veri sağlayıcısı. Bağımlılık çözümleyiciyi uygulayan ModelMetadataProvider tek bir sınıf kaydedilebilir ve sistem için şablon oluşturma ve doğrulama sistemleri meta verilerini sağlamak için kullanır.
- Değer sağlayıcıları. Bağımlılık çözümleyiciyi ValueProviderFactory uygulayan sınıflar kaydedilebilir ve sistem bunları denetleyicisi tarafından ve model bağlama sırasında tüketilen değer sağlayıcıları oluşturmak için kullanır.
- Model bağlayıcıları. Bağımlılık çözümleyiciyi IModelBinderProvider uygulayan sınıflar kaydedilebilir ve sistem bunları model bağlama sistem tarafından tüketilen model bağlayıcıları oluşturmak için kullanır.

### <a id="0.1__Toc274034221"></a>  Örtük jQuery tabanlı Ajax yeni desteği

ASP.NET MVC Ajax yardımcı yöntemler aşağıdaki gibi içerir:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Bu yöntemler, tam geri gönderimin kullanmak yerine sunucu üzerinde bir eylem yöntemini çağırmak için JavaScript kullanır. Bu işlevsellik, jQuery örtük bir şekilde yararlanmak için güncelleştirilmiştir. Satır içi istemci komut dosyalarını intrusively yayma yerine bu yardımcı yöntemler davranışı biçimlendirmeden kullanarak HTML5 özniteliklerini yayma tarafından ayrı *veri ajax* öneki. Davranış, uygun JavaScript dosyaları başvurarak biçimlendirme sonra uygulanır. Şu JavaScript dosyaları başvurulduğundan emin olun:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Bu özellik, ASP.NET MVC 3 yeni proje şablonları Web.config dosyasında varsayılan olarak etkindir, ancak mevcut projeleri için varsayılan olarak devre dışıdır. Daha fazla bilgi için bkz: [eklenen istemci doğrulama ve örtük JavaScript uygulama çapında bayrakları](#0.1_AddedApplicationWideFlagsForClientValida) belgesinde.

### <a id="0.1__Toc274034222"></a>  Örtük jQuery doğrulama için yeni destek

Varsayılan olarak, ASP.NET MVC 3 Beta istemci tarafı doğrulama gerçekleştirmek için örtük bir şekilde jQuery doğrulama kullanır. Örtük istemci doğrulamasını etkinleştirmek için bir görünüm içinde aşağıdaki gibi bir çağrı yapın:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Bu, ViewContext.UnobtrusiveJavaScriptEnabled özelliği aşağıdaki çağrıyı yaparak yapmak için true olarak ayarlandığını gerektirir:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Ayrıca aşağıdaki JavaScript dosyaları başvurulduğundan emin olun.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Bu özellik, ASP.NET MVC 3 yeni proje şablonları Web.config dosyasında varsayılan olarak etkinleştirilir, ancak mevcut projeleri için varsayılan olarak devre dışıdır. Daha fazla bilgi için bkz: [istemci doğrulama ve örtük JavaScript için yeni uygulama çapında bayrakları](#0.1_AddedApplicationWideFlagsForClientValida) belgesinde.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  İstemci doğrulama ve örtük JavaScript için yeni uygulama çapında bayrakları

Etkinleştirmek veya istemci doğrulama ve genel olarak aşağıdaki örnekteki gibi HtmlHelper sınıfının statik üyeleri kullanarak örtük JavaScript devre dışı bırakın:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Varsayılan proje şablonları varsayılan olarak örtük JavaScript'i etkinleştirir. Etkinleştirmek veya aşağıdaki ayarları kullanarak, uygulamanızın kök Web.config dosyasında bu özellikleri devre dışı bırak:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Varsayılan olarak bu özellikleri etkinleştirmek için yeni aşırı varsayılan ayarları geçersiz kılar aşağıdaki örneklerde gösterildiği gibi izin HtmlHelper sınıfı için yapılmıştır:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Geriye dönük uyumluluk için bu özelliklerin her ikisi de varsayılan olarak devre dışıdır.

### <a id="0.1__Toc274034224"></a>  Yeni görünümler çalıştırmadan önce çalışan bir kod desteği

Adlı bir dosya artık koyabilirsiniz \_viewstart.cshtml (veya \_viewstart.vbhtml) görünümleri dizininde ve o dizin ve alt dizinlerinde birden çok görünüm arasında paylaşılacak bu kodu ekleyin. Örneğin, aşağıdaki kodu bırakabilecek \_~/Views klasörde viewstart.cshtml sayfa:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Her görünüm görünümleri klasöründeki ve onun tüm alt klasörleri yinelemeli Düzen sayfasını ayarlar. Ne zaman bir görünüm işlenirken, kodda \_viewstart.cshtml dosya görünüm kodu çalıştırmadan önce çalışır. \_Viewstart.cshtml kod o klasördeki her görünüm için geçerlidir.

Varsayılan olarak, kodda \_viewstart.cshtml dosya da uygulandığı herhangi bir alt görünümlerde. Ancak, ayrı ayrı alt klasörler kendi sürümüne sahip olabilir \_viewstart.cshtml dosya; içeren durumda, yerel sürümü önceliklidir. Örneğin, tüm görünümlere HomeController için ortak olan kodu çalıştırmak için put bir \_~/Views/Home klasöründe viewstart.cshtml dosyası.

### <a id="0.1__Toc274034225"></a>  Yeni destek VBHTML Razor sözdizimi için

Önceki ASP.NET MVC Önizleme C# üzerinde alan Razor sözdizimini kullanarak görünümleri için destek dahil. Bu görünümler .cshtml dosya uzantısını kullanabilirsiniz. Razor desteklemeye devam eden iş parçası olarak, ASP.NET MVC 3 Beta .vbhtml dosya uzantısını kullanır Visual Basic'te Razor sözdizimi için destek sunar.

Visual Basic söz dizimine VBHTML sayfalarında kullanmaya giriş bilgileri için aşağıdaki URL'de öğretici bakın:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  ValidateInputAttribute üzerinde daha ayrıntılı denetim

ASP.NET MVC her zaman gelen istek zararlı Giriş içermediğinden emin olmak için çekirdek ASP.NET istek doğrulama altyapı çağırır ValidateInputAttribute sınıfı eklemiştir. Varsayılan olarak, giriş doğrulaması etkindir. Aşağıdaki örnekte olduğu gibi ValidateInputAttribute özniteliğini kullanarak istek doğrulamayı devre dışı bırakmak mümkündür:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Ancak, birçok web uygulamaları kalan alanları vermemelisiniz sırada HTML, izin vermek için tek tek form alanları vardır. ValidateInputAttribute sınıfı, şimdi istek doğrulama eklenmemelidir alanların listesini belirtmenize olanak sağlar.

Örneğin, bir blog altyapısı geliştiriyorsanız, gövde ve Özet alanları biçimlendirmede izin vermek isteyebilirsiniz. Bu alanların her özellik adı ("Body" ve "Özet") karşılık gelen bir ad özniteliği olan iki giriş öğe tarafından temsil edilebilir. İstek devre dışı bırakmak için bu alanlar için doğrulama yalnızca belirtin (aşağıdaki örnekte olduğu gibi ValidateInput sınıfının dışlama özelliğinde virgülle) adları:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Yardımcıları alt çizgi için kısa çizgi anonim nesneleri kullanarak belirtilen HTML öznitelik adları için Dönüştür

Yardımcı yöntemler aşağıdaki örnekteki gibi bir anonim nesnenin kullanarak öznitelik ad/değer çiftleri belirtmenizi sağlar:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

ASP.NET özelliği adı bir tire kullanılamadığı için bu yaklaşım, öznitelik adı kısa çizgi kullanmanıza izin vermez. Ancak, kısa çizgi özel HTML5 öznitelikler için önemli olan; Örneğin, HTML5 "data-" önekini kullanır.

Aynı zamanda, alt çizgi HTML öznitelik adları için kullanılamaz, ancak özellik adları içinde geçerlidir. Bu nedenle, bir anonim nesnenin kullanarak özniteliklerini belirtirseniz ve öznitelik adları alt çizgi dahil ederseniz,, yardımcı yöntemler alt çizgi için kısa çizgi dönüştürür. Örneğin, alt çizgi aşağıdaki yardımcı sözdizimini kullanır:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Yardımcı çalıştığında, önceki örnekte aşağıdaki biçimlendirme oluşturur:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Hata düzeltmeleri

Varsayılan nesne şablonu EditorFor ve DisplayFor şablonu Yardımcıları için artık DisplayAttribute.Order özelliğinde belirtilen sıralama destekler. (Önceki sürümlerde, sipariş ayarı kullanılmadı.)

İstemci doğrulama şimdi doğrulama doğrulama öznitelikleri uygulanan geçersiz kılınan özellikleri destekler.

JsonValueProviderFactory artık varsayılan olarak kaydedilir.

## <a id="0.1__Toc274034229"></a>  Yeni değişiklikler

Özel durum filtreleri için yürütme sırasını aynı sıra değeri olması için özel durum filtreleri değişti. Bir eylem yönteminin üzerindekiler eylem yöntemi özel durum filtreleri önce yürütüldü gibi aynı sıraya sahip denetleyicisinde özel durum ASP.NET MVC 2 ve daha önceki sürümlerde, filtreler. Özel durum filtreleri belirtilen bir sıra değeri uygulanan olduğunda bu durum genellikle olacaktır. Böylece en belirli özel durum işleyici ilk yürütür ASP.NET MVC 3'te bu sırasını tersine çevrildi. Order özelliğini açıkça belirtilmediği takdirde, önceki sürümlerinde olduğu gibi belirtilen sırada filtreleri çalıştırılır.

## <a id="0.1__Toc274034230"></a>  Bilinen sorunlar

Yükleme sırasında EULA kabul iletişim kutusunda Lisans Koşulları'nı istenenden daha küçük bir pencerede görüntüler.

Razor görünümleri ya da söz dizimi vurgulamayı IntelliSense desteği yoktur. Visual Studio'da Razor sözdizimi için destek daha sonraki bir sürümüne bir parçası olarak dahil olacağını tahmin edilmektedir.

Razor Görünüm (CSHTML dosyası) düzenlerken <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> denetleyicisi Git menü öğesi Visual Studio'da kullanılabilir olmaz ve hiçbir kod parçacıkları vardır.

Kullanırken @model kesin türü belirtilmiş bir CSHTML belirtmek için sözdizimini görüntülemek, türleri için dile özgü kısayolları tanınmıyor. Örneğin, @model int çalışmaz, ancak @model Int32 çalışır. Geçici çözüm bu hata için model türü belirttiğinizde, gerçek tür adı kullanmaktır.

Kullanırken @model kesin türü belirtilmiş bir CSHTML görünümü belirtmek için sözdizimi (veya @ModelType kesin türü belirtilmiş bir VBHTML görünümü belirtmek için), boş değer atanabilir türler ve dizi bildirimleri desteklenmez. Örneğin, @model int? desteklenmiyor. Bunun yerine, kullanın `@model Nullable<Int32>`. Sözdizimi @model string [] ayrıca desteklenmez; bunun yerine, kullanın `@model IList<string>`.

Bir ASP.NET MVC 2 projesini ASP.NET MVC 3'e yükselttiğinizde, Web.config dosyasının appSettings bölümünde aşağıdaki eklediğinizden emin olun:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Kimliği doğrulanmamış kullanıcıların ~/Account/Web.config dosyasında kullanılan forms kimlik doğrulaması ayarı yoksayılıyor oturum açmak için her zaman yeniden yönlendirmek form kimlik doğrulaması neden bilinen bir sorun yoktur. Aşağıdaki uygulama ayarı eklemek için geçici bir çözüm değildir.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Sorumluluk reddi

© 2011 Microsoft Corporation. Tüm hakları saklıdır. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Kullanım riski size aittir.

Bu belge, herhangi bir Microsoft ürünü üzerinde hiçbir fikri mülkiyet hakkı sağlamaz. Kopyalayabilir ve bu belgeyi şirket içinde kullanmak başvuru amaçlıdır.
