---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Bu belgede, ASP.NET MVC 4 sürümü açıklanmaktadır.
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: 9e4d73349c4c01e0717aabe7da9cb126d2c545c0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826584"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Bu belgede, ASP.NET MVC 4 sürümü açıklanmaktadır.


- [Yükleme notları](#_Toc303253802)
- [Belgeler](#_Toc303253803)
- [Destek](#_Toc303253804)
- [Yazılım gereksinimleri](#_Toc303253805)
- [ASP.NET MVC 4'deki yeni özellikler](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Varsayılan proje şablonları geliştirmeler](#_Toc303253808)
    - [Mobil proje şablonu](#_Toc303253809)
    - [Görüntü modları](#_Toc303253810)
    - [jQuery Mobile, görünüm değiştirici ve tarayıcı geçersiz kılma](#_Toc303253811)
    - [Görev zaman uyumsuz denetleyicileri desteği](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Veritabanı geçişlerini](#_Toc303253818)
    - [Boş proje şablonu](#_Toc303253819)
    - [Denetleyici için herhangi bir proje klasörü ekleme](#_Toc303253820)
    - [Paketleme ve Küçültme](#_Toc303253821)
    - [Oturum açma bilgileri Facebook ve OAuth ve Openıd kullanarak diğer sitelere etkinleştirme](#_Toc303253822)
- [Bir ASP.NET MVC 3 projesini ASP.NET MVC 4 için yükseltme](#_Toc303253806)
- [ASP.NET MVC 4 Sürüm Adayı değişiklikleri](#_Toc303253817)
- [Bilinen sorunlar ve yeni değişiklikler](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Yükleme notları

Visual Studio 2010 için ASP.NET MVC 4 yüklenebilir [ASP.NET MVC 4 giriş sayfası](../mvc/mvc4.md) Web Platformu Yükleyicisi'ni kullanarak.

ASP.NET MVC 4'ü yüklemeden önce ASP.NET MVC 4 önceden yüklenmiş tüm önizlemeleri kaldırma öneririz. Kaldırmadan, ASP.NET MVC 4'e ve ASP.NET MVC 4 Beta Sürüm Adayı yükseltebilirsiniz.

Bu sürüm, tüm .NET Framework 4.5 Önizleme sürümleriyle uyumlu değildir. Ayrı olarak herhangi bir yüklü Önizleme sürümü .NET Framework 4.5, ASP.NET MVC 4'ü yüklemeden önce en son sürüme yükseltmeniz gerekir.

ASP.NET MVC 4, yüklü ve çalışma yan yana ASP.NET MVC 3 ile olabilir.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Belgeler

ASP.NET MVC için belgeleri MSDN Web sitesinde şu URL'den ulaşabilirsiniz:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Öğreticiler ve ASP.NET MVC hakkında diğer bilgiler Web sitesinin ASP.NET MVC 4 sayfasında kullanılabilir ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Destek

ASP.NET MVC 4 tam olarak desteklenir. Bu sürümle birlikte çalışma hakkında sorularınız varsa, ayrıca bunları ASP.NET MVC foruma ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), burada ASP.NET topluluk üyelerinin resmi olmayan destek sık sağlayabilir.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET MVC 4 bileşenleri Visual Studio için PowerShell 2.0 ve hizmet paketi 1 ile Visual Studio 2010 veya Visual Web Developer Express 2010 Service Pack 1 gerektirir.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>ASP.NET MVC 4'deki yeni özellikler

Bu bölümde sunulan özellikler açıklanır ASP.NET MVC 4 sürümünde.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 bir çeşit tarayıcılar ve mobil cihazlar dahil olmak üzere istemciye ulaşan HTTP Hizmetleri oluşturmaya yönelik yeni bir çerçeve olan ASP.NET Web API içerir. ASP.NET Web API aynı zamanda RESTful hizmetleri oluşturmak için ideal bir platformdur.

ASP.NET Web API desteği için aşağıdaki özellikleri içerir:

- **Modern HTTP programlama modeli:** doğrudan erişmek ve HTTP isteklerini ve yanıtlarını yeni, kesin türü belirtilmiş HTTP nesne modelini kullanarak Web API'leri. Aynı programlama modeli ve HTTP ardışık düzen istemcide yeni simetrik kullanılabilir *HttpClient* türü.
- **Tam yollar için destek:** ASP.NET Web API kümesini rota parametrelerinin ve kısıtlamalar dahil olmak üzere ASP.NET yönlendirmesi için rota özelliklerini destekler. Ayrıca, HTTP yöntemleri için Eylemler eşlemek için basit kuralları kullanın.
- **İçerik anlaşması:** istemci ve sunucu birlikte bir web API'si tarafından döndürülen veriler için doğru biçimde belirlemek için çalışabilir. ASP.NET Web API, XML, JSON, varsayılan desteği sağlar ve Form URL'SİNİN kodlanmış biçimleri ve bu destek kendi biçimlendiricileri ekleyerek genişletebilir, ve hatta varsayılan içerik anlaşması stratejisi değiştirin.
- **Model bağlama ve doğrulama:** Model bağlayıcıları bir HTTP isteğinin çeşitli parçalarını verileri ayıklamak ve ileti bölümlerinde Web API eylemlerini tarafından kullanılan .NET nesnelerine dönüştürmek için kolay bir yol sağlar. Doğrulama ayrıca veri ek açıklamalarını tabanlı eylem parametrelerini gerçekleştirilir.
- **Filtreler:** ASP.NET Web API destekler iyi bilinen filtreler gibi filtreler *[Authorize]* özniteliği. Yazar ve eylem, yetkilendirme ve özel durum işleme için kendi filtrelerinizi takın.
- **Sorgu oluşturma:** kullanım *[Queryable]* döndüren bir eylem filtresi özniteliği *Iqueryable* etkinleştir OData sorgu kuralları ile web API sorgulama desteği için.
- **Gelişmiş Test Edilebilirlik:** statik bağlam nesneleri ayar HTTP ayrıntıları yerine web API eylemlerini iş örnekleriyle *HttpRequestMessage* ve *HttpResponseMessage*. Hızlı bir şekilde, Web API işlevleri için birim testleri yazma kullanmaya başlamak için Web API projesi yanı sıra birim testi projesi oluşturun.
- **Kod tabanlı yapılandırma:** bırakarak, yapılandırma dosyalarını temizlemek, ASP.NET Web API configuration, yalnızca kod gerçekleştirilir. Genişletilebilirlik noktaları yapılandırmak için sağlanan hizmet bulucu düzeni kullanın.
- **Geliştirilmiş denetimi tersine çevirme (IOC) kapsayıcı desteği:** ASP.NET Web API aracılığıyla geliştirilmiş bağımlılık çözümleyici soyutlaması IOC kapsayıcılar için mükemmel destek sağlar
- **Barındırma:** Web API'leri barındırılabilir IIS yanı sıra kendi işleminizde yollar'ın tüm gücünden ve diğer özellikleri Web API'sinin hala kullanırken.
- **Özel Yardım oluşturmak ve test sayfalarının:** artık kolayca özel bir Yardım derleyebilir ve test sayfalarının web Apı'leriniz için yeni kullanarak *IApiExplorer* web API'leri tam çalışma zamanı açıklamasını almak için hizmeti.
- **İzleme ve Tanılama:** ASP.NET Web API artık hafif izleme System.Diagnostics, ETW ve üçüncü taraf günlük altyapılarına gibi mevcut günlük çözümleriyle tümleştirmek kolaylaştıran bir altyapı sağlar. Sağlayarak izlemeyi etkinleştirebilirsiniz bir *ITraceWriter* uygulama ve web API yapılandırmanıza ekleme.
- **Bağlantı oluşturma:** ASP.NET Web API'sini *UrlHelper* aynı uygulamada ilgili kaynaklara bağlantılar oluşturmak için.
- **Web API projesi şablonu:** yeni Web API projesi formu hızla ASP.NET Web API'sini kullanmaya başlamak için yeni MVC 4 Proje Sihirbazı'nı seçin.
- **Yapı iskelesi:** kullanım **denetleyici Ekle** hızla bir Entity Framework tabanlı bir web API denetleyicisi iskelesinin nasıl iletişim tabanlı model türü.

ASP.NET Web API hakkında daha fazla bilgi edinmek için lütfen [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Varsayılan proje şablonları geliştirmeler

Yeni ASP.NET MVC 4 projeleri oluşturmak için kullanılan şablon daha modern görünümlü bir Web sitesi oluşturmak için güncelleştirilmiştir:

![](mvc4-release-notes/_static/image1.png)

Yüzeysel iyileştirmelerinin yanı sıra yeni şablonu işlevindeki var. geliştirdi. Şablon iyi masaüstü tarayıcıları ve mobil tarayıcılar özelleştirme olmadan aramak için Uyarlamalı işleme olarak adlandırılan tekniği kullanır.

![](mvc4-release-notes/_static/image2.png)

Uyarlamalı işleme iş başında görmek için mobil öykünücü kullanın veya yalnızca daha küçük olacak şekilde Masaüstü tarayıcı penceresini yeniden boyutlandırmayı deneyin. Tarayıcı penceresini yeterince aldığında sayfasının düzenini değiştirir.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobil proje şablonu

Yeni bir proje başlatılıyor ve tablet tarayıcılar ve mobil için özel bir site oluşturmak istiyorsanız, yeni mobil uygulaması proje şablonu kullanabilirsiniz. Bu, jQuery Mobile, dokunma özelliği iyileştirilmiş bir kullanıcı Arabirimi oluşturmak için bir açık kaynak kitaplığı, dayalıdır:

![](mvc4-release-notes/_static/image3.png)

Bu şablon Internet uygulama şablonu aynı uygulama yapısını içerir (ve denetleyici kodlarının neredeyse aynıdır), ancak düzgün görünmesini ve davranmasını dokunmatik tabanlı mobil cihazlarda jQuery Mobile kullanılarak stil uygulanmış. Yapısı ve stil mobil kullanıcı Arabirimi hakkında daha fazla bilgi için bkz. [jQuery Mobile proje Web sitesi](http://jquerymobile.com/).

Mobil için iyileştirilmiş görünümlerine eklemek istediğiniz veya tek bir site oluşturmak istiyorsanız farklı bir stilde görünümleri Masaüstü ve mobil tarayıcılara hizmet veren bir masaüstü odaklı site zaten varsa, yeni görüntü modları özelliğini kullanabilirsiniz. (Sonraki bölüme bakın.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Görüntü modları

Yeni görüntü modları özelliği isteği yapan tarayıcının bağlı olarak görünümleri seçin bir uygulama sağlar. Örneğin, bir masaüstü tarayıcısı giriş sayfasını isterse, uygulama Views\Home\Index.cshtml şablonu kullanabilir. Bir mobil tarayıcıda giriş sayfası isterse, uygulama Views\Home\Index.mobile.cshtml şablon döndürebilir.

Ayrıca düzenleri ve kısmi bölümleri için belirli bir tarayıcı türleri geçersiz kılınabilir. Örneğin:

- Görünümler/paylaşılan klasörünüzde her ikisi de varsa \_Layout.cshtml ve \_Layout.mobile.cshtml şablonları, uygulamayı kullanacağı varsayılan \_Layout.mobile.cshtml mobil tarayıcılar ve gelenisteklerisırasında\_Layout.cshtml diğer istekleri sırasında.
- Her ikisi de bir klasör içeriyorsa \_MyPartial.cshtml ve \_MyPartial.mobile.cshtml, yönerge @Html.Partial("\_MyPartial") işlenir \_MyPartial.mobile.cshtml mobil gelen istekleri sırasında Tarayıcılar ve \_MyPartial.cshtml diğer istekleri sırasında.

Daha özel görünümler, düzenler veya diğer cihazlar için kısmi görünümler oluşturmak istiyorsanız, yeni bir kaydedebilirsiniz *DefaultDisplayMode* , bir istek belirli koşullar sağladığında aramak için ad belirtmek için örneği. Örneğin, aşağıdaki kodu ekleyebilirsiniz *uygulama\_Başlat* dize "iPhone" Apple iPhone tarayıcı bir istekte bulunduğunda, geçerli bir görüntü modu kaydetmek için Global.asax dosyasındaki yöntemi:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Bir Apple iPhone tarayıcı bir istekte bulunduğunda bu kod çalıştırıldıktan sonra görünümler/paylaşılan uygulamanızın kullanacağı\\_Layout.iPhone.cshtml Düzen (varsa). Görüntü modu hakkında daha fazla bilgi için bkz. [ASP.NET MVC 4 mobil özellikleri](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). DisplayModeProvider kullanan uygulamalar yüklemelisiniz [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet paketi. [ASP.NET Fall 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) içerir [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet paketini yeni proje şablonları. Bkz: [ASP.NET MVC 4 mobil önbelleğe alma hata Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) düzeltme hakkında ayrıntılı bilgi için.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile ve mobil özellikler

ASP.NET MVC 4 ile mobil uygulamalar oluşturma hakkında bilgi için jQuery Mobile bkz öğretici [ASP.NET MVC 4 mobil özellikleri](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Görev zaman uyumsuz denetleyicileri desteği

Zaman uyumsuz eylem yöntemi artık bir nesne türü döndüren olarak tek yöntemler yazabilirsiniz *görev* veya *görev&lt;actionresult öğesini&gt;*.

 Daha fazla bilgi için [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4, Windows Azure SDK 1.6 ve daha yeni sürümleri destekler.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Veritabanı geçişlerini

ASP.NET MVC 4 projeleri artık, Entity Framework 5 de içerir. Entity Framework 5'te harika özelliklerden biri, veritabanı geçişlerini desteğidir. Bu özellik, kolayca kod odaklı bir geçiş kullanarak veritabanındaki verileri korurken kullanarak veritabanı şemanızı gelişmesi sağlar. Veritabanı geçişlerini hakkında daha fazla bilgi için bkz. [film modeli ve tablosuna yeni alan ekleme](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) içinde [ASP.NET MVC 4 öğreticiye giriş](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Boş proje şablonu

Tamamen temiz bir sayfadan başlayabilmesi MVC boş proje şablonu artık gerçekten boştur. Boş proje şablonu önceki sürümü için temel olarak adlandırıldı.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Denetleyici için herhangi bir proje klasörü ekleme

Artık sağ tıklayın seçin ve **denetleyici Ekle** MVC projenizdeki herhangi bir klasörden. Bu, MVC ve Web API denetleyicilerinin ayrı klasörlerde tutma dahil olmak üzere istediğiniz, ancak denetleyicilerinizden düzenlemek için daha fazla esneklik sağlar.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Paketleme ve küçültme

Paketleme ve küçültme framework, bir Web sayfası, betik ve CSS ile birlikte gelen, tek bir dosyaya tek tek dosyaları birleştirerek hale getirin gerekiyor HTTP isteklerinin sayısını azaltmak sağlar. Paket içeriğini küçültme göre bu isteklerin toplam boyutunu ardından azaltabilir. Küçültme, hatta kendi semantiği dayalı CSS Seçici daraltma için değişken adlarını kısaltmak için boşluk ortadan gibi etkinlikler dahil edebilirsiniz. Paketleri bildirildi ve yapılandırılmış kod ve bu kolayca başvurulan görünümlerinde ya da oluşturan yardımcı yöntemler aracılığıyla tek bir paket bağlantı veya hata ayıklarken birden çok tek tek Paket içeriğini bağlar. Daha fazla bilgi için [paketleme ve küçültme](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Oturum açma bilgileri Facebook ve OAuth ve Openıd kullanarak diğer sitelere etkinleştirme

ASP.NET MVC 4 Internet proje şablonu varsayılan şablonlarında artık DotNetOpenAuth kitaplığının kullanarak, OAuth ve Openıd oturum açma desteği içerir. Bir OAuth veya Openıd sağlayıcısı yapılandırma hakkında daha fazla bilgi için bkz. [WebForms ve MVC Web sayfaları için OAuth/Openıd Destek](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) ve [OAuth ve Openıd özellik belgeleri ASP.NET Web Pages'de](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Bir ASP.NET MVC 3 projesini ASP.NET MVC 4 için yükseltme

ASP.NET MVC 4, aynı bilgisayarda bir ASP.NET MVC 3 uygulama ASP.NET MVC 4 Yükseltme zamanı seçme esnekliği sağlayan ASP.NET MVC 3 ile yan yana yüklenebilir.

Yeni ASP.NET MVC 4 proje ve kopya oluşturmak için tüm görünümleri, denetleyicisi, kod ve içerik dosyaları var olan MVC 3 projeden yeni projeye ve ardından derlemeyi güncelleştirmek için herhangi bir MVC olmayan şablon eşleşecek şekilde yeni proje başvurularının yükseltmek için en kolay yolu olan kullanmakta olduğunuz klenen assembiles. MVC 3 projesini Web.config dosyasında değişiklik yaptıysanız, MVC 4 proje içinde Web.config dosyasına da bu değişiklikleri birleştirmeniz gerekir.

Mevcut bir ASP.NET MVC 3 uygulama sürüm 4 için el ile yükseltmek için aşağıdakileri yapın:

1. Tüm Web.config dosyasında (var. bir proje, görünümleri klasöründe, diğeri görünümleri klasöründe, projenizdeki her bölgenin kökünde) proje dosyalarında aşağıdaki metni her örneğini değiştirin (Not: System.Web.WebPages, sürüm = 1.0.0.0 bulunamadı Visual Studio 2012 ile oluşturulan projeleri): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    Aşağıdaki karşılık gelen metinle:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Kök Web.config dosyasında güncelleştirme *webPages:Version* "2.0.0.0" öğesine ve yeni bir *PreserveLoginUrl* "true" değerini anahtar: 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. Çözüm Gezgini'ndeki başvurular üzerinde sağ tıklayın ve NuGet paketlerini Yönet'i seçin. Sol bölmede seçin **Online\NuGet resmi paket kaynağı**, sonra da aşağıdakileri güncelleştirin:

    - ASP.NET MVC 4
    - (İsteğe bağlı) jQuery, jQuery doğrulama ve jQuery kullanıcı Arabirimi
    - (İsteğe bağlı) Varlık çerçevesi
    - (Optonal) Modernizr
4. Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından projeyi seçin. Sonra adı tekrar sağ tıklayıp Düzen *ProjectName*.csproj.
5. Bulun *ProjectTypeGuids* öğesi ve {E3E379DF-F4C6-4180-9B81-6769533ABE47} {E53F8FEA-EAE0-44A6-8774-FFD645390401} değiştirin.
6. Değişiklikleri kaydedin, düzenlemekte olduğunuz proje (.csproj) dosyayı kapatın, projeye sağ tıklayın ve ardından projeyi seçin.
7. ASP.NET MVC'ın önceki sürümleri kullanılarak derlenmiş herhangi bir üçüncü taraf kitaplıklar proje başvuru yapıyorsa, kök Web.config dosyasını açın ve aşağıdaki üç Ekle *bindingRedirect* altındaki öğeleri  *Yapılandırma* bölümü: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>ASP.NET MVC 4 Sürüm Adayı değişiklikleri

ASP.NET MVC 4, Sürüm Adayı sürüm notlarını buradan ulaşabilirsiniz:

ASP.NET MVC 4, Sürüm Adayı'ndan büyük değişiklikler bu sürümde, aşağıda özetlenmiştir:

- **Denetleyici yapılandırması başına:** ASP.NET Web API denetleyicisi uygulayan özel bir öznitelik ile öznitelikli *IControllerConfiguration* kendi biçimlendiricileri, eylem seçiciyi ve parametre bağlayıcıları ayarlama . *HttpControllerConfigurationAttribute* kaldırıldı.
- **Rota ileti işleyicileri başına:** belirli bir rota için istek zincirindeki son ileti işleyicisi artık belirtebilirsiniz. Bu yolculuk boyunca kendi için gönderme yönlendirmeyi kullanmak çerçeveler için destek sağlar (olmayan*IHttpController*) uç noktalar.
- **İlerleme bildirimleri:** *ProgressMessageHandler* hem yüklenen istek varlıkları ve indirilen yanıt varlıkları için ilerleme bildirimi oluşturur. Bu işleyici kullanarak ne kadar bir istek gövdesi karşıya yükleme veya yanıt gövdesi indirme izlemek mümkündür.
- **İçerik Gönderme:** *PushStreamContent* sınıfı doğrudan istek veya yanıt (eşzamanlı veya zaman uyumsuz olarak) bir akış kullanarak yazmak için bir veri üreticisinin burada istediği senaryoları sağlar. Zaman *PushStreamContent* çağırır için bir eylem temsilcisi ile çıkış akışına veri kabul etmeye hazır. Akışa yazılırken sürece gerekli ve Kapat tamamlandı için geliştirici ardından akışa yazabilirsiniz. *PushStreamContent* akışın kapanış algılar ve zaman uyumsuz temel tamamlar *görev* içeriği teslim yazma.
- **Hata yanıtları oluşturma:** kullanım *HTTP hatası* hala uygularken çalışırken hata bilgileri doğrulama hataları ve özel durumlar gibi tutarlı bir şekilde göstermek için tür *IncludeErrorDetailPolicy*. Yeni *CreateErrorResponse* kolayca ile hata yanıtları oluşturmak için genişletme yöntemleri *HTTP hatası* içeriği. *HTTP hatası* içeriktir tam olarak içerik anlaşması.
- **MediaRangeMapping kaldırıldı:** medya türü aralıkları artık varsayılan içerik uzlaştırıcı tarafından işlenir.
- **Basit tür parametreleri için varsayılan parametre bağlaması, artık [FromUri]:** önceki sürümlerinde ASP.NET Web API basit tür parametreleri kullanılan model bağlama için varsayılan parametre bağlaması. Basit tür parametreleri için varsayılan parametre bağlaması sunulmuştur *[FromUri]*.
- **Eylem Seçimi gerekli parametreleri geliştirir:** ASP.NET Web API eylem seçimi artık yalnızca seçerseniz bir eylem URI'den gelen tüm gerekli parametreler sağlanır. Bir parametre isteğe bağlı eylem yönteminin imzası bağımsız değişkeni için bir varsayılan değer sağlayarak belirtilebilir.
- **HTTP parametresi bağlamaları özelleştirme:** kullanın *ParameterBindingAttribute* belirli bir eylem parametresi için parametre bağlaması özelleştirme veya *ParameterBindingRules* üzerinde *HttpConfiguration* parametresi bağlamaları özelleştirmek için daha geniş.
- **MediaTypeFormatter geliştirmeleri:** Biçimlendiricileri artık tam erişime sahip *HttpContent* örneği.
- **Ana Bilgisayar arabelleğe alma İlkesi Seçimi:** uygulama ve yapılandırma *IHostBufferPolicySelector* zaman arabelleğe alma kullanılacaksa İlkesi belirlemek için ana bilgisayarları etkinleştirmek için ASP.NET Web API hizmeti.
- **İstemci sertifikaları bir konak dilden bağımsız bir şekilde erişim:** kullanım *GetClientCertificate* istek iletisinden sağlanan istemci sertifikasını almak için genişletme yöntemi.
- **İçerik anlaşması genişletilebilirlik:** türeterek içerik anlaşması özelleştirme *DefaultContentNegotiator* ve istediğiniz içerik anlaşması herhangi bir özelliği geçersiz kılma.
- **406 Kabul edilemez yanıtlarını döndürmek için destek:** oluşturarak uygun bir biçimlendirici bulunamazsa 406 Kabul edilemez yanıtları ASP.NET Web API'de artık döndürebilir bir *DefaultContentNegotiator* ile*excludeMatchOnTypeOnly* parametresini *true*.
- **Form verileri NameValueCollection veya JToken okuyun:** URI sorgu dizesinde veya istek gövdesi olarak form verileri okuyabilir bir *NameValueCollection* kullanarak *ParseQueryString* ve  *ReadAsFormDataAsync* genişletme yöntemlerini sırasıyla. Benzer şekilde, URI sorgu dizesinde veya istek gövdesi olarak form verileri okuyabilir bir *JToken* kullanarak *TryReadQueryAsJson* ve *ReadAsAsync*&lt;T&gt; genişletme yöntemlerini sırasıyla.
- **Çok bölümlü geliştirmeleri:** yazmak artık mümkündür bir *MultipartStreamProvider* , tamamen uyarlanmış bunu okuyun ve böylelikle kullanıcı için en iyi şekilde sonuç sunmak MIME çok bölümlü veri türü. Bir işlem sonrası adımı şirket bağlayabilirsiniz *MultipartStreamProvider* ne olursa olsun post istediği MIME çok bölümlü gövde bölümü üzerinde işlem yapmak uygulama izin verir. Örneğin, *MultipartFormDataStreamProvider* uygulama eklenmeye HTML form verileri bölümlerini okur ve bir *NameValueCollection* alın çağrıyı yapandan kolay şekilde.
- **Bağlantı oluşturma geliştirmeleri:** *UrlHelper* artık bağımlı *HttpControllerContext*. Artık erişebilirsiniz *UrlHelper* herhangi bağlamından burada *HttpRequestMessage* kullanılabilir.
- **İleti işleyici yürütme sırası değişikliği:** ileti işleyicileri artık bunlar yerine ters sırada yapılandırılan sırayla yürütülür.
- **İleti işleyicileri yukarı bağlantı için Yardımcısı:** yeni *HttpClientFactory* , wire yukarı *DelegatingHandlers* oluşturup bir *HttpClient* ile İstenen işlem hattını kullanıma hazır. Ayrıca alternatif iç işleyicileri olan yukarı bağlantı için işlevsellik sağlar (varsayılan değer *HttpClientHandler*) yanı sıra kullanırken kablolama yapmak *HttpMessageInvoker* veya başka bir  *DelegatingHandler* yerine *HttpClient* üst başlatıcı olarak.
- **ASP.NET Web iyileştirme, CDN desteği:** ASP.NET Web iyileştirme CDN alternatif yolları her biri için belirtin olanak sağlayan bir içerik teslim ağı üzerinde aynı söz konusu kaynağa işaret eden ek bir URL paket için destek artık sağlar. CDN'ler destekleyen Web uygulamalarınızı son tüketicilerinin, betik ve stil paketleri coğrafi olarak yakın bir konumda yararlanmanıza olanak sağlar. CDN kullanılamıyorsa, üretim uygulamaları bir geri dönüş uygulamalıdır. Geri dönüş test edin.
- **ASP.NET Web API yolları ve yapılandırma taşınabilir *WebApiConfig.Register* test kodundaki resused olabilir statik yöntem.** ASP.NET Web API yolları daha önce de eklendi *RouteConfig.RegisterRoutes* birlikte standart MVC yönlendirir. Varsayılan ASP.NET Web API yolları ve yapılandırma artık ayrı bir işlenen *WebApiConfig.Register* test edilmesini kolaylaştırmak için yöntemi.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

- **Mobil görünümler döndürülmesi ASP.NET MVC 4 RC ve RTM sürümü yanlış önbelleğe alınmış Masaüstü görünüm döndürdü.**

    - Bkz: [ASP.NET MVC 4 mobil önbelleğe alma hatası düzeltildi](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) düzeltme hakkında ayrıntılı bilgi için. Düzeltme yüklenebilir [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet paketi.
- **Yeni değişiklikler Razor görünüm altyapısını**. Aşağıdaki türleri sıradan kaldırıldığını *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Aşağıdaki yöntemleri de kaldırıldı: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Bir ASP.NET MVC 4 uygulamaları/bin dizininde WebMatrix.WebData.dll eklendiğinde, form kimlik doğrulaması URL'sini kazanır.** (Örneğin, "ASP.NET Web sayfaları ile Razor sözdizimi" dağıtılabilir bağımlılıkları ekleme iletişim kullanırken seçerek) WebMatrix.WebData.dll derlemeyi uygulamanıza ekleme/hesabı/oturum açma kimlik doğrulaması oturum açma yeniden yönlendirme kılar yerine / hesabı/varsayılan olarak ASP.NET MVC hesap denetleyicisi beklendiği gibi oturum açın. Bu davranışı engellemek ve web.config kimlik doğrulaması bölümünde belirtilen URL zaten kullanmak üzere PreserveLoginUrl adlı bir uygulama ayarı ekleme ve true olarak ayarlayın: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **NuGet Paket Yöneticisi, Visual Studio 2010 ve Visual Web Developer 2010 'un yan yana yüklemeleri için ASP.NET MVC 4 yüklenmeye çalışılırken yükleme başarısız olur.** Visual Studio 2010 ve Visual Web Developer 2010 ASP.NET MVC 4 ile yan yana çalıştırmak için her iki Visual Studio sürümü zaten yükledikten sonra ASP.NET MVC 4 yüklemeniz gerekir.
- **Önkoşullar zaten kaldırılmış ASP.NET MVC 4 kaldırma başarısız olur.** ASP.NET MVC düzgün bir şekilde kaldırmak için 4you Visual Studio kaldırmadan önce ASP.NET MVC 4 kaldırmalısınız.
- **ASP.NET MVC 4'ü yüklemeden, ASP.NET MVC 3 RTM uygulamaları keser.** RTM ile oluşturulmuş bir ASP.NET MVC 3 uygulamaları yayınlayın (değil [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/download/details.aspx?id=1491) sürüm) yan yana ASP.NET MVC 4 ile çalışmak için aşağıdaki değişiklikleri gerektirir. Proje derleme hataları bu güncelleştirmeleri sonuçları yapmadan oluşturma. 

    **Gerekli güncelleştirmeleri**

  1. Kök Web.config dosyasında yeni bir ekleme *&lt;appSettings&gt;* anahtarla giriş *webPages:Version* ve değer *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından projeyi seçin. Sonra adı tekrar sağ tıklayıp Düzen *ProjectName*.csproj.
  3. Aşağıdaki derleme başvurularının bulun: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Bunları aşağıdakiyle değiştirin:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Değişiklikleri kaydedin, düzenleme ve projeye sağ tıklayın ve yeniden seçin proje (.csproj) dosyasını kapatın.

- **ASP.NET MVC 4 projelerini 4.0 hedefine 4.5'ten değiştirme EntityFramework bütünleştirilmiş kod başvurusu güncelleştirilmez:** , ASP.NET MVC 4 projelerini hedef 4.0 için hala EntityFramework derlemesine başvuru üzerine hedefleyen 4.5 sonra değiştirirseniz 4.5 sürümü. Bu sorunu kaldırma düzeltin ve yeniden EntityFramework NuGet paketi için.
- **Bir ASP.NET MVC 4 uygulama 4.0 hedefine 4.5'ten değiştirdikten sonra Azure'da çalıştırırken 403 Yasak:** ASP.NET MVC 4 projelerini 4.5 hedefleyen sonra hedef 4.0 değiştirin ve ardından Azure'da dağıtma çalışma zamanında bir 403 Yasak hatası görebilirsiniz. Bu sorunu geçici olarak, Web.config dosyasında aşağıdakileri ekleyin: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 kilitleniyor yazarken bir '\' bir dize sabit değerinde bir Razor dosyasında.** Çalışmak için bu sorunu geçici olarak ilk dize sabit değerinin kapanış tırnak işareti girin.
- <strong>Gözatmaya &quot;hesabı/Yönet&quot; CHS ve Dim CHT diller için çalışma zamanı hatası Internet şablon sonucu.</strong> Sorunu düzeltmek için ayırmak için sayfayı Değiştir <em>@User.Identity.Name</em> içinde tek içeriği olarak koyarak tarafından <em>&lt;güçlü&gt;</em> etiketi.
- **Google ve LinkedIn sağlayıcıları, Azure Web siteleri içinde desteklenmez.** Alternatif kimlik doğrulama sağlayıcıları, Azure Web sitelerine dağıtırken kullanırsınız.
- **Uzantı kullanmaya çalıştığınızda UriPathExtensionMapping Express/IIS IIS 8 ile kullanırken, 404 bulunamadı hataları alırsınız.** Web API'leri kullanan istekleri ile statik bir dosya işleyicisi müdahale *UriPathExtensionMappings*. Ayarlama *runAllManagedModulesForAllRequests = true* sorunu çözmek için web.config dosyasındaki.
- **Artık, Controller.Execute yöntemi çağrılır.** Tüm MVC denetleyicileri artık her zaman zaman uyumsuz olarak yürütülür.
