---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 7342b5f4a7e2327f3f3850941510a6e46ec30842
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753811"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Genel bakış](#overview)
- [Yükleme notları](#installation-notes)
- [Yazılım gereksinimleri](#software-requirements)
- [Belgeler](#documentation)
- [Destek](#support)
- [Bir ASP.NET MVC 2 projesini ASP.NET MVC için yükseltme 3 araçları güncelleştirme](#upgrading)
- [ASP.NET MVC 3 araçları güncelleştirme (12 Nisan 2011)](#tu-changes)

    - ["Denetleyici Ekle" iletişim kutusu artık denetleyicileriyle görünümleri ve veri erişim kodu iskelesini oluşturabilirsiniz](#tu-AddControllerDialog)
    - [Yapılan geliştirmeler "ASP.NET MVC 3 yeni proje" iletişim kutusu](#tu-ImprovementsNewDialogBox)
    - [Proje şablonları artık Modernizr 1.7 içerir](#tu-Modernizr)
    - [Proje şablonları içerir, jQuery, jQuery kullanıcı Arabirimi ve jQuery güncelleştirilmiş sürümlerini doğrulama](#tu-UpdatedJQuery)
    - [Proje şablonları, ADO.NET varlık çerçevesi 4.1 artık önceden yüklenmiş bir NuGet paketi olarak içerir.](#tu-EF)
    - [Proje şablonları JavaScript kitaplıklarını önceden yüklenmiş NuGet paketleri olarak içerir.](#tu-JavaScriptLibsNuget)
    - [Bilinen Sorunlar](#tu-KI)
- [ASP.NET MVC 3 RTM (13 Ocak 2011)](#MVC3RTM)

    - [Değişiklik: sürümü, jQuery kullanıcı Arabirimi 1.8.7 için güncelleştirildi.](#RTM-1)
    - [Değişiklik: ModelMetadataProvider geri DataAnnotationsModelMetadataProvider için varsayılan değiştirildi](#RTM-2)
    - [Düzeltildi: ters içindeki boşluk sonuçlarını içeren bir Razor ifadesinin parçası yapıştırma](#RTM-3)
    - [Düzeltildi: düzenleyicide açılan bir Razor dosyayı yeniden adlandırma söz dizimi renklendirme ve IntelliSense devre dışı bırakır](#RTM-4)
    - [Bilinen Sorunlar](#RTM-KI)
    - [Bozucu değişiklikler](#RTM-BC)
- [ASP.NET MVC 3 Sürüm Adayı 2 (10 Aralık 2010)](#_Toc2)

    - [Proje şablonları jQuery 1.4.4, jQuery doğrulama 1.7 ve jQuery kullanıcı Arabirimi 1.8.6y kullanıcı Arabirimi 1.8.6 içerecek şekilde değiştirildi.](#_Toc2_1)
    - [Eklenen "AdditionalMetadataAttribute" sınıfı](#_Toc2_2)
    - [Geliştirilmiş görünüm iskele kurma](#_Toc2_3)
    - [Eklenen Html.Raw yöntemi](#_Toc2_3)
    - [Özellik adı "Controller.ViewModel" ve "ViewBag" "Görünüm" özelliğine](#_Toc2_4)
    - ["SessionStateAttribute" olarak yeniden adlandırıldı "ControllerSessionStateAttribute" sınıfı](#_Toc2_5)
    - [Yeniden adlandırılan RemoteAttribute "Alanlar" özelliğini "AdditionalFields"](#_Toc2_6)
    - ["AllowHtmlAttribute" için "SkipRequestValidationAttribute" olarak yeniden adlandırıldı](#_Toc2_7)
    - [Değiştirilen "Html.ValidationMessage" yöntemi ilk kullanışlı hata iletisini görüntüler](#_Toc2_8)
    - [Sabit @model boşluk belgeye ekleme bildirimi](#_Toc2_9)
    - [Görünüm altyapıları motoru özel dosya adları desteklemek için eklenen "FileExtensions" özelliğini](#_Toc2_10)
    - [Sabit "LabelFor" Yardımcısı "İçin" özniteliği için doğru değeri yayma](#_Toc2_11)
    - [Model bağlama sırasında açık değerler öncelik vermek için sabit "RenderAction" yöntemi](#_Toc2_12)
    - [Bozucu değişiklikler](#_Toc2_BC)
    - [Bilinen Sorunlar](#_Toc2_KI)
- [ASP.NET MVC 3 Sürüm Adayı (9 Kasım 2010)](#TOC_ASP_NET_3_RC)

    - [ASP.NET MVC 3 rc'deki yenilikleri](#_Toc276711785)
    - [NuGet Paket Yöneticisi](#_Toc276711786)
    - ["Yeni Proje" Gelişmiş iletişim kutusu](#_Toc276711787)
    - [Oturumsuz denetleyicileri](#_Toc276711788)
    - [Yeni doğrulama öznitelikleri](#_Toc276711789)
    - ["LabelFor" ve "LabelForModel" yöntemleri için yeni aşırı yüklemeler](#_Toc276711790)
    - [Alt eylem çıktı önbelleği](#_Toc276711791)
    - ["Görünümü Ekle" iletişim kutusu iyileştirmeleri](#_Toc276711792)
    - [Ayrıntılı isteği doğrulama](#_Toc276711793)
    - [Bozucu değişiklikler](#_Toc276711794)
    - [Bilinen Sorunlar](#_Toc276711795)
- [ASP. MVC 3 (6 Ekim 2010) Beta notları](#TOC_ASP_NET_3_Beta)

    - [ASP.NET MVC 3 Beta sürümündeki yeni özellikler](#0.1__Toc274034215)
    - [NuPack Paket Yöneticisi](#0.1__Toc274034216)
    - [Yeni Proje iletişim kutusu İyileştirildi](#0.1__Toc274034217)
    - [Kesin belirtmek için basitleştirilmiş bir yolu modelleri Razor görünümleri yazdınız.](#0.1__Toc274034218)
    - [Yeni ASP.NET Web sayfaları için yardımcı yöntemler desteği](#0.1__Toc274034219)
    - [Ek bağımlılık ekleme desteği](#0.1__Toc274034220)
    - [Yeni bir örtük jQuery tabanlı Ajax desteği](#0.1__Toc274034221)
    - [Örtük jQuery doğrulaması için yeni destek](#0.1__Toc274034222)
    - [İstemci doğrulama ve Unobtrusive JavaScript için yeni uygulama çapında bayrakları](#0.1__Toc274034223)
    - [Görünümleri çalıştırmadan önce çalışan kod için yeni destek](#0.1__Toc274034224)
    - [VBHTML Razor sözdizimi için yeni destek](#0.1__Toc274034225)
    - [ValidateInputAttribute üzerinde daha ayrıntılı denetim](#0.1__Toc274034226)
    - [Yardımcıları kısa çizgiler için alt çizgi, anonim nesneleri kullanarak belirtilen HTML öznitelik adları için Dönüştür.](#0.1__Toc274034227)
    - [Hata düzeltmeleri](#0.1__Toc274034228)
    - [Bozucu değişiklikler](#0.1__Toc274034229)
    - [Bilinen Sorunlar](#0.1__Toc274034230)
- [Sorumluluk reddi](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Genel Bakış

Bu belgede, ASP.NET MVC 3 RTM sürümü Visual Studio 2010 için açıklanmaktadır. ASP.NET MVC, Model-View-Controller (MVC) deseni kullanan Web uygulamaları geliştirmeye yönelik bir çerçevedir. ASP.NET MVC 3 Yükleyici aşağıdaki bileşenleri içerir:

- ASP.NET MVC 3 çalışma zamanı bileşenleri
- ASP.NET MVC 3 Visual Studio 2010 Araçları
- ASP.NET Web sayfaları çalışma zamanı bileşenleri
- ASP.NET Web sayfaları Visual Studio 2010 Araçları
- Microsoft .NET (NuGet) için Paket Yöneticisi
- Razor sözdizimi için destek sağlayan Visual Studio 2010 için bir güncelleştirme. (Ayrıntılar için bkz. Bilgi Bankası makalesi 2483190)

Eksiksiz bir ASP.NET MVC 3 her yayın öncesi sürümü için sürüm notları, ASP.NET Web sitesinde şu URL'de bulunabilir:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Yükleme notları

Web Platformu Yükleyicisi (Web PI) kullanarak ASP.NET MVC 3 RTM yüklemek için şu sayfayı ziyaret edin:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternatif olarak, ASP.NET MVC 3 RTM için yükleyici Visual Studio 2010 için şu sayfadan indirebilirsiniz:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 yüklenebilir ve yan yana çalıştırabilirsiniz ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET MVC 3 çalışma zamanı bileşenlerini aşağıdaki yazılımı gerektirir:

- .NET framework sürüm 4. 

    ASP.NET MVC 3 Visual Studio 2010 Araçları aşağıdaki yazılımı gerektirir:
- Visual Studio 2010 veya Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

ASP.NET MVC için belgeleri, MSDN Web sitesinde şu URL'de kullanılabilir:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Öğreticiler ve diğer bilgileri ASP.NET MVC ASP.NET Web sitesinin şu URL'de MVC sayfası bulunur:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Destek

Bu tam olarak desteklenen bir sürümdür. Teknik destek alma hakkında bilgi bulunabilir [Microsoft Support Web sitesi](https://support.microsoft.com/).

Ayrıca bu sürüm hakkında sorular ASP.NET topluluk üyelerinin sık resmi olmayan desteği mümkün olduğu ASP.NET MVC, foruma çekinmeyin:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Bir ASP.NET MVC 2 projesini ASP.NET MVC için yükseltme 3 araçları güncelleştirme

ASP.NET MVC 3, aynı bilgisayarda bir ASP.NET MVC 2'uygulaması için ASP.NET MVC 3 yükseltme zamanı seçme esnekliği sağlayan ASP.NET MVC 2'ile yan yana yüklenebilir.

Mevcut bir ASP.NET MVC 2 uygulama sürüm 3 için el ile yükseltmek için aşağıdakileri yapın:

1. Yeni ve boş bir ASP.NET MVC 3 projelerini oluşturur. Bu proje yükseltme için gerekli olan bazı dosyaları içerir.
2. Aşağıdaki dosyalar, ASP.NET MVC 3 projeden ASP.NET MVC 2 projenizin karşılık gelen konumuna kopyalayın. JQuery (1.5.1.js jQuery) yeni dosya için hesap kitaplığa yönelik tüm başvuruları güncelleştirmeniz gerekir: 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /İçerik/temaları/\*.\*
3. Kopyalama *paketleri* çözümün .sln dosyasının bulunduğu dizindir çözümünüzün kök boş ASP.NET MVC 3 proje çözümde kökünde klasör.
4. ASP.NET MVC 2 projenizi alanları içeriyorsa, /Views/Web.config dosyaya kopyalayın *görünümleri* her alanı klasörü.
5. Hem ASP.NET MVC 2 projesini Web.config dosyalarında, küresel olarak aramak ve ASP.NET MVC sürüm değiştirin. Aşağıda bulabilirsiniz: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Aşağıdakiyle değiştirin:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Solution Explorer'da, başvurusunu silmek *System.Web.Mvc* (hangi noktalarından dll sürüm 2), ardından bir başvuru ekleyin *System.Web.Mvc* (v3.0.0.0).
7. System.Web.WebPages.dll ve System.Web.Helpers.dll bir başvuru ekleyin. Bu derlemeler bulunan aşağıdaki klasörler bulunur: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. Çözüm Gezgini'nde proje adına sağ tıklayın ve projeyi seçin. Ardından Proje adı tekrar sağ tıklayıp Düzen *ProjectName*.csproj.
9. Bulun *ProjectTypeGuids* öğesi ve {E53F8FEA-EAE0-44A6-8774-FFD645390401} {F85E285D-A4E0-4152-9332-AB1D724D3325} değiştirin.
10. Değişiklikleri kaydedin, projeye sağ tıklayın ve ardından projeyi seçin.
11. Uygulamanın kök Web.config dosyasında aşağıdaki ayarlara ekleme *derlemeleri* bölümü. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. ASP.NET MVC 2 kullanılarak derlenen herhangi bir üçüncü taraf kitaplıklar proje başvuru yapıyorsa, vurgulanan aşağıdakileri ekleyin *bindingRedirect* uygulama kökü Web.config dosyasında öğesine  *Yapılandırma* bölümü: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Değişiklikleri ASP.NET MVC 3 araçları güncelleştirme

Bu bölümde ASP.NET MVC 3 araçları güncelleştirme sürümde ASP.NET MVC 3 RTM sürümünden sonra yapılan değişiklikler açıklanmaktadır.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>"Denetleyici Ekle" iletişim kutusu artık denetleyicileriyle görünümleri ve veri erişim kodu iskelesini oluşturabilirsiniz

Yapı iskelesi bir denetleyici ve görünüm uygulamanız için hızlıca oluşturmanın bir yoludur. Kod oluşturulduktan sonra projenizin gereksinimlerini karşılayacak şekilde düzenleyebilirsiniz.

Başlatmak için *denetleyici Ekle* ASP.NET MVC 3, iletişim kutusunda sağ *denetleyicileri* klasöründe *Çözüm Gezgini*, tıklayın *Ekle*ve ardından *denetleyicisi*. İletişim kutusu, ek yapı iskelesi seçenekleri sunmak için geliştirilmiştir.

![](mvc3-release-notes/_static/image1.png)

Varsayılan olarak üç yapı iskelesi şablonlar vardır.

#### <a name="empty-controller"></a>Boş denetleyici

Bu şablon, bir boş denetleyici dosyası oluşturur. Bu şablon olmayan denetimini eşdeğerdir *oluştur, Düzenle, Ayrıntılar için Eylemler ekleyin, senaryoları silme* ASP.NET MVC önceki sürümlerinde. Bu tercih ederseniz başka hiçbir seçenek kullanılabilir.

#### <a name="controller-with-empty-readwrite-actions"></a>Boş okuma/yazma eylemleri ile denetleyicisi

Bu şablon, tüm gerekli eylem yöntemlerini ancak hiçbir uygulama kodu yöntemlere olan bir denetleyici dosyası oluşturur. Bu şablon denetimini eşdeğerdir *oluştur, Düzenle, Ayrıntılar için Eylemler ekleyin, senaryoları silme* ASP.NET MVC önceki sürümlerinde. Bu tercih ederseniz başka hiçbir seçenek kullanılabilir.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Okuma/yazma eylemleri ve Entity Framework kullanarak görünümler ile denetleyicisi

Bu şablon, çalışma veri girişi kullanıcı arabirimi hızlı bir şekilde oluşturmanızı sağlar. Bu, bir dizi ortak gereksinimleri ve aşağıdaki gibi senaryoları işleyen kodu oluşturur:

- *Veri erişimi*. Oluşturulan kodun okur ve varlıkları bir veritabanına yazar. Varolan bir veri bağlamı sınıfı seçin veya yeni bir şablon izin verirseniz Entity Framework Code First yaklaşımıyla çalışır *DbContext* sınıfı. Mevcut bir seçerseniz Entity Framework veritabanı First veya Model ilk yaklaşım ile de çalışır *ObjectContext* sınıfı.
- *Doğrulama*. Oluşturulan kodun ASP.NET MVC model bağlama ve meta veri özelliklerini kullanır, böylece form gönderilerini görselleştirip model sınıfınızın bildirilen kurallara göre doğrulanır. Bu gibi yerleşik doğrulama kuralı içerir *gerekli* ve *StringLength* öznitelikler ve özel doğrulama kuralları.
- *Bire çok ilişkileri*. Model sınıflarınızı arasında bire çok yabancı anahtar ilişkileri tanımlarsanız, ilgili varlıkları seçmek için açılır listede oluşturulan kod üretecektir. Örneğin, Entity Framework Code First kurallarına aşağıdaki model sınıfları tanımlayabilir: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Ne zaman, ardından iskelesini bir denetleyici için *ürün* sınıfı görünümlerini kullanıcıların seçim sağlayacaktır bir *kategori* her nesne *ürün* örneği.

    Bu şablon, ek seçenekler sağlayan *denetleyici Ekle* iletişim kutusu. İçin *Model sınıfı*, herhangi bir model sınıfı kullanıcılar oluşturma veya düzenleme olabilecek verilerin türünü belirler, çözümünüzdeki seçebilirsiniz:
- Entity Framework Code First kullanmak istiyorsanız, herhangi bir model sınıfı seçebilirsiniz.
- Entity Framework veritabanı First veya Entity Framework modelini First kullanıyorsanız, kavramsal modelde tanımlı bir varlık sınıfı'ı seçtiğinizden emin olun.

İçin *veri bağlamı sınıfı*, bu seçimler yapabilirsiniz:

- Code First kullanıyorsanız ve sınıf öğesini mevcut bir veri bağlamı istiyorsanız ** yeni veri bağlamı **. Bir veri bağlamı sınıfının sonra sizin için oluşturulur.
- Code First kullanıyorsanız ve varolan bir veri bağlamı sınıfı istiyorsanız, burada seçin. Seçtiğiniz model sınıfı kalıcı hale getirmek için güncelleştirilir.
- Veritabanı ilk veya Model ilk kullanıyorsanız, nesne bağlamı sınıfı seçin.

Görünümler, kullanmak istediğiniz görünüm altyapısı seçin veya tüm görünümleri iskelesini istemiyorsanız hiçbiri seçin.

Gelişmiş Optionsto seçebilirsiniz. daha fazla üretilmiş görünümleri için seçeneklerini belirtin. Örneğin, kullanılacak ana sayfa ve düzeni seçebilirsiniz.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Yapılan geliştirmeler "ASP.NET MVC 3 yeni proje" iletişim kutusu

Yeni ASP.NET MVC 3 projeleri oluşturmak için kullandığınız iletişim kutusu, aşağıda listelenen şekilde birden çok geliştirmeleri içerir.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Yeni "Intranet projesi" şablonu

Proje şablonu listesinde yeni bir Intranet uygulaması şablonu içerir. Bu şablon, form kimlik doğrulaması yerine Windows kimlik doğrulaması kullanarak bir web uygulaması oluşturmaya yönelik ayarları içerir. Bir intranet uygulama proje şablonunda kapsüllenmiş bazı IIS ayarlarını gerektirdiğinden, şablonu proje şablonu IIS'de çalışır hale getirme için yönergeler içeren bir benioku dosyası içerir. Belgeleri için yeni bir Intranet uygulaması şablonu, MSDN Web sitesinde şu URL'den ulaşabilirsiniz:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Proje şablonları, şimdi etkin HTML5

Yeni Proje iletişim kutusu artık proje şablonları için HTML5 özgü özellikler eklemek için bir seçenek içerir. Yeni HTML5 içeren oluşturulması görünüm neden seçeneğini belirleyerek `<header>`, `<footer>`, ve `<navigation>` öğeleri.

HTML5 özel etiketler desteklemeyen tarayıcılar önceki sürümlerini unutmayın. Bu sınırlama gidermek için Modernizr kitaplığına bir başvuruyu HTML5 proje şablonları içerir. (Sonraki bölüme bakın.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Proje şablonları artık Modernizr 1.7 içerir

Modernizr henüz bu özellikleri desteklemeyen tarayıcılar, CSS 3 ve HTML5 için destek sağlayan bir JavaScript kitaplıktır. Bu kitaplık şablonları ASP.NET MVC 3 projeleri için önceden yüklenmiş bir NuGet paketi olarak dahil edilir. Modernizr hakkında daha fazla bilgi için bkz: [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Proje şablonları içerir, jQuery, jQuery kullanıcı Arabirimi ve jQuery güncelleştirilmiş sürümlerini doğrulama

Proje şablonları, şimdi jQuery betikleri'nın şu sürümleri şunlardır:

- jQuery 1.5.1
- jQuery doğrulama 1.8
- jQuery kullanıcı Arabirimi 1.8.11

Bu kitaplıklar, önceden yüklenmiş NuGet paketleri olarak dahil edilir.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Proje şablonları, ADO.NET varlık çerçevesi 4.1 artık önceden yüklenmiş bir NuGet paketi olarak içerir.

ADO.NET Entity Framework 4.1 Code First özelliği içerir. Öncelikle var olan veritabanı ilk ve Model ilk desenleri için bir alternatif sağlayan ADO.NET Entity Framework için yeni bir geliştirme Düzen kodudur.

Kod, önce Visual Basic veya C# ile yazılmış POCO sınıfları ("düz eski CLR nesnelerini") kullanarak modelinizi tanımlama etrafında odaklanmıştır. Bu sınıflar, mevcut bir veritabanına eşlenebilir veya bir veritabanı şema oluşturmak için kullanılabilir. Ek yapılandırma kullanılarak sağlanabilir *DataAnnotations* öznitelikleri ya da fluent API'ler kullanarak.

Kod Firstwith ASP.NET MVC'yi kullanmaya yönelik belgeler ASP.NET Web sitesinde aşağıdaki URL'ler üzerinde kullanılabilir:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Proje şablonları JavaScript kitaplıklarını önceden yüklenmiş NuGet paketleri olarak içerir.

Yeni bir ASP.NET MVC 3 projesi oluşturduğunuzda, projeyi daha önce bahsedilen JavaScript dosyaları (Modernizr kitaplığı gibi) bunları yükleyerek içerir doğrudan komut proje şablonu betikler klasörüne eklemek yerine NuGet kullanma içeriği. Bu komut yeni sürümleri yayımlandığında betikleri en son sürüme güncelleştirmek için NuGet kullanmanıza olanak sağlar.

Örneğin, yeni jQuery yayınlar sıklığını göz önünde bulundurulduğunda, proje şablonuna dahil jQuery sürümü belirli bir noktada güncel olacaktır. JQuery yüklü bir NuGet paketi olarak dahil olduğundan, jQuery daha yeni sürümü var. ancak, NuGet iletişim kutusunda bildirilir.

JQuery dosya adında sürüm numarasını içerdiğinden, jQuery, en son sürüme güncelleştirme de güncelleştirilmesini gerektirir `<script>` yeni dosya adını kullanmak için jQuery dosyasına başvuruda etiketi. En son sürümlerine daha kolay güncelleştirilebilmesi için diğer dahil betik kitaplıkları betik adı, sürüm numarası dahil değildir.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- Bazı durumlarda, hata iletisi "yükleme (0x80070643) hata koduyla başarısız oldu ile" Kurulum başarısız olabilir. Bu soruna geçici bir çözüm hakkında daha fazla bilgi için bkz: [Bilgi Bankası makalesi 2531566](https://support.microsoft.com/kb/2531566).
- Denetleyici ekleme için yapı iskelesi Entity Framework içinde varlık devralma desteğinden yararlanabilir varlıkları için iskele değil. Örneğin, temel verilen *kişi* tarafından devralınan sınıf bir *Öğrenci* sınıf, yapı iskelesi *Öğrenci* sınıfı derlenmiyor üretilen kodda neden olur.
- Neden bir çözüm klasörü içinde yeni bir ASP.NET MVC 3 proje oluşturma bir *NullReferenceException* hata. Geçici çözüm, çözüm kök dizininde ASP.NET MVC 3 projesini oluşturmak ve ardından çözüm klasörüne taşımak sağlamaktır.
- ReSharper yüklü olduğunda, Razor sözdizimi için IntelliSense çalışmaz. ReSharper yüklü olması ve ASP.NET MVC 3'te Razor IntelliSense desteği yararlanmak istiyorsanız bakın [Razor IntelliSense ve ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri'nın blogunda, bunları birlikte bugün kullanmanın yolları bakın.
- Yükleme sırasında Lisans Koşulları'nı istenenden daha küçük bir pencerede EULA kabulü iletişim kutusu görüntüler.
- Ne zaman düzenlemekte olduğunuz bir Razor Görünüm (.cshtml veya. *vbhtml* dosyası), görünüm. ASP.NET MVC 3, Razor görünümleri için herhangi bir kod parçacıkları içermez... bir ASP.NET MVC için kod parçacığı aspxselecting için kod parçacıkları gösterir
- ASP.NET MVC 3, ASP.NET MVC 3 Visual Web Developer Express için Visual Studio yüklü olduğu bir bilgisayara yükleyin ve daha sonra Visual Studio'yu yükleyin, yeniden yüklemeniz gerekir. Visual Studio ve Visual Web Developer Express ASP.NET MVC 3 yükleyicisi tarafından yükseltilir bileşenleri paylaşın. ASP.NET MVC 3, Visual Studio için Visual Web Developer Express sahip ve ardından Visual Web Developer Express yükleyin bir bilgisayara yüklerseniz, aynı sorunu geçerlidir.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>ASP.NET MVC 3 RTM değişiklikleri

Bu bölümde, değişiklikler ve hata düzeltmeleri ASP.NET MVC 3 RTM sürümündeki RC2 çıkışından bu yana yapılan açıklanmaktadır.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Değişiklik: sürümü, jQuery kullanıcı Arabirimi 1.8.7 için güncelleştirildi.

Visual Studio için ASP.NET MVC proje şablonları, jQuery kullanıcı Arabirimi kitaplığı en son sürümünü içerecek şekilde güncelleştirildi. Şablonları ayrıca en küçük grup jQuery kullanıcı Arabirimi, ilişkili CSS ve görüntü dosyaları gibi gerekli kaynak dosyaları içerir.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Değişiklik: ModelMetadataProvider geri DataAnnotationsModelMetadataProvider için varsayılan değiştirildi

ASP.NET MVC 3'ün kullanıma sunulan RC2 sürümünü bir *CachedDataAnnotationsMetadataProvider* sınıfı, sağlanan mevcut üzerinde önbelleğe alma *DataAnnotationsModelMetadataProvider* olarak sınıf bir performans geliştirmesi. Ancak değişiklik geri ve kullanılabilir MVC vadeli projesine taşındı bu uygulama ile bazı hatalar raporlandı [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Düzeltildi: ters içindeki boşluk sonuçlarını içeren bir Razor ifadesinin parçası yapıştırma

Bir Razor dosyasına boşluk içeren bir Razor ifade parçası yapıştırdığınızda ASP.NET MVC 3 yayın öncesi sürümlerinde, elde edilen ifade ters çevrilir. Örneğin, aşağıdaki Razor kodu bloğu göz önünde bulundurun:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

İlk yöntem, bu durumda metni "bir ilk parametre"'i seçin ve ikinci yönteme bağımsız değişken olarak yapıştırın, sonuç aşağıdaki gibidir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Doğru yapıştırma işlemi aşağıdaki sonuç, davranıştır:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

İfade doğru yapıştırma işlemi sırasında korunur, böylece RTM sürümünde bu sorun düzeltilmiştir.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Düzeltildi: düzenleyicide açılan bir Razor dosyayı yeniden adlandırma söz dizimi renklendirme ve IntelliSense devre dışı bırakır

Dosya düzenleyici penceresinde açılır ancak Çözüm Gezgini'ni kullanarak Razor dosya yeniden adlandırılırken söz dizimi vurgulama ve IntelliSense, bu dosya için çalışmayı durdurmasına neden olur. Bu vurgulamayı olacak şekilde düzeltildi ve IntelliSense, yeniden adlandırma sonrasında korunur.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- NuGet Paket Yöneticisi konsolu açıkken, Visual Studio 2010 SP1 Beta kapatırsanız, Visual Studio kilitleniyor ve yeniden başlatmayı dener. Bu sorun Visual Studio 2010 SP1 RTM sürümünde.
- ASP.NET MVC 3 yükleyici yalnızca bir NuGet Paket Yöneticisi ilk sürümünü yüklemek kullanabilirsiniz. İlk sürüm yükledikten sonra NuGet yüklenebilir ve Visual Studio Uzantı Yöneticisi'ni kullanarak güncelleştirildi. Yüklü olan NuGet zaten varsa, NuGet en son sürüme güncelleştirmek için Visual Studio uzantı Galerisi gidin.
- Neden bir çözüm klasörü içinde yeni bir ASP.NET MVC 3 proje oluşturma bir *NullReferenceException* hata. Geçici çözüm, çözüm kök dizininde ASP.NET MVC 3 projesini oluşturmak ve ardından çözüm klasörüne taşımak sağlamaktır.
- Yükleyici tamamlamak için ASP.NET MVC önceki sürümlerinden çok daha uzun sürebilir. Visual Studio 2010 'un bileşenleri güncelleştirmeleri olmasıdır.
- ReSharper yüklü olduğunda, Razor sözdizimi için IntelliSense çalışmaz. ReSharper yüklü olması ve ASP.NET MVC 3'te Razor IntelliSense desteği yararlanmak istiyorsanız bakın [Razor IntelliSense ve ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri'nın blogunda, bunları birlikte bugün kullanmanın yolları bakın.
- ASP.NET MVC 3'ün Beta sürümü ile oluşturulmuş CCSHTML ve VBHTML görünümler doğru şekilde ayarlanması, derleme eylemine sahip, proje yayımlandığında bu görüntülemek sonucuyla türleri göz ardı edilir. Bu dosyalar için derleme eylemi değeri "İçerik" olarak ayarlanmalıdır. ASP.NET MVC 3 RTM yeni dosyalar için bu sorunu giderir ancak yayın öncesi sürümler ile oluşturulmuş bir projeyi için varolan dosyaları ayarı düzeltmek değil.
- ![](mvc3-release-notes/_static/image3.png)
- Yükleme sırasında Lisans Koşulları'nı istenenden daha küçük bir pencerede EULA kabulü iletişim kutusu görüntüler.
- Razor görünümü (.cshtml dosyası) düzenlerken, denetleyici Git menü öğesi Visual Studio'da kullanılabilir olmaz ve hiçbir kod parçacıkları vardır.
- ASP.NET MVC 3, ASP.NET MVC 3 Visual Web Developer Express için Visual Studio yüklü olduğu bir bilgisayara yükleyin ve daha sonra Visual Studio'yu yükleyin, yeniden yüklemeniz gerekir. Visual Studio ve Visual Web Developer Express ASP.NET MVC 3 yükleyicisi tarafından yükseltilir bileşenleri paylaşın. ASP.NET MVC 3, Visual Studio için Visual Web Developer Express sahip ve ardından Visual Web Developer Express yükleyin bir bilgisayara yüklerseniz, aynı sorunu geçerlidir.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Yeni Değişiklikler

- ASP.NET MVC önceki sürümlerinde, filtreleri eylem bazı durumlarda dışında istek başına oluşturun. Bu davranış hiçbir zaman garantili bir davranış ancak yalnızca bir uygulama ayrıntısı ve sözleşme filtreleri için durum bilgisi olmayan dikkate alınması gereken şeklindeydi. ASP.NET MVC 3'te filtreleri daha agresif bir biçimde önbelleğe alınır. Bu nedenle, örnek durumu yanlış depolayan herhangi bir özel eylem filtre bozuk olabilir.
- Özel durum filtreleri için yürütme sırasını aynı olan özel durum filtreleri değişti *sipariş* değeri. ASP.NET MVC 2 ve önceki sürümlerinde, aynı denetleyicisinde özel durum filtreleri *sipariş* değeri gibi bir eylem yöntemi üzerindekiler eylem yöntemi özel durum filtreleri önce yürütülür. Özel durum filtreleri uygulandığında bu durum genelde olacaktır belirtilen olmadan *sipariş* değeri. Böylece en belirli özel durum işleyicisi yürütür ASP.NET MVC 3'te, bu Sıralamayı tersine çevrildi. Önceki sürümlerinde olduğu gibi *sipariş* özelliği açıkça belirtildiğinde, belirtilen sırada çalıştırılan filtreler.
- Adlı yeni bir özellik *FileExtensions* eklenmişse *VirtualPathProviderViewEngine* temel sınıfı. ASP.NET (Ada göre değil) yoluyla bir görünüm göründüğünde, bu yeni özelliği tarafından belirtilen listede yer alan bir dosya uzantısına sahip tek görünüm olarak kabul edilir. Bir özel dosya uzantısı Web Form görünümleri için etkinleştirmek için bir özel yapı sağlayıcısı kayıtlı olduğunda ve bir tam yol yerine bir adı kullanarak bu görünümleri sağlayıcı başvuran bir uygulamalarda değişiklik budur. Geçici çözüm değerini değiştirmektir *FileExtensions* özel dosya uzantısı içerecek şekilde özelliği.
- Doğrudan uygulayan özel denetleyici üreteci uygulamaları *IControllerFactory* arabirimi, yeni bir uygulamasını sağlaması gerektiği *GetControllerSessionBehavior* olan yöntemi Bu sürümde arabirimine eklendi. Genel olarak, bu, değil doğrudan bu arabirimi uygulayan ve bunun yerine, sınıfından türetilen önerilir *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>ASP.NET MVC 3 RC2'deki değişiklikler

Bu bölümde, ASP.NET MVC 3 RC2 yayın RC sürümünden sonra yapılan değişiklikler (yeni özellikler ve hata düzeltmeleri) açıklanmaktadır.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Proje şablonları jQuery 1.4.4, jQuery doğrulama 1.7 ve jQuery kullanıcı Arabirimi 1.8.6 içerecek şekilde değiştirildi.

ASP.NET MVC 3 için'proje şablonları artık jQuery, jQuery doğrulaması ve jQuery en son sürümleri içerir kullanıcı Arabirimi. jQuery kullanıcı Arabirimi yeni eklenen proje şablonları ve kullanışlı bir kullanıcı arabirimi pencere öğeleri sağlar. JQuery kullanıcı Arabirimi hakkında daha fazla bilgi için kendi giriş sayfasını ziyaret edin: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Eklenen "AdditionalMetadataAttribute" sınıfı

Kullanabileceğiniz *AdditionalMetadataAttribute* doldurmak için sınıf *ModelMetadata.AdditionalValues* model özelliğine sözlüğü.

Örneğin, bir görünüm modeli yalnızca yönetici görüntülenen özellikler olduğunu varsayalım. Bu model aşağıdaki örnekteki gibi bir değer olarak anahtar ve true olarak AdminOnly kullanarak yeni bir öznitelik ile açıklama eklenebilir:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Bir ürün görünümü model yeniden işlendiğinde bu meta veriler herhangi bir görüntü veya Düzenleyicisi şablon için kullanılabilir hale getirilir. Bu size, meta veri bilgilerini yorumlamak için uygulama geliştiricisi olarak bağlıdır.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Geliştirilmiş görünüm iskele kurma

T4 şablonlarını yapı iskelesi görünümleri için artık kullanılan şablon Yardımcısı yöntemlere yapılan çağrılar gibi oluşturan *EditorFor* gibi Yardımcıları yerine *TextBoxFor*. Bu değişiklik Görünüm Ekle iletişim kutusu bir görünümünü oluşturur meta veri modeli biçiminde veri ek açıklama öznitelikleri için desteği geliştirir.

Görünüm Ekle yapı iskelesi, ayrıca geliştirilmiş bir algılama ve birincil anahtar bilgileri standardına göre modeli kullanımını içerir. Örneğin, Görünüm Ekle iletişim kutusu, birincil anahtar değeri bir düzenlenebilir bir forma alan olarak başladınız değil emin olmak için bu bilgileri kullanır.

Varsayılan düzenleme ve oluşturma şablonları istemci doğrulama için jQuery betikleri başvurular içerir.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Eklenen Html.Raw yöntemi

Varsayılan olarak, Razor alt yapısı HTML olarak kodlar tüm değerleri görüntüleyin. Örneğin, sayfa olarak görüntülenir, böylece aşağıdaki kod parçacığı HTML Karşılama değişkeni içinde kodlar `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Yeni *Html.Raw* yöntemi içeriği güvenli olarak bilindiğinde kodlanmamış HTML görüntüleme, basit bir yol sağlar. Aşağıdaki örnek aynı dize görüntüler, ancak dize biçimlendirmesi olarak işlenir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Özellik adı "Controller.ViewModel" ve "ViewBag" "Görünüm" özelliğine

Daha önce *ViewModel* özelliği *denetleyicisi* için corresponded *görünümü* görünüm özelliği. Bu özelliklerin her ikisi de erişim değerleri için bir yol sağlama *ViewDataDictionary* dinamik özellik erişimcisi sözdizimi kullanılarak nesne. Her iki özellik, Karışıklığı önlemek için ve daha tutarlı olarak aynı olacak şekilde değiştirilmiştir.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>"SessionStateAttribute" olarak yeniden adlandırıldı "ControllerSessionStateAttribute" sınıfı

*ControllerSessionStateAttribute* sınıf, ASP.NET MVC 3 RC sürümünde tanıtılmıştır. Daha fazla Sözün özelliği değiştirildi.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Yeniden adlandırılan RemoteAttribute "Alanlar" özelliğini "AdditionalFields"

*RemoteAttribute* sınıfın *alanları* özelliği kullanıcılar arasında karışıklığa neden oldu. Bu özelliğe yeniden adlandırma *AdditionalFields* konuşmanın niyetini açıklar.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>"AllowHtmlAttribute" için "SkipRequestValidationAttribute" olarak yeniden adlandırıldı

*SkipRequestValidationAttribute* özniteliği adlandırıldı *AllowHtmlAttribute* daha iyi hedeflenen kullanımını göstermek için.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Değiştirilen "Html.ValidationMessage" yöntemi ilk kullanışlı hata iletisini görüntüler

*Html.ValidationMessage* yöntemi yalnızca ilk hata görüntüleme yerine ilk kullanışlı hata mesajını göstermeye sabit.

Model bağlama sırasında *ModelState* sözlük modelden dahil olmak üzere bu özellikle ilgili hata iletileri ile birden çok kaynaktan doldurulabilir (bunu uygularsa *IValidatableObject* ), özellik erişim sırasında oluşturulan özel durumlar ve özelliğine uygulanan doğrulama öznitelikleri.

Zaman *Html.ValidationMessage* yöntemi bir doğrulama iletisi görüntüler, çünkü bunlar genellikle son kullanıcı için tasarlanmamıştır, bir özel durum içeren model durumu girişleri atlar. Bunun yerine, yöntemi bir özel durumla ilişkili değil ve bu iletiyi görüntüleyen bir ilk doğrulama iletisi görünür. Bu tür bir ileti bulunursa, ilk özel durum ile ilişkili bir genel hata iletisini varsayılan kullanır.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Sabit @model boşluk belgeye ekleme bildirimi

Önceki sürümlerde, <em>@model</em> üst görünüm bildirimi eklenip boş bir satır için işlenen HTML çıkışı. Bildirimi boşluk sunmaz, bu düzeltilmiştir.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Görünüm altyapıları motoru özel dosya adları desteklemek için eklenen "FileExtensions" özelliğini

Bir görünüm altyapısı aşağıdaki örnekteki gibi bir açık görünüm yolunu kullanarak görünüm döndürebilirsiniz:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

İlk Görünüm altyapısı görünüm işlemek her zaman çalışır. Varsayılan olarak, Web Forms görünüm altyapısı ilk görünüm altyapısı bulunur. Razor Görünümü Web Forms altyapısı işlenemiyor, bir hata oluşur. Görünüm altyapısı artık sahip bir *FileExtensions* destekledikleri hangi dosya uzantılarını belirtmek için kullanılan özellik. Bu özellik, ASP.NET, görünüm altyapısının bir dosya işlenip işlenmeyeceğini belirler denetlenir. Bu bir değişiklik ve daha fazla ayrıntı dahil [bozucu değişiklikleri](#_Toc2_BC) bu belgenin bölüm.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Sabit "LabelFor" Yardımcısı "İçin" özniteliği için doğru değeri yayma

Bir hatanın nerede düzeltilip *LabelFor* işlenen yöntemi bir *için* eşleşen öznitelik *giriş* öğenin *adı* yerine özniteliği kimliğini W3C göre *için* özniteliği eşleşmelidir *giriş* öğenin kimliği.

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Model bağlama sırasında açık değerler öncelik vermek için sabit "RenderAction" yöntemi

Önceki sürümlerde için geçirilmiş açık değerler *RenderAction* yöntemi yok sayıldı geçerli form değerlerini yerine bir alt eylem içinde model bağlama sırasında. Açık değerler öncelikli model bağlama sırasında düzeltme sağlar.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Yeni Değişiklikler

- ASP.NET MVC önceki sürümlerde, birkaç durumda dışında istek başına eylem filtrelerini oluşturuldu. Bu davranış hiçbir zaman garantili bir davranış ancak yalnızca bir uygulama ayrıntısı ve sözleşme filtreleri için durum bilgisi olmayan dikkate alınması gereken şeklindeydi. ASP.NET MVC 3'te filtreleri daha agresif bir biçimde önbelleğe alınır. Bu nedenle, örnek durumu yanlış depolayan herhangi bir özel eylem filtre bozuk olabilir.
- Özel durum filtreleri için yürütme sırasını aynı olan özel durum filtreleri değişti *sipariş* değeri. ASP.NET MVC 2 ve önceki sürümlerinde, aynı olan denetleyicisinde özel durum filtreleri *sipariş* değeri gibi bir eylem yöntemi üzerindekiler eylem yöntemi özel durum filtreleri önce yürütüldü. Özel durum filtreleri uygulandığında bu durum genelde olacaktır belirtilen olmadan *sipariş* değeri. Böylece en belirli özel durum işleyicisi yürütür ASP.NET MVC 3'te, bu Sıralamayı tersine çevrildi. Önceki sürümlerinde olduğu gibi *sipariş* özelliği açıkça belirtildiğinde, belirtilen sırada çalıştırılan filtreler.
- Adlı yeni bir özellik *FileExtensions* eklenmişse *VirtualPathProviderViewEngine* temel sınıfı. ASP.NET (Ada göre değil) yoluyla bir görünüm göründüğünde, bu yeni özelliği tarafından belirtilen listede yer alan bir dosya uzantısına sahip tek görünüm olarak kabul edilir. Bir özel dosya uzantısı Web Form görünümleri için etkinleştirmek için bir özel yapı sağlayıcısı kayıtlı olduğunda ve bir tam yol yerine bir adı kullanarak bu görünümleri sağlayıcı başvuran bir uygulamalarda değişiklik budur. Geçici çözüm değerini değiştirmektir *FileExtensions* özel dosya uzantısı içerecek şekilde özelliği.
- Doğrudan uygulayan özel denetleyici üreteci uygulamaları <em>IControllerFactory</em> arabirimi, yeni bir uygulamasını sağlaması gerektiği <em>GetControllerSessionBehavior</em>  <em>Bu sürümde arabirimine eklendi yöntemi</em>. Genel olarak, bu, değil doğrudan bu arabirimi uygulayan ve bunun yerine, sınıfından türetilen önerilir <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- ASP.NET MVC 3 yükleyici yalnızca bir NuGet Paket Yöneticisi ilk sürümünü yüklemek kullanabilirsiniz. İlk sürüm yükledikten sonra NuGet yüklenebilir ve Visual Studio Uzantı Yöneticisi'ni kullanarak güncelleştirildi. Yüklü olan NuGet zaten varsa, NuGet en son sürüme güncelleştirmek için Visual Studio uzantı Galerisi gidin.
- Neden bir çözüm klasörü içinde yeni bir ASP.NET MVC 3 proje oluşturma bir *NullReferenceException* hata. Geçici çözüm, çözüm kök dizininde ASP.NET MVC 3 projesini oluşturmak ve ardından çözüm klasörüne taşımak sağlamaktır.
- Yükleyici tamamlamak için ASP.NET MVC önceki sürümlerinden çok daha uzun sürebilir. Visual Studio 2010 'un bileşenleri güncelleştirmeleri olmasıdır.
- ReSharper yüklü olduğunda, Razor sözdizimi için IntelliSense çalışmaz. ReSharper yüklü olması ve ASP.NET MVC 3 RC2'deki Razor IntelliSense desteği yararlanmak istiyorsanız bakın [Razor IntelliSense ve ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri'nın blogunda, bunları birlikte bugün kullanmanın yolları bakın.
- ASP.NET MVC 3 Beta sürümüyle oluşturulan CSHTML ve VBHTML görünümler doğru şekilde ayarlanması, derleme eylemine sahip, proje yayımlandığında bu görüntülemek sonucuyla türleri göz ardı edilir. *Derleme eylemi* bu dosyaların içeriğini ayarlanmalıdır için bir değer ". ASP.NET MVC 3 RC2, yeni dosyalar için bu sorunu giderir ancak Beta sürümü ile oluşturulmuş bir projeyi için varolan dosyaları ayarı düzeltmek değil.![](mvc3-release-notes/_static/image4.png)
- Yükleme sırasında Lisans Koşulları'nı istenenden daha küçük bir pencerede EULA kabulü iletişim kutusu görüntüler.
- Razor görünümü (.cshtml dosyası) düzenlerken, denetleyici Git menü öğesi Visual Studio'da kullanılabilir olmaz ve hiçbir kod parçacıkları vardır.
- ASP.NET MVC 3, ASP.NET MVC 3 Visual Web Developer Express için Visual Studio yüklü olduğu bir bilgisayara yükleyin ve daha sonra Visual Studio'yu yükleyin, yeniden yüklemeniz gerekir. Visual Studio ve Visual Web Developer Express ASP.NET MVC 3 yükleyicisi tarafından yükseltilir bileşenleri paylaşın. ASP.NET MVC 3, Visual Studio için Visual Web Developer Express sahip ve ardından Visual Web Developer Express yükleyin bir bilgisayara yüklerseniz, aynı sorunu geçerlidir.
- ASP.NET MVC 3 RC 2'yi yükleme yüklü zaten varsa, NuGet güncelleştirmez. NuGet yükseltmek için Visual Studio Uzantı Yöneticisi'ni ve onu gidin kullanılabilir bir güncelleştirme olarak gösterilmesi gerekir. NuGet, buradan en son sürüme yükseltebilirsiniz.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Sürüm Adayı

ASP.NET MVC Sürüm Adayı 9 Kasım 2010'da yayımlanmıştır.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>ASP.NET MVC 3 rc'deki yenilikleri

Bu bölümde sunulan özellikler açıklanır ASP.NET MVC 3 RC sürümünden itibaren Beta sürümü.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet Paket Yöneticisi

ASP.NET MVC 3, NuGet paket (eski adıyla NuPack bilinir) Yöneticisi olan bir tümleşik paket yönetim aracını kitaplıkları ve araçları, Visual Studio projelerine ekleme içerir. Bu araç, geliştiricilerin hemen bir kitaplık, kaynak ağacına almak için attığınız adımlar otomatikleştirir.

NuGet ile bir komut satırı aracı, tümleşik bir konsol penceresi içinde Visual Studio 2010, Visual Studio bağlam menüsünden ve PowerShell cmdlet'leri kümesi olarak çalışabilir.

NuGet hakkında daha fazla bilgi için okuma [Nuget belgeleri](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>"Yeni Proje" Gelişmiş iletişim kutusu

Yeni bir proje oluşturduğunuzda, yeni proje iletişim kutusu artık bir ASP.NET MVC proje türü yanı sıra, görünüm altyapısının belirtmenize olanak sağlar.

![](mvc3-release-notes/_static/image5.png)

Bu sürümde şablonları ve görünüm altyapıları iletişim kutusunda listelenen listesini değiştirmek için destek eklenmiştir.

Varsayılan Şablonlar aşağıda verilmiştir:

Boş. Varsayılan dizin yapısı ASP.NET MVC projeleri için ASP.NET MVC stilleri varsayılan ve varsayılan JavaScript dosyalarını içeren bir betik dizin içeren bir Site.css dosyası dahil olmak üzere bir ASP.NET MVC projesi dosyaları en az bir kümesini içerir.

Internet uygulaması. ASP.NET MVC ile üyelik sağlayıcıyı kullanacak şekilde nasıl erişileceğini gösteren örnek işlevleri içerir.

Windows kayıt defterinde iletişim kutusunda görüntülenen proje şablonları listesinde belirtilir.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Oturumsuz denetleyicileri

Yeni *ControllerSessionStateAttribute* belirterek denetleyicileri için oturum durumu davranışı üzerinde daha fazla denetim verir bir [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) numaralandırma değeri.

Aşağıdaki örnek, tüm istekleri bir denetleyici için oturum durumu devre dışı bırakmak gösterilmektedir.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Aşağıdaki örnek, bir denetleyiciye tüm istekler için salt okunur oturum durumu ayarlamak gösterilmektedir.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Yeni doğrulama öznitelikleri

#### <a name="compareattribute"></a>CompareAttribute

Yeni *CompareAttribute* doğrulama özniteliği bir modelin iki farklı özelliklerin değerlerini karşılaştırmanıza olanak tanır. Aşağıdaki örnekte, *ComparePassword* özelliği eşleşmelidir *parola* geçerli olması için alan.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Yeni *RemoteAttribute* doğrulama özniteliği gerçek Doğrulama mantığı gerçekleştiren sunucu üzerinde bir yöntemi çağırmak istemci tarafı doğrulamasını etkinleştirir jQuery doğrulama eklentinin uzak doğrulayıcısını yararlanır.

Aşağıdaki örnekte, *kullanıcıadı* özelliği *RemoteAttribute* uygulanır. Bu özellik düzenleme Görünümü'nde düzenlerken, istemci doğrulama adlı bir eylem çağırır *UserNameAvailable* üzerinde *UsersController* Bu alan doğrulayabilmek sınıfı.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Aşağıdaki örnek, karşılık gelen denetleyicisi gösterilir.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Varsayılan olarak, öznitelik uygulanan özellik adı, eylem yöntemi için bir sorgu dizesi parametresi gönderilir.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>"LabelFor" ve "LabelForModel" yöntemleri için yeni aşırı yüklemeler

Yeni aşırı yüklemeler için eklenmiştir *LabelFor* ve *LabelForModel* olanak tanıyan yöntemler etiket metni belirtin. Aşağıdaki örnek, bu aşırı yüklemeler kullanma işlemini gösterir.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Alt eylem çıktı önbelleği

*OutputCacheAttribute* kullanılarak çağrıldığı alt işlemlerin çıktı önbelleği destekleyen *Html.RenderAction* veya *Html.Action* yardımcı yöntemler. Aşağıdaki örnek, başka bir eylem çağıran bir görünümü gösterir.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate* eylem ile ek açıklamalı *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Bu kod çalıştığında, Html.Action("GetDate") çağrısının sonucunu 100 saniye için önbelleğe alınır.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"Görünümü Ekle" iletişim kutusu iyileştirmeleri

Kesin türü belirtilmiş Görünüm Ekle, Görünüm Ekle iletişim kutusu artık önceki sürümlerde, çok sayıda çekirdek .NET Framework türleri gibi daha fazla geçerli olmayan türleri filtreler. Ayrıca, liste artık sınıf adı ve türleri bulmayı kolaylaştıran tam olarak nitelenmiş tür adı değil göre sıralanır. Örneğin, tür adı artık aşağıdaki örnekte olduğu gibi görüntülenir:

ClassName (ad alanı)

Önceki sürümlerde, bu aşağıdaki gibi görüntülendi:

: ADALANI.SınıfAdı

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Ayrıntılı isteği doğrulama

*Hariç* özelliği *ValidateInputAttribute* artık yok. Bunun yerine, belirli özellikleri bir model için model bağlama sırasında atlandı istek doğrulama için yeni kullanın *SkipRequestValidationAttribute*.

Örneğin, bir eylem yöntemi bir blog gönderisi düzenlemek için kullanılan varsayalım:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Aşağıdaki örnek, bir blog gönderisi için Görünüm modeli gösterir.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Model bağlama, bir kullanıcı Description özelliği için bazı biçimlendirme gönderdiğinde, istek doğrulama nedeniyle başarısız olur. İstek doğrulamanın açıklama blog gönderisi için model bağlama sırasında devre dışı bırakmak için uygulama *SkipRequpestValidationAttribute* Bu örnekte gösterildiği gibi özelliğine:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Modelin her özelliği için istek doğrulamayı devre dışı bırakmak için alternatif olarak, uygulama *ValidateInputAttribute* değeriyle *false* eylem yöntemi için:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Yeni Değişiklikler

- Özel durum filtreleri için yürütme sırasını aynı olan özel durum filtreleri değişti *sipariş* değeri. ASP.NET MVC 2 ve önceki sürümlerinde, aynı olan denetleyicisinde özel durum filtreleri *sipariş* gibi bir eylem yöntemi üzerindekiler eylem yöntemi özel durum filtreleri önce yürütüldü. Özel durum filtreleri uygulandığında bu durum genelde olacaktır belirtilen olmadan *sipariş* değeri. Böylece en belirli özel durum işleyicisi yürütür ASP.NET MVC 3'te, bu Sıralamayı tersine çevrildi. Önceki sürümlerinde olduğu gibi *sipariş* özelliği açıkça belirtildiğinde, belirtilen sırada çalıştırılan filtreler.
- Adlı yeni bir özellik eklendi *FileExtensions* için *VirtualPathProviderViewEngine* temel sınıfı. Bir görünüm yoluyla (ve ada göre değil) yer alan bir dosya uzantısı yalnızca bir görünümlerle baktığımda, bu yeni özelliği tarafından belirtilen liste olarak kabul edilir. Kullanıcıların web form görünümleri için bir özel dosya uzantısını etkinleştirmek için bir özel yapı sağlayıcıyı kaydettirin ve bir tam yol yerine bir adı kullanarak bu görünümleri başvuran bir değişiklik budur. Geçici çözüm değerini değiştirmektir *FileExtensions* özel dosya uzantısı içerecek şekilde özelliği.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Bilinen Sorunlar

- Yükleyici bileşenleri Visual Studio 2010 'un güncelleştirdiğinden tamamlamak için ASP.NET MVC önceki sürümlerinden çok daha uzun sürebilir.
- Görünüm Ekle iskele astrongly seçerken görünümü iskelesini kurar salt yazılır özellikler yazdınız. Bu, her zaman yapı iskelesi tarafından yoksayılacak. Görünüm Ekle iletişim kutusu bir "Düzenleme" veya "Oluştur" görünümü oluştururken salt okunur özelliklerini de iskele oluşturulduğunu. Salt okunur özellikler yalnızca görüntüleme ve liste görünümleri için iskele kurulmuş.
- ASP.NET MVC 3 ile birlikte zaman uyumsuz CTP yüklü olduğunda, hata ayıklama çalışmıyor. ASP.NET MVC 3 yüklü yan yana zaman uyumsuz CTP sahip olamaz. Hata ayıklama onarmak için zaman uyumsuz CTP kaldırın. Daha fazla bilgi edinmek için [bu blog gönderisini](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) ASP.NET MVC 3 RC tüm parçalarını kaldırma hakkında.
- Resharper yüklü olduğunda, razor IntelliSense çalışmaz. ReSharper yüklü olması ve ASP.NET MVC 3 okuyun RC sürümünde Razor IntelliSense desteği yararlanmak istiyorsanız [bu blog gönderisini](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) gelen JetBrains, bunları birlikte bugün kullanma yolları açıklanmaktadır.
- Beta, ASP.NET MVC 3 ile oluşturulan CSHTML ve VBHTML görünümleri, yapı eylemi doğru bunları yayımlama atlar yoktur. *Derleme eylemi* için bu dosyalar "İçerik" olarak ayarlanmalıdır. ASP.NET MVC 3 RC, yeni dosyalar için bu sorunu giderir ancak Beta sürümü ile oluşturulmuş bir projeyi için varolan dosyaları ayarı düzeltmek değil.
- Yükleyici bileşenleri Visual Studio 2010 'un güncelleştirdiğinden tamamlamak için ASP.NET MVC önceki sürümlerinden çok daha uzun sürebilir.
- Görünüm Ekle iskele "Düzenle" kesin seçerek görünümü iskelesini kurar yazıldığında yalnızca özelliklerini okuyun. Benzer şekilde, salt yazılır Özellikler "Display" görünümler için iskele kurulmuş.
- Yükleme sırasında Lisans Koşulları'nı istenenden daha küçük bir pencerede EULA kabulü iletişim kutusu görüntüler.
- Visual Studio zaman uyumsuz CTP yükleme aracınızın ASP.NET MVC 3 bir parçası olarak dahil edilir Razor sürümüyle bir çakışmasına neden oluyor. Visual Studio zaman uyumsuz CTP hem Razor yayın aynı makinede yüklemeye çalışmayın emin olun.
- Razor görünümü (.cshtml dosyası) düzenlerken, denetleyici Git menü öğesi Visual Studio'da kullanılabilir olmaz ve hiçbir kod parçacıkları vardır.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta 6 Ekim 2010'da yayımlanmıştır. Aşağıdaki notları Beta sürümüne özeldir ve herhangi bir güncelleştirme veya değişiklik yukarıdaki ASP.NET MVC 3 Sürüm Adayı bölümünde başvurulan tabidir.

## <a id="0.1__Toc274034215"></a>  Yeni Featuresin ASP.NET MVC 3 Beta

<a id="0.1__Default_validation_system"></a>Bu bölümde sunulan özellikler açıklanır ASP.NET MVC 3 Beta sürümünde.

### <a id="0.1__Toc274034216"></a>  NuGet Paket Yöneticisi

ASP.NET MVC 3 bir tümleşik paket yönetim aracını ekleme kitaplıkları ve araçları Visual Studio projeleri için NuGet Paket Yöneticisi'ni içerir. Çoğunlukla, geliştiricilerin hemen bir kitaplık, kaynak ağacına almak için attığınız adımlar otomatikleştirir.

NuGet ile bir komut satırı aracı, tümleşik bir konsol penceresi içinde Visual Studio 2010, Visual Studio bağlam menüsünden ve PowerShell cmdlet'leri kümesi olarak çalışabilir.

NuGet hakkında daha fazla bilgi için okuma [NuGet belgeleri](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Yeni Proje iletişim kutusu İyileştirildi

Yeni bir proje oluşturduğunuzda, yeni proje iletişim kutusu artık bir ASP.NET MVC proje türü yanı sıra, görünüm altyapısının belirtmenize olanak sağlar.

![](mvc3-release-notes/_static/image6.png)

Bu sürümde şablonları ve görünüm altyapıları iletişim kutusunda listelenen listesini değiştirmek için destek dahil edilmez.

Varsayılan Şablonlar aşağıda verilmiştir:

Boş. Varsayılan dizin yapısı ASP.NET MVC projeleri için ASP.NET MVC stilleri varsayılan ve varsayılan JavaScript dosyalarını içeren bir betik dizin içeren küçük bir Site.css dosyası dahil olmak üzere bir ASP.NET MVC projesi dosyaları en az bir kümesini içerir.

Internet uygulaması. ASP.NET MVC içindeki üyelik sağlayıcısının nasıl kullanılacağını gösteren örnek işlevleri içerir.

### <a id="0.1__Toc274034218"></a>  Kesin belirtmek için basitleştirilmiş bir yolu modelleri Razor görünümleri yazdınız.

Model türü kesin olarak belirlenmiş Razor görünümleri belirtmenin bir yolu, yeni kullanarak basitleştirilmiştir @model CSHTML görünümler için yönerge ve @ModelType VBHTML görünümler için yönerge. ASP.NET MVC eski sürümleri, bu şekilde kesin türü belirtilmiş bir model için Razor görünümleri belirtmeniz gerekir:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

Bu sürümde, aşağıdaki söz dizimini kullanabilirsiniz:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Yeni ASP.NET Web sayfaları için yardımcı yöntemler desteği

Yeni ASP.NET Web Pages teknolojisi, görünümleri ve denetleyicileri için yaygın olarak kullanılan işlevler eklemek için kullanışlı yardımcı yöntemler kümesi içerir. Bu yardımcı yöntemler denetleyicileri ve görünümleri (uygun olduğunda) kullanarak ASP.NET MVC 3 destekler. Bu yöntemler System.Web.Helpers derlemede yer alır. Birkaç ASP.NET Web Pages yardımcı yöntemler aşağıdaki tabloda listelenmektedir.

| **Yardımcısı** | **Açıklama** |
| --- | --- |
| Grafik | Bir Grafik görünümündeki işler. Chart.ToWebImage Chart.Save ve Chart.Write gibi yöntemler içerir. |
| Şifreleme | Karma algoritmaları düzgün bir şekilde oluşturmak için kullandığı salted ve parolaların karma. |
| WebGrid | Nesneler (genellikle, bir veritabanındaki verileri) koleksiyonunu bir kılavuz işler. Sayfalama ve sıralama destekler. |
| WebImage | Bir görüntü oluşturur. |
| Web postası | Bir e-posta iletisi gönderir. |

Temel söz dizimi ve Yardımcıları listeleyen bir hızlı başvuru konu kullanılabilir ASP.NET Razor söz dizimi belgeler şu URL'de bir parçası olarak:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Ek bağımlılık ekleme desteği

ASP.NET MVC 3 Önizleme 1 sürüm üzerinde oluşturma, geçerli sürümde, iki yeni hizmet ve dört var olan hizmetleri için destek eklendi ve bağımlılık çözümlemesi ve genel hizmet bulucu için geliştirilmiş destek içerir.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Ayrıntılı denetleyicisi örnekleme için yeni IControllerActivator arabirimi

Yeni IControllerActivator arabirimi denetleyicileri bağımlılık ekleme örneği nasıl oluşturulur üzerinde daha ayrıntılı denetim sağlar. Aşağıdaki örnek, bir arabirimi gösterir:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Bu denetleyici üretecini rolüne karşılaştırın. Denetleyici üreteci denetleyici türü bulmak ve bu denetleyici türünün örneğini oluşturmak için sorumludur IControllerFactory arabirimi uygulamasıdır.

Denetleyici etkinleştiricileri yalnızca denetleyici türünün örneğini oluşturmak için sorumludur. Denetleyici türü aramasında gerçekleştirmeyin. Uygun denetleyici türü bulunduktan sonra denetleyici üreteçlerini IControllerActivator örneğine gerçek denetleyici örneği oluşturulmasını işlemek için temsilci olarak atanmalıdır.

DefaultControllerFactory sınıfın IControllerFactory örneği kabul eden yeni bir oluşturucusu vardır. Bu, varsayılan denetleyici türü arama davranışı değiştirmek zorunda kalmadan denetleyicisi oluşturmanın bu yönden yönetmek için bağımlılık ekleme uygulamanıza olanak sağlar.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Iservicelocator arabirimi Idependencyresolver ile değiştirildi

Topluluk geri bildirimi doğrultusunda, ASP.NET MVC 3 Beta sürümü, ASP.NET MVC ihtiyaçlarını belirli bir slimmed aşağı Idependencyresolver arabirimi Iservicelocator arabirimi kullanımını değiştirilmiştir. Aşağıdaki örnek, yeni bir arabirimi gösterir:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Bu değişikliğin bir parçası olarak ServiceLocator sınıfı da DependencyResolver sınıfı ile değiştirilmiştir. ASP.NET MVC eski sürümleri için bir bağımlılık çözümleyiciyi kaydı benzer:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Bu arabirimin uygulamaları yalnızca istenen tür için kayıtlı hizmet sağlamak üzere temel alınan bağımlılık ekleme kapsayıcısına temsilci.

İstenen türün kayıtlı hizmeti olmadığında, ASP.NET MVC GetService null döndürmek için ve GetServices boş koleksiyon döndürmek için bu arabirimin uygulamalarını bekler.

Yeni DependencyResolver sınıfı kaydetme yeni Idependencyresolver arabirim veya genel hizmet bulucu arabirimini (Iservicelocator) uygulayan sınıflar sağlar. Genel hizmet bulucu hakkında daha fazla bilgi için bkz: [github'da CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Ayrıntılı Görünüm sayfası örnekleme için yeni IViewActivator arabirimi

Yeni IViewPageActivator arabirimi görünüm sayfalarının bağımlılık ekleme nasıl örneği oluşturulur üzerinde daha ayrıntılı denetim sağlar. Bu, hem WebFormView örnekleri hem de RazorView örnekleri için geçerlidir. Aşağıdaki örnek, yeni bir arabirimi gösterir:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Bu sınıfların artık olanak sağlayan ViewPage ViewUserControl ve WebViewPage türleri örneği nasıl oluşturulur denetlemek için bağımlılık ekleme kullanma IViewPageActivator oluşturucu bağımsız değişken kabul eder.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Var olan hizmetleri için yeni bağımlılık çözümleyici desteği

Yeni sürüm bağımlılık çözünürlük desteği için aşağıdaki hizmetleri içerir:

- Model doğrulama sağlayıcılarının. Bağımlılık Çözümleyicisi ModelValidatorProvider uygulayan sınıflar kaydedilebilir ve sistemin onları istemci ve sunucu tarafı doğrulamayı desteklemek için kullanır.
- Model meta veri sağlayıcısı. Bağımlılık Çözümleyicisi ModelMetadataProvider uygulayan bir tek sınıf kaydedilebilir ve Sistem şablonu oluşturma ve doğrulama sistemleri için meta verilerini sağlamak için kullanır.
- Değer sağlayıcıları. Bağımlılık Çözümleyicisi ValueProviderFactory uygulayan sınıflar kaydedilebilir ve sistem bunları denetleyicisi tarafından ve model bağlama sırasında kullanılan değer sağlayıcıları oluşturmak için kullanır.
- Model bağlayıcılar. Bağımlılık Çözümleyicisi IModelBinderProvider uygulayan sınıflar kaydedilebilir ve sistem bunları model bağlama sistemi tarafından kullanılan bir model bağlayıcılar oluşturmak için kullanır.

### <a id="0.1__Toc274034221"></a>  Yeni bir örtük jQuery tabanlı Ajax desteği

ASP.NET MVC Ajax yardımcı yöntemler aşağıdaki gibi içerir:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Bu yöntemler, tam bir geri gönderme kullanma yerine sunucu üzerinde bir eylem yöntemini çağırmak için JavaScript kullanır. Bu işlev, jQuery örtük bir şekilde yararlanmak için güncelleştirildi. Satır içi istemci betiklerini intrusively yayma yerine bu yardımcı yöntemler davranışı biçimlendirmeden kullanarak HTML5 özniteliklerini yayma tarafından ayrı *veri ajax* önek. Davranışı, uygun JavaScript dosyalarına başvuruda bulunarak işaretlemede sonra uygulanır. Aşağıdaki JavaScript dosyaları yapıldığından emin olun:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Bu özellik, ASP.NET MVC 3 yeni proje şablonları Web.config dosyasında varsayılan olarak etkindir, ancak mevcut projeleri için varsayılan olarak devre dışıdır. Daha fazla bilgi için [eklenen birçok farklı uygulama bayrakları için istemci doğrulama ve unobtrusive JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) bu belgenin devamındaki.

### <a id="0.1__Toc274034222"></a>  Örtük jQuery doğrulaması için yeni destek

Varsayılan olarak, ASP.NET MVC 3 Beta istemci tarafı doğrulama yapabilmek için örtük bir şekilde jQuery doğrulaması kullanır. Örtük istemci doğrulamasını etkinleştirmek için bir görünüm içindeki aşağıdaki gibi bir çağrı yapın:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Bu, ViewContext.UnobtrusiveJavaScriptEnabled özelliği aşağıdaki çağrısı yaparak bunu True olarak ayarlandığını gerektirir:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Ayrıca aşağıdaki JavaScript dosyaları başvurulduğundan emin olun.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Bu özellik, ASP.NET MVC 3 yeni proje şablonları Web.config dosyasında varsayılan olarak etkinleştirilir, ancak mevcut projeleri için varsayılan olarak devre dışıdır. Daha fazla bilgi için [istemci doğrulama ve unobtrusive JavaScript için yeni uygulama çapında bayrakları](#0.1_AddedApplicationWideFlagsForClientValida) bu belgenin devamındaki.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  İstemci doğrulama ve Unobtrusive JavaScript için yeni uygulama çapında bayrakları

Etkinleştirmek veya istemci doğrulama ve genel olarak aşağıdaki örnekte olduğu gibi HtmlHelper sınıfının statik üyeleri kullanarak örtük JavaScript devre dışı bırakabilirsiniz:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Varsayılan proje şablonları, varsayılan olarak örtük JavaScript'i etkinleştirir. Etkinleştirebilir veya aşağıdaki ayarları kullanarak, uygulamanızın kök Web.config dosyasında bu özellikleri devre dışı bırak:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Bu özellikler varsayılan olarak etkin olduğundan, yeni aşırı yüklemeler aşağıdaki örneklerde gösterildiği gibi varsayılan ayarları geçersiz kılacak olanak tanıyan HtmlHelper sınıfı yaptınız:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Geriye dönük uyumluluk için bu özelliklerin her ikisi de varsayılan olarak devre dışıdır.

### <a id="0.1__Toc274034224"></a>  Görünümleri çalıştırmadan önce çalışan kod için yeni destek

Adlı bir dosya artık koyabilirsiniz \_viewstart.cshtml (veya \_viewstart.vbhtml) görünümleri dizinde ve bu dizin ve alt dizinlerinde birden çok görünüm arasında paylaşılacak bu kodu ekleyin. Örneğin, aşağıdaki kodu bırakabilecek \_~/Views klasörde viewstart.cshtml sayfa:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Bu düzen sayfası görünümleri klasördeki her bir görünüm ve onun alt klasörleri yinelemeli olarak tüm ayarlar. Ne zaman bir görünüm işlenirken, kodda \_viewstart.cshtml dosya kodu görüntüle çalışmadan önce çalışır. \_o klasördeki her görünüm viewstart.cshtml kod uygular.

Varsayılan olarak, kodda \_viewstart.cshtml dosya herhangi bir alt klasöründeki görünümler için de geçerlidir. Ancak, ayrı ayrı alt klasörleri kendi sürümü olabilir \_viewstart.cshtml dosya; bu durumda, yerel sürüm önceliklidir. Örneğin, tüm görünümlere HomeController için ortak olan kodu çalıştırmak için put bir \_~/Views/Home klasöründe viewstart.cshtml dosyası.

### <a id="0.1__Toc274034225"></a>  VBHTML Razor sözdizimi için yeni destek

Önceki ASP.NET MVC tabanlı C# üzerinde Razor sözdizimi kullanılarak görünümleri için destek önizlemesi. Bu görünümler, .cshtml uzantısına kullanın. Razor desteklemeye devam eden iş işleminin bir parçası olarak, ASP.NET MVC 3 Beta .vbhtml uzantısına kullanan Visual Basic'te, Razor sözdizimi için destek sunuyor.

Şu URL'de VBHTML sayfaları Visual Basic sözdizimini kullanarak, bir giriş için bkz:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  ValidateInputAttribute üzerinde daha ayrıntılı denetim

ASP.NET MVC, gelen istek kötü amaçlı olabilecek Giriş içermediğinden emin olmak için çekirdek ASP.NET isteği doğrulama altyapısını çağırır ValidateInputAttribute sınıfı her zaman eklemiştir. Varsayılan olarak, giriş doğrulaması etkin. Aşağıdaki örnekte olduğu gibi ValidateInputAttribute özniteliği kullanılarak istek doğrulamayı devre dışı bırakmak mümkündür:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Ancak, birçok web uygulamaları kalan alanları gerektiği HTML izin ver gereken form alanlarını vardır. ValidateInputAttribute sınıfı artık istek doğrulama dahil edilmemesi gereken alanların listesini belirtmenize olanak tanır.

Örneğin, blog altyapısıdır ve geliştiriyorsanız gövde ve Özet alanları işaretlemeye izin isteyebilirsiniz. Bu alanlar, özellik adı ("Body" ve "Özet") karşılık gelen bir ad özniteliği ile her iki input öğesi tarafından temsil edilebilir. İstek devre dışı bırakmak için bu alanlar için doğrulama yalnızca (-ValidateInput sınıfın, aşağıdaki örnekte olduğu gibi dışlama özelliği, virgülle ayrılmış) adlarını belirtir:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Yardımcıları kısa çizgiler için alt çizgi, anonim nesneleri kullanarak belirtilen HTML öznitelik adları için Dönüştür.

Yardımcı yöntemler aşağıdaki örnekteki gibi bir anonim nesnenin kullanarak öznitelik ad/değer çiftleri belirtmenizi sağlar:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

ASP.NET'te bir özellik adı için bir kısa çizgi kullanılamaz çünkü bu yaklaşım, öznitelik adı kısa çizgi kullanın izin vermez. Ancak, kısa çizgi özel HTML5 öznitelikleri için önemlidir; Örneğin, HTML5 "data-" ön eki kullanır.

Aynı zamanda, alt çizgi HTML öznitelik adları için kullanılamaz, ancak özellik adları içinde geçerlidir. Bu nedenle, bir anonim nesnenin kullanarak özniteliklerini belirtirseniz ve öznitelik adları alt çizgi eklerseniz, yardımcı yöntemler alt çizgileri kısa çizgiler için dönüştürür. Örneğin, alt çizgi aşağıdaki yardımcı sözdizimini kullanır:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Yardımcı çalıştığında, önceki örnekte aşağıdaki biçimlendirme oluşturur:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Hata düzeltmeleri

Varsayılan nesne şablonu EditorFor ve DisplayFor şablon Yardımcıları DisplayAttribute.Order özelliğinde belirtilen sıralama artık desteklemektedir. (Önceki sürümlerde, sipariş ayarı kullanılmadı.)

İstemci doğrulama artık doğrulama uygulanan doğrulama öznitelikleri geçersiz kılınan özellikleri destekler.

JsonValueProviderFactory artık varsayılan olarak kaydedilir.

## <a id="0.1__Toc274034229"></a>  Bozucu değişiklikler

Özel durum filtreleri için yürütme sırasını aynı sırası değerine sahip özel durum filtreleri değişti. Eylem yöntemi özel durum filtreleri önce bir eylem yöntemi üzerindekiler dahil edilen gibi aynı sırada denetleyiciyle üzerinde özel durum ASP.NET MVC 2 ve önceki sürümlerinde, filtreler. Belirtilen bir sıra değeri özel durum bir filtre uygulansaydı, bu durum genellikle olacaktır. Böylece en belirli özel durum işleyicisi yürütür ASP.NET MVC 3'te, bu Sıralamayı tersine çevrildi. Order özelliğini açıkça belirtilmediği takdirde, daha önceki sürümlerinde olduğu gibi belirtilen sırada filtreleri çalıştırılır.

## <a id="0.1__Toc274034230"></a>  Bilinen sorunlar

Yükleme sırasında Lisans Koşulları'nı istenenden daha küçük bir pencerede EULA kabulü iletişim kutusu görüntüler.

Razor görünümleri ya da söz dizimi vurgulama IntelliSense desteği yoktur. Visual Studio'da Razor sözdizimi için destek daha yeni bir sürümünü bir parçası olarak dahil edilecek beklenen.

Razor görünümü (CSHTML dosyası) düzenlerken <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> denetleyicisi Git menü öğesi Visual Studio'da kullanılabilir olmayacak ve hiçbir kod parçacıkları vardır.

Kullanırken @model kesin türü belirtilmiş CSHTML belirtmek için söz dizimini görüntülemek, türleri için dile özgü kısayolları tanınmıyor. Örneğin, @model int çalışmaz, ancak @model Int32 çalışır. Geçici çözüm bu hata için model türü belirttiğinizde gerçek tür adı kullanmaktır.

Kullanırken @model kesin türü belirtilmiş bir CSHTML görünüm belirtmek için sözdizimi (veya @ModelType kesin türü belirtilmiş bir VBHTML görünüm belirtmek için), boş değer atanabilir türler ve dizi bildirimleri desteklenmez. Örneğin, @model int? desteklenmiyor. Bunun yerine, `@model Nullable<Int32>`. Söz dizimi @model string [] ayrıca desteklenmiyor; bunun yerine, `@model IList<string>`.

Bir ASP.NET MVC 2 projesini ASP.NET MVC 3'e yükselttiğinizde, aşağıdaki Web.config dosyasına appSettings bölümünü ekleyin emin olun:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Kimliği doğrulanmamış kullanıcılar ~/Account/oturum açma Web.config dosyasında kullanılan forms kimlik doğrulaması ayarı yok, her zaman yeniden yönlendirmek form kimlik doğrulaması neden olan bilinen bir sorun yoktur. Aşağıdaki uygulama ayarı eklemek için çözüm olabilir.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Sorumluluk reddi

© 2011 Microsoft Corporation. Tüm hakları saklıdır. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Bunu kullanarak riski size aittir.

Bu belge ile herhangi bir Microsoft ürünü üzerinde hiçbir fikri mülkiyet hakkı sağlamaz. Kopyalayabilir ve dahili olarak, bu belgeyi kullanmak amacıyla başvuru.
