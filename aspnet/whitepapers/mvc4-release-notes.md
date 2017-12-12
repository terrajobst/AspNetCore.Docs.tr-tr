---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "Bu belgede, ASP.NET MVC 4 sürümü açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: fb9d2eaa83fe7486279815c21aec204bdfdf122d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Bu belgede, ASP.NET MVC 4 sürümü açıklanmaktadır.


- [Yükleme notları](#_Toc303253802)
- [Belgeler](#_Toc303253803)
- [Destek](#_Toc303253804)
- [Yazılım gereksinimleri](#_Toc303253805)
- [ASP.NET MVC 4'teki yeni özellikler](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Varsayılan proje şablonları geliştirmeler](#_Toc303253808)
    - [Mobil proje şablonu](#_Toc303253809)
    - [Görüntü modları](#_Toc303253810)
    - [jQuery Mobile, görünüm değiştirici ve tarayıcı geçersiz kılma](#_Toc303253811)
    - [Zaman uyumsuz denetleyicileri görev desteği](#_Toc303253813)
    - [Azure SDK'sı](#_Toc303253814)
    - [Veritabanı geçişleri](#_Toc303253818)
    - [Boş proje şablonu](#_Toc303253819)
    - [Denetleyici için herhangi bir proje klasör ekleme](#_Toc303253820)
    - [Paketleme ve küçültme](#_Toc303253821)
    - [Facebook ve OAuth ve Openıd kullanarak diğer sitelere oturum açmayı etkinleştirme](#_Toc303253822)
- [ASP.NET MVC 4 için bir ASP.NET MVC 3 projesinin yükseltme](#_Toc303253806)
- [ASP.NET MVC 4 Sürüm Adayı değişiklikler](#_Toc303253817)
- [Bilinen sorunlar ve yeni değişiklikler](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Yükleme notları

Visual Studio 2010 için ASP.NET MVC 4 yüklenebilir [ASP.NET MVC 4 giriş sayfası](../mvc/mvc4.md) Web Platformu Yükleyicisi'ni kullanarak.

ASP.NET MVC 4'ü yüklemeden önce ASP.NET MVC 4 önceden yüklenmiş tüm önizlemesini kaldırma öneririz. Kaldırmadan Sürüm Adayı ve ASP.NET MVC 4 Beta ASP.NET MVC 4'e yükseltebilirsiniz.

Bu sürüm, .NET Framework 4.5 herhangi Önizleme sürümleri ile uyumlu değil. ASP.NET MVC 4'ü yüklemeden önce en son sürüme ayrı olarak .NET Framework 4.5 herhangi yüklü Önizleme sürümleri yükseltmeniz gerekir.

ASP.NET MVC 4 yüklü ve çalışma yan yana ASP.NET MVC 3 olabilir.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Belgeler

ASP.NET MVC için belgeleri MSDN Web sitesinde şu URL'de bulunmaktadır:

[https://go.microsoft.com/fwlink/?LinkId=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Öğreticiler ve diğer bilgileri ASP.NET MVC hakkında ASP.NET Web sitesi MVC 4 sayfasında kullanılabilir ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Destek

ASP.NET MVC 4 tam olarak desteklenir. Bu sürüm ile birlikte çalışma hakkında sorularınız varsa, ayrıca bunları ASP.NET MVC forumuna nakledebilirsiniz ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), burada ASP.NET topluluk üyeleri resmi olmayan destek sık sağlayabilir.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

Visual Studio için ASP.NET MVC 4 bileşenleri PowerShell 2.0 ve Visual Studio 2010 Service Pack 1 veya Visual Web Developer Express 2010 Service Pack 1 gerektirir.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>ASP.NET MVC 4'teki yeni özellikler

Bu bölümde tanıtılmıştır özellikleri açıklanmıştır ASP.NET MVC 4 sürümde.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 ASP.NET Web API, tarayıcılar ve mobil cihazlar dahil olmak üzere istemcileri geniş bir yelpazedeki ulaşabilir HTTP Hizmetleri oluşturmak için yeni bir çerçeve içerir. ASP.NET Web API de RESTful hizmetlerini geliştirmek için ideal bir platformdur.

ASP.NET Web API aşağıdaki özellikleri destekler:

- **Modern HTTP programlama modeli:** doğrudan erişmek ve HTTP istekleri ve yeni, kesin türü belirtilmiş HTTP nesne modelini kullanarak, Web API'leri yanıtları değiştirmek. Aynı programlama modeli ve HTTP ardışık düzen kullanılabilir simetrik yeni aracılığıyla istemcide *HttpClient* türü.
- **Tam yollar için destek:** ASP.NET Web API, ASP.NET, rota parametrelerinin ve sınırlamalar da dahil olmak üzere yönlendirme, rota özelliklerini tam kümesini destekler. Ayrıca, Eylemler HTTP yöntemleri eşlemek için basit kuralları kullanın.
- **İçerik anlaşması:** istemci ve sunucu birlikte bir web API döndürülen veriler için doğru biçimde belirlemek için çalışabilir. ASP.NET Web API XML, JSON, varsayılan desteği sağlar ve Form URL'SİNİN kodlanmış biçimleri ve bu destek kendi biçimlendiricileri ekleyerek genişletebilir, ve hatta varsayılan içerik anlaşması stratejisi değiştirin.
- **Model bağlama ve doğrulama:** Model bağlayıcıları çeşitli kısımlarını bir HTTP isteği verileri ayıklar ve bu ileti bölümleri Web API eylemleri tarafından kullanılan .NET nesnelerini dönüştürmek için kolay bir yol sağlar. Doğrulama da üzerinde veri ek açıklamaları temel eylem parametrelerini gerçekleştirilir.
- **Filtreler:** ASP.NET Web API destekleyen iyi bilinen filtreler gibi filtreler *[Authorize]* özniteliği. Yazar ve Eylemler, yetkilendirme ve özel durum işleme için kendi filtreleri takın.
- **Sorgu oluşturma:** kullanım *[Queryable]* döndüren bir eylem filtresi özniteliği *Iqueryable* web API OData sorgu kuralları aracılığıyla sorgulama desteğini etkinleştirmek için.
- **Geliştirilmiş Test Edilebilirlik:** statik bağlam nesneleri ayar HTTP ayrıntıları yerine web API Eylemler iş örnekleri ile *HttpRequestMessage* ve *httpresponsemessage öğesini*. Hızlı bir şekilde Web API işlevleri için birim testleri yazma başlamak için Web API projesi birlikte birim testi projesi oluşturun.
- **Kod tabanlı yapılandırma:** , config bırakarak dosyaları temizleme, ASP.NET Web API yapılandırmasını yalnızca kod üzerinden gerçekleştirilir. Sağlanan hizmet bulucu deseni genişletilebilirlik noktaları yapılandırmak için kullanın.
- **Geliştirilmiş denetimi tersine çevirme (IOC) kapsayıcı desteği:** ASP.NET Web API geliştirilmiş bağımlılık çözümleyici soyutlama aracılığıyla IOC kapsayıcıları için harika desteği sağlar
- **Kendini barındırma:** Web API'leri barındırılması IIS yanı sıra kendi işleminde hala gücünden yolların ve diğer Web API özelliklerini kullanırken.
- **Özel Yardım oluşturmak ve test sayfaları:** şimdi kolayca özel Yardım yapı ve test sayfaları web API'leri yeni kullanarak *IApiExplorer* web API'leri tam çalışma zamanı açıklaması almak için hizmet.
- **İzleme ve Tanılama:** ASP.NET Web API artık System.Diagnostics, ETW ve üçüncü taraf günlük altyapıları gibi mevcut günlük çözümleriyle tümleştirmek kolaylaştırır hafif izleme altyapı sağlar. Sağlayarak izlemeyi etkinleştirebilirsiniz bir *ITraceWriter* uygulaması ve web API yapılandırmanızı ekleme.
- **Bağlantı oluşturma:** ASP.NET Web API kullanan *UrlHelper* aynı uygulamada ilgili kaynaklara bağlantılar oluşturmak için.
- **Web API projesi şablonunu:** yeni Web API projesi formu hızlı bir şekilde hazır ve çalışır ASP.NET Web API ile almak için yeni MVC 4 Proje Sihirbazı'nı seçin.
- **Yapı iskelesi:** kullanım **denetleyici Ekle** hızlı bir şekilde bir Entity Framework'ü temel bir web API denetleyicisi iskele iletişim tabanlı model türü.

ASP.NET Web API hakkında daha fazla ayrıntı için lütfen adresi ziyaret edin [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Varsayılan proje şablonları geliştirmeler

Yeni ASP.NET MVC 4 proje oluşturmak için kullanılan şablon daha modern görünümlü bir Web sitesi oluşturmak için güncelleştirilmiştir:

![](mvc4-release-notes/_static/image1.png)

Yüzeysel geliştirmeleri yanı sıra var. Yeni şablon işlevleri geliştirilmiştir. Şablon hem masaüstü tarayıcıları, hem de mobil tarayıcılar hiçbir özelleştirme gerektirmeden iyi aramak için Uyarlamalı işleme adında bir teknik kullanır.

![](mvc4-release-notes/_static/image2.png)

Eylem Uyarlamalı işlemede görmek için mobil öykünücü kullanın veya yalnızca masaüstü tarayıcı penceresini daha küçük olacak şekilde yeniden boyutlandırmayı deneyin. Tarayıcı penceresini küçük aldığında, sayfanın düzenini değiştirir.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobil proje şablonu

Yeni bir proje başlatıyorsanız ve tablet tarayıcılar ve mobil için özel olarak bir site oluşturmak isterseniz, yeni mobil uygulama proje şablonu kullanabilirsiniz. Bu jQuery Mobile, dokunma özelliği iyileştirilmiş bir kullanıcı Arabirimi oluşturmak için bir açık kaynak kitaplığı temel alır:

![](mvc4-release-notes/_static/image3.png)

Bu şablon Internet uygulama şablonu aynı uygulama yapısını içerir (ve denetleyici kodlarının neredeyse aynıdır), ancak iyi görünür ve iyi dokunma tabanlı mobil cihazlarda hareket etmesi için jQuery Mobile'ı kullanmaya stili. Yapı ve mobil UI stil hakkında daha fazla bilgi için bkz: [jQuery mobil proje Web sitesi](http://jquerymobile.com/).

Mobil iyileştirilmiş görünümlerine eklemek istediğiniz veya tek bir site oluşturmak istiyorsanız, masaüstü ve mobil tarayıcılar farklı stilde görünümlerine hizmet veren bir masaüstü odaklı siteniz varsa, yeni görüntü modları özelliğini kullanabilirsiniz. (Sonraki bölüme bakın.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Görüntü modları

Yeni görüntü modları özelliği isteği yapan tarayıcının bağlı olarak görünümleri seçin bir uygulama sağlar. Örneğin, bir masaüstü tarayıcısı giriş sayfası isterse, uygulama Views\Home\Index.cshtml şablonu kullanabilir. Bir mobil tarayıcı giriş sayfası isterse, uygulama Views\Home\Index.mobile.cshtml şablonu döndürebilir.

Ayrıca düzenleri ve kısmi belirli tarayıcı türleri için geçersiz kılınabilir. Örneğin:

- Görünümler/paylaşılan klasörünü hem içeriyorsa \_Layout.cshtml ve \_Layout.mobile.cshtml şablonları, uygulama kullanacağı varsayılan \_mobil tarayıcılar ve gelenisteklerisırasındaLayout.mobile.cshtml\_Diğer istekleri sırasında Layout.cshtml.
- Her ikisi de bir klasör içeriyorsa, \_MyPartial.cshtml ve \_MyPartial.mobile.cshtml, yönerge @Html.Partial("\_MyPartial") kılacak \_MyPartial.mobile.cshtml mobil gelen istekleri sırasında Tarayıcılar ve \_diğer istekleri sırasında MyPartial.cshtml.

Daha özel görünümler, düzenler veya diğer aygıtlar için kısmi görünümleri oluşturmak istiyorsanız, yeni bir kaydedebilirsiniz *DefaultDisplayMode* örneği, bir istek belirli koşullar sağladığında aramak için ad belirtin. Örneğin, aşağıdaki kodu ekleyebilirsiniz *uygulama\_Başlat* "iPhone" dizesi Apple iPhone tarayıcı bir istekte bulunduğunda, geçerli bir görüntü modu kaydetmek için Global.asax dosyasındaki yöntemi:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Bir Apple iPhone tarayıcı bir istekte bulunduğunda bu kodu çalıştıktan sonra uygulamanız görünümler/paylaşılan kullanacak\\(varsa) _Layout.iPhone.cshtml düzeni. Görüntü modu hakkında daha fazla bilgi için bkz: [ASP.NET MVC 4 mobil özellikleri](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). DisplayModeProvider kullanan uygulamalar yüklemelisiniz [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet paketi. [ASP.NET sonbaharda 2012 güncelleştirmesi](https://go.microsoft.com/fwlink/?LinkID=271322) içeren [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) yeni proje şablonları NuGet paketi. Bkz: [ASP.NET MVC 4 mobil önbelleğe alma hata Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) düzeltme hakkında ayrıntılı bilgi için.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile ve mobil özellikleri

ASP.NET MVC 4 ile mobil uygulamaları oluşturma hakkında bilgi için jQuery Mobile, bkz: kullanma Öğreticisi [ASP.NET MVC 4 mobil özellikleri](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Zaman uyumsuz denetleyicileri görev desteği

Bir nesne türü döndüren olarak tek yöntemleri zaman uyumsuz eylem yöntemleri şimdi yazabilirsiniz *görev* veya *görev&lt;ActionResult&gt;*.

 Daha fazla bilgi için bkz: [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK'sı

ASP.NET MVC 4, Windows Azure SDK 1.6 ve daha yeni sürümleri destekler.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Veritabanı geçişleri

ASP.NET MVC 4 projeleri şimdi Entity Framework 5 içerir. Entity Framework 5'te harika özellikler veritabanı geçişler için destek biridir. Bu özellik, kolayca veritabanındaki verileri korurken bir kod odaklı geçiş kullanarak, veritabanı şemasını gelişmesi sağlar. Veritabanı geçiş hakkında daha fazla bilgi için bkz: [film modeli ve tablo için yeni bir alan ekleyerek](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) içinde [ASP.NET MVC 4 öğretici giriş](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Boş proje şablonu

Tamamen temiz bir sayfadan başlayabilmeniz için MVC boş proje şablonu şimdi gerçekten boştur. Boş proje şablonu önceki sürümü için temel olarak değiştirildi.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Denetleyici için herhangi bir proje klasör ekleme

Artık sağ tıklayın seçin ve **denetleyici Ekle** MVC projenizdeki herhangi bir klasörden. Bu, MVC ve Web API denetleyicilerinin ayrı klasörlerde kalması dahil olmak üzere tüm istiyor ancak denetleyicilerinizi düzenlemek için daha fazla esneklik sağlar.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Paketleme ve küçültme

Paketleme ve küçültme framework komut dosyaları ve CSS ile birlikte gelen, tek bir dosyaya tek tek dosyaların birleştirerek duruma getirmek için bir Web sayfası gerekli HTTP isteklerinin sayısını azaltmak etkinleştirir. Paket içeriğini küçültülmesine tarafından bu istekleri toplam boyutunu sonra azaltabilir. Küçültme bile üzerinde kendi semantiği dayalı CSS Seçici daraltma için değişken adları kısaltmak için boşluk ortadan gibi etkinlikler dahil edebilirsiniz. Paketleri bildirilir ve yapılandırılan kod ve uygulamaları kolayca başvurulan görünümlerinde ya da oluşturabilir yardımcı yöntemler aracılığıyla paket için tek bir bağlantı veya hata ayıklarken, tek tek Paket içeriği için birden çok bağlar. Daha fazla bilgi için bkz: [paketleme ve küçültme](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Facebook ve OAuth ve Openıd kullanarak diğer sitelere oturum açmayı etkinleştirme

ASP.NET MVC 4 Internet proje şablonu varsayılan şablonların şimdi DotNetOpenAuth kitaplığının kullanarak OAuth ve Openıd oturum açma için destek içerir. Bir OAuth veya Openıd sağlayıcısı yapılandırma hakkında daha fazla bilgi için bkz: [WebForms, MVC ve Web sayfaları için OAuth/Openıd Destek](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) ve [OAuth ve Openıd özellik belgelerine ASP.NET Web Pages'de](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET MVC 4 için bir ASP.NET MVC 3 projesinin yükseltme

ASP.NET MVC 4, ASP.NET MVC 4 için ASP.NET MVC 3 uygulama yükseltme zamanı seçerken esneklik aynı bilgisayarda ASP.NET MVC 3 yan yana yüklenebilir.

Yeni ASP.NET MVC 4 proje ve kopya oluşturmak için tüm görünümleri, denetleyicileri, kod ve içerik dosyaları var olan MVC 3 projeden yeni projeye ve ardından derlemeyi güncelleştirmek için herhangi bir harici MVC şablonu eşleşecek şekilde yeni proje başvurur yükseltmek için en basit yolu olan kullanmakta olduğunuz klenen assembiles. MVC 3 projesinin Web.config dosyasında değişiklik yaptıysanız, MVC 4 proje Web.config dosyasına de bu değişiklikleri birleştirmeniz gerekir.

El ile sürüm 4 varolan bir ASP.NET MVC 3 uygulamaya yükseltmek için aşağıdakileri yapın:

1. Tüm Web.config dosyasında (bir olduğundan proje, görünümler klasöründe, diğeri her alanı projenizdeki görünümleri klasöründeki kökündeki) projedeki dosyaları Değiştir aşağıdaki metni, her örneği (Not: System.Web.WebPages, sürüm = 1.0.0.0 bulunamadı Visual Studio 2012 ile oluşturulan projeleri): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    Aşağıdaki karşılık gelen metinle:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Kök Web.config dosyasında güncelleştirme *webPages:Version* "2.0.0.0" öğesine ve yeni bir ekleme *PreserveLoginUrl* "true" değerini sahip anahtarı: 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. Çözüm Gezgini'nde başvuruların üzerinde sağ tıklayın ve NuGet paketlerini Yönet'i seçin. Sol bölmede seçin **Online\NuGet resmi bir paket kaynak**, ardından aşağıdaki güncelleştirin:

    - ASP.NET MVC 4
    - (İsteğe bağlı) jQuery, jQuery doğrulama ve jQuery UI
    - (İsteğe bağlı) Varlık çerçevesi
    - (Optonal) Modernizr
4. Çözüm Gezgini'nde proje adına sağ tıklayın ve sonra projeyi seçin. Daha sonra yeniden adına sağ tıklayın ve Düzenle'yi seçin *ProjectName*.csproj.
5. Bulun *ProjectTypeGuids* öğesi ve {E3E379DF-F4C6-4180-9B81-6769533ABE47} {E53F8FEA-EAE0-44A6-8774-FFD645390401} değiştirin.
6. Değişiklikleri kaydetmek, düzenlemekte olduğunuz projesi (.csproj) dosyasını kapatın, projeye sağ tıklayın ve sonra projeyi yeniden yükle seçin.
7. Proje ASP.NET MVC'ın önceki sürümlerini kullanan derlenmiş tüm üçüncü taraf kitaplıklar başvuruyorsa, kök Web.config dosyasını açın ve aşağıdaki üç ekleyin *bindingRedirect* altında öğelerin  *Yapılandırma* bölümü: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>ASP.NET MVC 4 Sürüm Adayı değişiklikler

ASP.NET MVC 4 Sürüm Adayı sürüm notları şurada bulunabilir:

ASP.NET MVC 4 Sürüm Adayı önemli değişikliklerden bu sürümde, aşağıda özetlenmiştir:

- **Denetleyici yapılandırması başına:** ASP.NET Web API denetleyicilerinin uygulayan için özel bir öznitelik öznitelikli *IControllerConfiguration* kendi biçimlendiricileri, eylem Seçicisi ve parametre bağlayıcıları Kurulumu . *HttpControllerConfigurationAttribute* kaldırılmıştır.
- **Rota ileti işleyicileri başına:** belirli bir rota için istek zincirindeki son ileti işleyicisi artık belirtebilirsiniz. Bu, kendi için gönderilmesi için yönlendirmeyi kullanmak kılma boyunca çerçeveler için destek sağlar (olmayan*IHttpController*) uç noktaları.
- **İlerleme bildirimleri:** *ProgressMessageHandler* yüklenen istek varlıkları ve indirilen yanıt varlıkları için ilerleme bildirimi oluşturur. Bu işleyici kullanarak ne kadar bir istek gövdesi karşıya yükleme veya indirme yanıt gövdesi izlemek mümkündür.
- **İçerik itme:** *PushStreamContent* sınıfı burada bir veri üreticisinin istediği doğrudan istek veya yanıtın (zaman uyumlu veya zaman uyumsuz olarak) kullanarak bir akış yazmak için senaryoları etkinleştirir. Zaman *PushStreamContent* bu çağrılar bir eylem temsilcisi çıkış akışı ile verileri kabul etmeye hazır. Yazarken akış sürece gerekli ve Kapat tamamlandı için geliştirici sonra akışa yazabilirsiniz. *PushStreamContent* akış kapanmasını algılar ve zaman uyumsuz arka plandaki tamamlandığında *görev* içeriği teslim yazma.
- **Hata yanıtları oluşturma:** kullanım *HTTP hatası* tutarlı bir şekilde hala uygularken çalışırken hata bilgileri doğrulama hataları ve özel durumlar gibi göstermek için türü *IncludeErrorDetailPolicy* . Yeni *CreateErrorResponse* kolayca hata yanıtları oluşturmak için genişletme yöntemleri *HTTP hatası* içerik olarak. *HTTP hatası* içeriktir tam içerik anlaşması.
- **Kaldırılan MediaRangeMapping:** medya türü aralıkları şimdi varsayılan içerik uzlaştırıcı tarafından işlenir.
- **Basit tür parametreleri için varsayılan parametre bağlaması olan şimdi [FromUri]:** önceki sürümlerinde ASP.NET Web API basit tür parametreleri kullanılan model bağlama için varsayılan parametre bağlaması. Basit tür parametreleri için varsayılan parametre bağlaması sunulmuştur *[FromUri]*.
- **Eylem Seçimi gerekli parametreleri geliştirir:** eylem ASP.NET Web API artık yalnızca seçer bir eylem URI'den gelen tüm gerekli parametreleri sağladıysanız. Bir parametre isteğe bağlı eylem yöntemi imzası bağımsız değişkeni için varsayılan bir değer sağlayarak belirtilebilir.
- **HTTP parametre bağlamaları özelleştirme:** kullanmak *ParameterBindingAttribute* kullanın ya da belirli bir eylemi parametre için parametre bağlaması özelleştirin için *ParameterBindingRules* üzerinde *HttpConfiguration* parametre bağlamaları özelleştirmek için daha geniş.
- **Mediatypeformatter öğesi geliştirmeleri:** Biçimlendiricileri artık erişiminiz tam *HttpContent* örneği.
- **Ana bilgisayar ilkesi seçimi arabelleğe alma:** uygulama ve yapılandırma *IHostBufferPolicySelector* zaman arabelleğe alma kullanılacaksa İlkesi belirlemek ana bilgisayarları etkinleştirmek için ASP.NET Web API hizmeti.
- **İstemci sertifikalarını bir ana bilgisayar belirsiz şekilde erişim:** kullanım *GetClientCertificate* istek iletisi sağlanan istemci sertifikasını almak için genişletme yöntemi.
- **İçerik anlaşması genişletilebilirlik:** türetme tarafından içerik anlaşması özelleştirme *DefaultContentNegotiator* ve herhangi bir değişiklik istediğiniz içerik anlaşması, geçersiz kılma.
- **406 Kabul edilemez yanıtları döndürmek için destek:** uygun bir biçimlendirici oluşturarak bulunmadığında, ASP.NET Web API'de 406 Kabul edilemez yanıtları şimdi döndürebilir bir *DefaultContentNegotiator* ile*excludeMatchOnTypeOnly* parametre kümesine *doğru*.
- **Form verileri NameValueCollection veya JToken olarak okuma:** URI sorgu dizesi veya istek gövdesi olarak form verileri okuyabilir bir *NameValueCollection* kullanarak *ParseQueryString* ve  *ReadAsFormDataAsync* genişletme yöntemleri sırasıyla. Benzer şekilde, URI sorgu dizesi veya istek gövdesi olarak form verileri okuyabilir bir *JToken* kullanarak *TryReadQueryAsJson* ve *ReadAsAsync*&lt;T&gt; genişletme yöntemleri sırasıyla.
- **Çok bölümlü geliştirmeleri:** yazmak artık mümkündür bir *MultipartStreamProvider* , tamamen uyarlanmış onu okuyun ve böylelikle kullanıcı için en iyi şekilde sonucu sunar MIME çok bölümlü veri türü için. Üzerinde bir işlem sonrası adımı kanca de *MultipartStreamProvider* ne olursa olsun post istediği MIME çok bölümlü gövde bölümlerinin üzerinde işlem yapmak uygulama izin verir. Örneğin, *MultipartFormDataStreamProvider* uygulama HTML formu veri bölümleri okur ve bunlara ekler bir *NameValueCollection* alın çağrıyı yapandan kolay şekilde.
- **Bağlantı nesil geliştirmeleri:** *UrlHelper* artık bağımlı *HttpControllerContext*. Artık erişebilirsiniz *UrlHelper* herhangi bağlamından nerede *HttpRequestMessage* kullanılabilir.
- **İleti işleyici yürütme sırası değiştirme:** ileti işleyicileri şimdi bunların yerine ters sırada yapılandırıldığından sırada yürütülür.
- **İleti işleyicileri yukarı kablolama Yardımcısı:** yeni *HttpClientFactory* , wire yukarı *DelegatingHandlers* ve oluşturma bir *HttpClient* ile İstenen işlem hattı başlamaya hazır olursunuz. Alternatif iç işleyicileri ile yukarı kablolama işlevselliği de sağlar (varsayılan *HttpClientHandler*) yanı sıra kullanırken kablolama yapmak *HttpMessageInvoker* veya başka bir  *DelegatingHandler* yerine *HttpClient* olarak üst çağırıcı.
- **ASP.NET Web iyileştirme CDN'ler destek:** ASP.NET Web iyileştirme şimdi desteği sağlar. her biri için belirtmenizi etkinleştirme CDN diğer yollar bir içerik teslim ağı aynı, bu kaynağa işaret eden ek bir URL paket. CDN'ler destekleyen Web uygulamalarınızın son tüketicilere, komut dosyası ve stil paketleri coğrafi olarak yakın almanızı sağlar.
- **ASP.NET Web API yönlendirir ve yapılandırma taşınabilir *WebApiConfig.Register* resused test kodu olabilir statik yöntemi.** ASP.NET Web API yolları daha önce de eklenmiştir *RouteConfig.RegisterRoutes* birlikte standart MVC yönlendirir. Varsayılan ASP.NET Web API yönlendirir ve yapılandırma ayrı bir şimdi işlenen *WebApiConfig.Register* sınama kolaylaştırmak için yöntem.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

- **Mobil görünümlerini döndürülmesi ASP.NET MVC 4 RC ve RTM sürümü yanlış önbelleğe alınmış Masaüstü görünümleri döndürdü.**

    - Bkz: [ASP.NET MVC 4 mobil önbelleğe alma hata sabit](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) düzeltme hakkında ayrıntılı bilgi için. Düzeltme yüklenebilir [sabit DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet paketi.
- **Razor görüntüleme altyapısı önemli değişiklikler**. Aşağıdaki türlerden sıradan kaldırıldığını *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Aşağıdaki yöntemlerden ayrıca kaldırıldı: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Bir ASP.NET MVC 4 uygulamaları/bin dizininde WebMatrix.WebData.dll dahil olduğunda, form kimlik doğrulaması için URL kazanır.** (Örneğin, "ASP.NET Web sayfaları ile Razor sözdizimi" dağıtılabilir bağımlılıklar ekleme iletişim kullanırken seçerek) uygulamanıza WebMatrix.WebData.dll derleme ekleme/account/oturum açma kimlik doğrulaması oturum açma yeniden yönlendirme kılar yerine / Hesap/ASP.NET MVC hesap denetleyicisi varsayılan beklendiği gibi oturum açın. Bu davranışı önlemek ve web.config kimlik bölümünde belirtilen URL zaten kullanmak için PreserveLoginUrl adlı appSetting ekleyin ve bunu true olarak ayarlayın: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Visual Studio 2010 ve Visual Web Developer 2010 yan yana yüklemeler için ASP.NET MVC 4 yüklemeye çalışırken yüklemek NuGet Paket Yöneticisi başarısız.** Her iki sürümü Visual Studio'nun zaten yüklendikten sonra Visual Studio 2010 ve Visual Web Developer 2010 ASP.NET MVC 4 yan yana çalıştırmak için ASP.NET MVC 4 yüklemeniz gerekir.
- **Önkoşullar zaten kaldırdınız ASP.NET MVC 4 kaldırma başarısız olur.** ASP.NET MVC düzgün bir şekilde kaldırmak için 4you ASP.NET MVC 4 Visual Studio kaldırmadan önce kaldırmanız gerekir.
- **ASP.NET MVC 4'ü yüklemeden ASP.NET MVC 3 RTM uygulamaları keser.** RTM ile oluşturulmuş bir ASP.NET MVC 3 uygulamaları yayın (değil [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/en-us/download/details.aspx?id=1491) sürüm) yana birimi ASP.NET MVC 4 ile çalışması için aşağıdaki değişiklikleri gerekiyor. Proje derleme hataları bu güncelleştirmeleri sonuçları yapmadan oluşturma. 

    **Gerekli güncelleştirmeleri**

    1. Kök Web.config dosyasında yeni Ekle  *&lt;appSettings&gt;*  anahtar girişle *webPages:Version* ve değeri *1.0.0.0*. 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. Çözüm Gezgini'nde proje adına sağ tıklayın ve sonra projeyi seçin. Daha sonra yeniden adına sağ tıklayın ve Düzenle'yi seçin *ProjectName*.csproj.
    3. Aşağıdaki derleme başvurularını bulun: 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        Bunları aşağıdakiyle değiştirin:

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. Değişiklikleri kaydetmek, düzenlemekte olduğunuz ve projeye sağ tıklayın ve yeniden yükle seçeneğini belirleyin (.csproj) proje dosyasını kapatın.
- **Bir ASP.NET MVC 4 Proje hedef 4.0 4.5'ten değiştirme EntityFramework derleme başvurusu güncelleştirmez:** , bir ASP.NET MVC 4 Proje hedef 4.0 atamak için hala EntityFramework derlemesine başvuru üzerine 4.5 sonra değiştirirseniz 4.5 sürümü. Bu sorunu kaldırma düzeltin ve EntityFramework NuGet paketini yeniden yüklemek için.
- **Bir ASP.NET MVC 4 uygulama hedef 4.0, 4.5'ten değiştirdikten sonra Azure üzerinde çalışırken 403 Yasak:** bir ASP.NET MVC 4 Proje hedef 4.0 4.5 atamak sonra değiştirmek ve Azure'a dağıtma çalışma zamanında 403 Yasak hatası görebilirsiniz. Bu sorun için geçici çözüm web.config aşağıdaki ekleyin:`<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 çöküyor yazarken bir '\' Razor dosyasında değişmez değer dize.** Çalışmak için kapama çift tırnağı dize sabit değeri, sorunu çözmek ilk girin.
- **İçin dizin taramayı &quot;hesap/Yönet&quot; CHS, TRK ve CHT dil için bir çalışma zamanı hatası Internet şablon sonucu.** Sorunu düzeltmek için ayırmak için sayfayı değiştirme  *@User.Identity.Name*  içinde yalnızca içerik olarak koyarak kullanılabilir  *&lt;güçlü&gt;*  etiketi.
- **Google ve LinkedIn sağlayıcıları Azure Web siteleri içinde desteklenmez.** Alternatif kimlik doğrulama sağlayıcıları için Azure Web siteleri dağıtırken kullanın.
- **Uzantı kullanmaya çalıştığınızda UriPathExtensionMapping IIS 8 Express/IIS ile kullanırken, 404 bulunamadı hataları alırsınız.** Statik dosya işleyici Web kullanmak API'leri isteklerle müdahale *UriPathExtensionMappings*. Ayarlama *runAllManagedModulesForAllRequests = true* sorunu çözmek için web.config dosyasında.
- **Controller.Execute yöntemi artık çağrılır.** Tüm MVC denetleyicileri şimdi her zaman zaman uyumsuz olarak çalıştırılır.
