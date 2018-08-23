---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Bu belgede bu sürüm Visual Studio 2010 için ASP.NET MVC 4 Beta açıklanmaktadır.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: f1d949ec716ea8cb677c54fe5b07431161c58fbc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756273"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Bu belgede bu sürüm Visual Studio 2010 için ASP.NET MVC 4 Beta açıklanmaktadır.
> 
> > [!NOTE]
> > Bu, en son sürüm değil. ASP.NET MVC 4 RC sürüm notları kullanılabilir [burada](mvc4-release-notes.md).


- [Yükleme notları](#_Toc303253802)
- [Belgeler](#_Toc303253803)
- [Destek](#_Toc303253804)
- [Yazılım gereksinimleri](#_Toc303253805)
- [Bir ASP.NET MVC 3 projesini ASP.NET MVC 4 için yükseltme](#_Toc303253806)
- [ASP.NET MVC 4 Beta sürümündeki yeni özellikler](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET tek sayfalı uygulama](#_Toc317096198)
    - [Varsayılan proje şablonları geliştirmeler](#_Toc303253808)
    - [Mobil proje şablonu](#_Toc303253809)
    - [Görüntü modları](#_Toc303253810)
    - [jQuery Mobile, görünüm değiştirici ve tarayıcı geçersiz kılma](#_Toc303253811)
    - [Visual Studio'da kod üretimi yönelik tarifleri](#_Toc303253812)
    - [Görev zaman uyumsuz denetleyicileri desteği](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Bilinen sorunlar ve yeni değişiklikler](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Yükleme notları

Visual Studio 2010 için ASP.NET MVC 4 Beta yüklenebilir [ASP.NET MVC 4 giriş sayfası](../mvc/mvc4.md) Web Platformu Yükleyicisi'ni kullanarak.

ASP.NET MVC 4 Beta yüklemeden önce ASP.NET MVC 4 önceden yüklenmiş tüm önizlemeleri kaldırmanız gerekir.

Bu sürümde, .NET Framework 4.5 Developer Preview ile uyumlu değil. ASP.NET MVC 4 Beta yüklemeden önce .NET 4.5 Geliştirici önizlemesi kaldırmanız gerekir.

ASP.NET MVC 4 yüklenebilir ve yan yana çalıştırabilirsiniz ASP.NET MVC 3 ile.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Belgeler

ASP.NET MVC için belgeleri MSDN Web sitesinde şu URL'den ulaşabilirsiniz:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Öğreticiler ve ASP.NET MVC hakkında diğer bilgiler Web sitesinin ASP.NET MVC 4 sayfasında kullanılabilir ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Destek

Bu bir önizleme sürümüdür ve resmi olarak desteklenmez. Bu sürümle birlikte çalışma hakkında sorularınız varsa, bunları ASP.NET MVC foruma ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), burada ASP.NET topluluk üyelerinin resmi olmayan destek sık sağlayabilir.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET MVC 4 bileşenleri Visual Studio için PowerShell 2.0 ve hizmet paketi 1 ile Visual Studio 2010 veya Visual Web Developer Express 2010 Service Pack 1 gerektirir.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Bir ASP.NET MVC 3 projesini ASP.NET MVC 4 için yükseltme

ASP.NET MVC 4, aynı bilgisayarda bir ASP.NET MVC 3 uygulama ASP.NET MVC 4 Yükseltme zamanı seçme esnekliği sağlayan ASP.NET MVC 3 ile yan yana yüklenebilir.

Yükseltmek için en basit yolu, yeni bir ASP.NET MVC 4 projesi ve kopya oluşturmak için tüm görünümleri, denetleyicisi, kod ve içerik dosyaları var olan MVC 3 projeden yeni projeye ve derlemeyi güncelleştirmek için eski proje eşleştirmek için yeni proje başvuruları ' dir. MVC 3 projesini Web.config dosyasında değişiklik yaptıysanız, MVC 4 proje içinde Web.config dosyasına da bu değişiklikleri birleştirmeniz gerekir.

Mevcut bir ASP.NET MVC 3 uygulama sürüm 4 için el ile yükseltmek için aşağıdakileri yapın:

1. (Var. bir proje, görünümleri klasöründe, diğeri görünümleri klasöründe, projenizdeki her bölgenin kökünde) projedeki tüm Web.config dosyalarında aşağıdaki metni her örneğini değiştirin:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    Aşağıdaki karşılık gelen metinle:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Kök Web.config dosyasında güncelleştirme *webPages:Version* "2.0.0.0" öğesine ve yeni bir *PreserveLoginUrl* "true" değerini anahtar:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Solution Explorer'da, başvurusunu silmek *System.Web.Mvc* (işaret ettiği için sürüm 3 DLL). Ardından bir başvuru ekleyin *System.Web.Mvc* (v4.0.0.0). Özellikle, derleme başvurularını güncelleştirmek için aşağıdaki değişiklikleri yapın. Ayrıntılar aşağıda verilmiştir:

    1. Çözüm Gezgini'nde, aşağıdaki derlemelere başvuruları silin: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Bir aşağıdaki derlemelere başvurular ekleyin: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından projeyi seçin. Sonra adı tekrar sağ tıklayıp Düzen *ProjectName*.csproj.
5. Bulun *ProjectTypeGuids* öğesi ve {E3E379DF-F4C6-4180-9B81-6769533ABE47} {E53F8FEA-EAE0-44A6-8774-FFD645390401} değiştirin.
6. Değişiklikleri kaydedin, düzenlemekte olduğunuz proje (.csproj) dosyayı kapatın, projeye sağ tıklayın ve ardından projeyi seçin.
7. ASP.NET MVC'ın önceki sürümleri kullanılarak derlenmiş herhangi bir üçüncü taraf kitaplıklar proje başvuru yapıyorsa, kök Web.config dosyasını açın ve aşağıdaki üç Ekle *bindingRedirect* altındaki öğeleri  *Yapılandırma* bölümü: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>ASP.NET MVC 4 Beta sürümündeki yeni özellikler

Bu bölümde sunulan özellikler açıklanır ASP.NET MVC 4 Beta sürümünde.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4, ASP.NET Web API artık içerir, HTTP Hizmetleri oluşturmaya yarayan yeni bir çerçeve istemciler tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşabilirsiniz. ASP.NET Web API aynı zamanda RESTful hizmetleri oluşturmak için ideal bir platformdur.

ASP.NET Web API desteği için aşağıdaki özellikleri içerir:

- **Modern HTTP programlama modeli:** doğrudan erişmek ve HTTP isteklerini ve yanıtlarını yeni, kesin türü belirtilmiş HTTP nesne modelini kullanarak Web API'leri. Aynı programlama modeli ve HTTP ardışık düzen, simetrik istemci yeni bir HttpClient türü aracılığıyla kullanılabilir.
- **Tam yollar için destek**: Web API'leri, artık her zaman Rota parametrelerinin ve kısıtlamalar dahil olmak üzere Web yığının bir bölümü olan yol özellikleri kümesini destekler. Ayrıca, artık [HttpPost] gibi öznitelikleri uygulamak gereken şekilde eylemleri eşleme kuralları için tam destek, sınıflar ve yöntemler vardır.
- **İçerik anlaşması**: istemci ve sunucu birlikte API'den döndürülen veriler için doğru biçimde belirlemek için çalışabilir. Form URL'SİNİN kodlanmış biçimleri XML ve JSON için varsayılan desteği sunuyoruz ve kendi biçimlendiricileri ekleyerek bu desteğini genişletmek veya varsayılan içerik anlaşması stratejisi bile değiştirin.
- **Model bağlama ve doğrulama:** Model bağlayıcıları bir HTTP isteğinin çeşitli parçalarını verileri ayıklamak ve ileti bölümlerinde Web API eylemlerini tarafından kullanılan .NET nesnelerine dönüştürmek için kolay bir yol sağlar.
- **Filtreler:** Web API'leri artık [Authorize] özniteliği gibi iyi bilinen filtreler dahil olmak üzere, filtreleri destekler. Yazar ve eylem, yetkilendirme ve özel durum işleme için kendi filtrelerinizi takın.
- **Sorgu oluşturma:** yalnızca Iqueryable döndürerek&lt;T&gt;, Web API'niz ile OData URL kurallarına sorgulama destekleyecektir.
- **Gelişmiş HTTP ayrıntıları edilebilirliğini:** statik bağlam nesneleri ayar HTTP ayrıntıları yerine Web API eylemlerini artık HttpRequestMessage ve HttpResponseMessage örnekleri ile çalışabilirsiniz. Bu nesneler genel sürümlerini HTTP türlerine ek olarak, özel türler ile çalışmanıza izin vermek için de mevcuttur.
- **Ters çevirmeyi denetim (IOC) DependencyResolver aracılığıyla geliştirilmiş:** Web API artık MVC'nin bağımlılık çözümleyici tarafından uygulanan hizmet bulucu deseni örnekleri için birçok farklı olanakları elde etmek için kullanır.
- **Kod tabanlı yapılandırma:** bırakarak, yapılandırma dosyalarını temizlemek, Web API configuration, yalnızca kod gerçekleştirilir.
- **Barındırma:** Web API'leri barındırılabilir IIS yanı sıra kendi işleminizde yollar'ın tüm gücünden ve diğer özellikleri Web API'sinin hala kullanırken.

ASP.NET Web API hakkında daha fazla bilgi edinmek için lütfen [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET tek sayfalı uygulama

ASP.NET MVC 4, JavaScript ve Web API'leri kullanarak önemli istemci tarafı etkileşimler ile tek sayfa uygulamaları oluşturmaya yönelik deneyiminin önizlemesini erkenden artık içerir. Bu destek içerir:

- Önbelleğe alınmış verileri yerel daha zengin etkileşim için JavaScript kitaplıkları kümesi
- İş ve DAL destek birimi için ek Web API'si bileşenleri
- Hızlıca kullanmaya başlamak için yapı iskelesi MVC proje şablonuyla

ASP.NET MVC 4'te tek sayfalı uygulama hakkında daha fazla ayrıntı desteği için lütfen [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Varsayılan proje şablonları geliştirmeler

Yeni ASP.NET MVC 4 projeleri oluşturmak için kullanılan şablon daha modern görünümlü bir Web sitesi oluşturmak için güncelleştirilmiştir:

![](mvc4-beta-release-notes/_static/image1.png)

Yüzeysel iyileştirmelerinin yanı sıra yeni şablonu işlevindeki var. geliştirdi. Şablon iyi masaüstü tarayıcıları ve mobil tarayıcılar özelleştirme olmadan aramak için Uyarlamalı işleme olarak adlandırılan tekniği kullanır.

![](mvc4-beta-release-notes/_static/image2.png)

Uyarlamalı işleme iş başında görmek için mobil öykünücü kullanın veya yalnızca daha küçük olacak şekilde Masaüstü tarayıcı penceresini yeniden boyutlandırmayı deneyin. Tarayıcı penceresini yeterince aldığında sayfasının düzenini değiştirir.

Varsayılan proje şablonu için başka bir geliştirme, JavaScript daha zengin bir kullanıcı Arabirimi sağlamak için kullanılır. Şablonda kullanılan oturum açma ve kaydetme bağlantıları, zengin bir oturum açma ekranı göstermek için jQuery kullanıcı Arabirimi iletişim kutusu kullanma örnekleri şunlardır:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobil proje şablonu

Yeni bir proje başlatılıyor ve tablet tarayıcılar ve mobil için özel bir site oluşturmak istiyorsanız, yeni mobil uygulaması proje şablonu kullanabilirsiniz. Bu, jQuery Mobile, dokunma özelliği iyileştirilmiş bir kullanıcı Arabirimi oluşturmak için bir açık kaynak kitaplığı, dayalıdır:

![](mvc4-beta-release-notes/_static/image4.png)

Bu şablon Internet uygulama şablonu aynı uygulama yapısını içerir (ve denetleyici kodlarının neredeyse aynıdır), ancak düzgün görünmesini ve davranmasını dokunmatik tabanlı mobil cihazlarda jQuery Mobile kullanılarak stil uygulanmış. Yapısı ve stil mobil kullanıcı Arabirimi hakkında daha fazla bilgi için bkz. [jQuery Mobile proje Web sitesi](http://jquerymobile.com/).

Mobil için iyileştirilmiş görünümlerine eklemek istediğiniz veya tek bir site oluşturmak istiyorsanız farklı bir stilde görünümleri Masaüstü ve mobil tarayıcılara hizmet veren bir masaüstü odaklı site zaten varsa, yeni görüntü modları özelliğini kullanabilirsiniz. (Sonraki bölüme bakın.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Görüntü modları

Yeni görüntü modları özelliği isteği yapan tarayıcının bağlı olarak görünümleri seçin bir uygulama sağlar. Örneğin, bir masaüstü tarayıcısı giriş sayfasını isterse, uygulama Views\Home\Index.cshtml şablonu kullanabilir. Bir mobil tarayıcıda giriş sayfası isterse, uygulama Views\Home\Index.mobile.cshtml şablon döndürebilir.

Ayrıca düzenleri ve kısmi bölümleri için belirli bir tarayıcı türleri geçersiz kılınabilir. Örneğin:

- Görünümler/paylaşılan klasörünüzde her ikisi de varsa \_Layout.cshtml ve \_Layout.mobile.cshtml şablonları, uygulamayı kullanacağı varsayılan \_Layout.mobile.cshtml mobil tarayıcılar ve gelenisteklerisırasında\_Layout.cshtml diğer istekleri sırasında.
- Her ikisi de bir klasör içeriyorsa \_MyPartial.cshtml ve \_MyPartial.mobile.cshtml, yönerge @Html.Partial("\_MyPartial") işlenir \_MyPartial.mobile.cshtml mobil gelen istekleri sırasında Tarayıcılar ve \_MyPartial.cshtml diğer istekleri sırasında.

Daha özel görünümler, düzenler veya diğer cihazlar için kısmi görünümler oluşturmak istiyorsanız, yeni bir kaydedebilirsiniz *DefaultDisplayMode* , bir istek belirli koşullar sağladığında aramak için ad belirtmek için örneği. Örneğin, aşağıdaki kodu ekleyebilirsiniz *uygulama\_Başlat* dize "iPhone" Apple iPhone tarayıcı bir istekte bulunduğunda, geçerli bir görüntü modu kaydetmek için Global.asax dosyasındaki yöntemi:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Bir Apple iPhone tarayıcı bir istekte bulunduğunda bu kod çalıştırıldıktan sonra görünümler/paylaşılan uygulamanızın kullanacağı\\_Layout.iPhone.cshtml Düzen (varsa).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, görünüm değiştirici ve tarayıcı geçersiz kılma

jQuery Mobile, dokunma özelliği iyileştirilmiş web kullanıcı arabirimini oluşturmaya yönelik bir açık kaynak kitaplığıdır. JQuery Mobile ile bir ASP.NET MVC 4 uygulama kullanmak istiyorsanız, indirin ve başlamanıza yardımcı olacak bir NuGet paketini yükleyin. Visual Studio Paket Yöneticisi konsolunu yüklemek için aşağıdaki komutu yazın:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Bu, jQuery Mobile ve aşağıdakiler dahil olmak üzere bazı yardımcı dosyalar da yükler:

- Görünümler/paylaşılan/\_jQuery Mobile tabanlı bir düzen olan Layout.Mobile.cshtml.
- Görünümler/paylaşılan oluşan bir görünüm değiştiricibileşeni/\_ViewSwitcher.cshtml kısmi Görünüm ve ViewSwitcherController.cs denetleyicisi.

Paketi yükledikten sonra bir mobil tarayıcı kullanarak uygulamanızı çalıştırın (veya eşdeğer Firefox gibi [kullanıcı aracısı değiştirici](http://chrispederick.com/work/user-agent-switcher/) eklenti). JQuery Mobile Düzen hallettiğinden sayfalarınıza oldukça farklı görünmesini görürsünüz ve stil oluşturma. Bu yararlanmak için bunu yapabilirsiniz:

- Altında açıklandığı gibi Mobile özgü görünüm geçersiz kılmaları oluşturma [görüntü modları](#_Toc303253810) daha önce (örneğin, Views\Home\Index.cshtml mobil tarayıcılar için geçersiz kılmak için Views\Home\Index.mobile.cshtml oluşturma).
- Okuma [jQuery Mobile belgeleri](http://jquerymobile.com/) mobil görünümlerde dokunma özelliği iyileştirilmiş kullanıcı Arabirimi öğeleri ekleme hakkında daha fazla bilgi için.

Mobil için iyileştirilmiş web sayfaları için bir kuralı metni bir şey Masaüstü görünüm veya kullanıcıların bir masaüstü sürümüne geçiş olanak sağlayan tam site modu gibi olan bir bağlantı eklemektir. JQuery.Mobile.MVC paketi bu amaç için bir örnek görünüm değiştirici bileşeni içerir. Görünümler/paylaşılan varsayılan olarak kullanılan\\_Layout.Mobile.cshtml görünümü ve sayfa işlendiğinde şöyle görünür:

![](mvc4-beta-release-notes/_static/image5.png)

Ziyaretçiler bağlantıya tıklarsanız, aynı sayfada Masaüstü sürümüne geçiş.

Varsayılan olarak, masaüstü düzeni görünüm değiştirici içermez çünkü ziyaretçiler mobil moduna almanın bir yolu olmaz. Bunu etkinleştirmek için şu başvuruyu ekleyin  *\_ViewSwitcher* Masaüstü düzeninizi yalnızca içinde *&lt;gövdesi&gt;* öğesi:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Görünüm değiştirici tarayıcı geçersiz kılma adlı yeni bir özellik kullanır. Bu özellik bunların çıkıyormuş gibi istekleri kabul uygulamanızı olanak sağlar. olandan farklı bir tarayıcıdan (kullanıcı aracısı) gerçekte gelen oldukları. Tarayıcı geçersiz kılma sağlayan yöntemler aşağıdaki tabloda listelenmiştir.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | İsteğin asıl kullanıcı aracısı değerini belirtilen kullanıcı aracısını kullanarak geçersiz kılar. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Geçersiz kılma belirtilmemişse, isteğin Kullanıcı aracısını geçersiz kılma değerini veya asıl kullanıcı aracısı dizesi döndürür. |
| `HttpContext.GetOverriddenBrowser()` | Döndürür bir *HttpBrowserCapabilitiesBase* istek için ayarlanan kullanıcı aracısı karşılık gelen bir örneği (gerçek ya da geçersiz kılınan). Bu değer gibi özellikleri almak için kullanabileceğiniz *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Geçerli istek için herhangi bir geçersiz kılınan Kullanıcı aracısını kaldırır. |

Tarayıcı geçersiz kılmak, ASP.NET MVC 4 çekirdek özelliğidir ve jQuery.Mobile.MVC paketi yüklemezseniz bile kullanılabilir. Ancak, yalnızca görüntüleme, Düzen ve kısmi görünüm seçimi etkiler; bağlı olduğu diğer ASP.NET özelliğini etkilemez *Request.Browser* nesne.

Varsayılan olarak bir tanımlama bilgisi kullanarak kullanıcı aracısını geçersiz kılma depolanır. Geçersiz kılma başka bir yerde (örneğin, bir veritabanındaki) depolamak istiyorsanız, varsayılan sağlayıcı değiştirebilirsiniz (*BrowserOverrideStores.Current*). Bu sağlayıcı için belgeler, ASP.NET MVC daha yeni bir sürümünü eşlik eden kullanıma sunulacaktır.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Visual Studio'da kod üretimi yönelik tarifleri

NuGet kullanarak yükleyebileceğiniz paketleri dayalı çözüme özel kod oluşturmak Visual Studio yeni tarifleri özelliği sağlar. Tarif framework, geliştiricilerin yerleşik kod oluşturucuları alanı ekleyin, denetleyici Ekle ve Görünüm Ekle değiştirmek için kullanabileceğiniz kod üretimi eklentiler yazma kolaylaştırır. Tarif içeren NuGet paketleri olarak dağıtıldığından, kolayca kaynak denetimine iade ve projedeki tüm geliştiriciler için otomatik olarak paylaşıldığı. Ayrıca çözüm başına olarak kullanılabilir.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Görev zaman uyumsuz denetleyicileri desteği

Zaman uyumsuz eylem yöntemi artık bir nesne türü döndüren olarak tek yöntemler yazabilirsiniz *görev* veya *görev&lt;actionresult öğesini&gt;*.

Örneğin, Visual C# 5 kullanıyorsanız (veya bu adı kullanıyor [zaman uyumsuz CTP](https://msdn.microsoft.com/vstudio/async.aspx)), şuna benzeyen bir zaman uyumsuz eylem yöntemini oluşturabilirsiniz:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

Önceki eylem yöntemine çağrıları *newsService.GetHeadlinesAsync* ve *sportsService.GetScoresAsync* zaman uyumsuz olarak adlandırılır ve bir iş parçacığı havuzu iş parçacığından engellemez.

Döndüren zaman uyumsuz eylem yöntemleri *görev* örnekleri de zaman aşımlarını destekler. Eylem yönteminizi edilebilen türünde bir parametre ekleyin *CancellationToken* işlem metodu imzası. Aşağıdaki örnek, bir zaman aşımı değerini milisaniye 2500 olan ve görüntüleyen bir zaman uyumsuz eylem yöntemini gösterir. bir *zaman aşımına uğradı* bir zaman aşımı oluşması durumunda istemciye görüntüleyin.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta Windows Azure SDK'sı Eylül 2011 1.5 sürümünü destekler.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

- **ASP.NET MVC 4 Beta yükledikten sonra Visual Studio 2010 Service Pack 1 CSHTML VBHTML Düzenleyicisi CSHTML/VBHTML düzenleyicide kod parçacığı veya JavaScript içindeki cshtml veya vbhtml dosyaları yazdıktan sonra uzun bir süre duraklatmak.** Bu, az önce oluşturduğunuz ve henüz derlenmiş ASP.NET MVC 4 uygulamalarda oluşur.

    Geçici çözüm bin klasöründe derlemeler için projenin derlemektir. Unutmayın, bin klasöründen derlemeleri kaldıran projeyi Temizle, düzenleyici sorun geri dönün.

    Bu, sonraki sürümde düzeltilecektir.
- **Visual Studio 11 Beta için C# proje şablonları, bir Global.asax.cs yanlış bağlantı dizesini içerir.** Uygulamada belirtilen varsayılan bağlantı\_projeleri Visual Studio 11 Beta'da oluşturulan atlanmayan bir ters eğik çizgi içeren bir LocalDB bağlantı dizesi içeriyor yöntemi Başlat (\) karakter. Bu, bir SqlException oluşturan bir Entity Framework DbContext erişim girişimleri sırasında bağlantı hatası sonuçlanır.

    Bu sorunu düzeltmek için uygulama ters eğik çizgi karakteri kaçış\_şu şekilde okunması Global.asax.cs yöntemi başlatın:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **.NET 4.5 hedefleyen bir ASP.NET MVC 4 uygulamaları, .NET 4.0 altında çalıştırdığınızda System.Net.Http.dll derleme erişim girişimi sırasında bir FileLoadException durum oluşturur.** .NET 4.5 altında oluşturulan ASP.NET MVC 4 uygulamalarını içeren bir bağlama, bildiren bir FileLoadException neden olabilecek bir yeniden yönlendirme "yükleyemedi dosyası veya bütünleştirilmiş kod 'System.Net.Http' veya bağımlılıklarından biri." Uygulama bir sistemde .NET 4.0 yüklü olduğunda yürütülür. Bu sorunu düzeltmek için aşağıdaki bağlama yeniden yönlendirmesi, web.config dosyasından kaldırın:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Derleme bağlama öğesi değiştirilmiş Web.config dosyasında aşağıdaki gibi görünmelidir:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Visual Basic projelerinde "Denetleyici Ekle" öğe şablonu çağrıldığında hatalı bir ad alanı oluşturur</strong><strong>gelen bir alanı içinde.</strong> Visual Basic kullanan ASP.NET MVC projesinde bir alan için bir denetleyici eklediğinizde, öğe şablonu yanlış ad alanı Denetleyicisi'nde ekler. Denetleyicideki tüm eylem gittiğinizde bir "dosya bulunamadı" hatası sonucudur.  
  
  Oluşturulan ad alanı, her şeyin kök ad sonra atlar. Örneğin, oluşturulan ad alanıdır *RootNamespace* olmalıdır, ancak *RootNamespace.Areas.AreaName.Controllers* .
- **Razor görünüm altyapısını bozucu değişiklikler.** Sıradan bir yeniden yazma, Razor ayrıştırıcısı bir parçası olarak, aşağıdaki türleri kaldırıldığını *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Aşağıdaki yöntemleri de kaldırıldı: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Bir ASP.NET MVC 4 uygulamaları/bin dizininde WebMatrix.WebData.dll eklendiğinde, form kimlik doğrulaması URL'sini kazanır.** (Örneğin, "ASP.NET Web sayfaları ile Razor sözdizimi" dağıtılabilir bağımlılıkları ekleme iletişim kullanırken seçerek) WebMatrix.WebData.dll derlemeyi uygulamanıza ekleme/hesabı/oturum açma kimlik doğrulaması oturum açma yeniden yönlendirme kılar yerine / hesabı/varsayılan olarak ASP.NET MVC hesap denetleyicisi beklendiği gibi oturum açın. Bu davranışı engellemek ve web.config kimlik doğrulaması bölümünde belirtilen URL zaten kullanmak üzere PreserveLoginUrl adlı bir uygulama ayarı ekleme ve true olarak ayarlayın: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **NuGet Paket Yöneticisi, Visual Studio 2010 ve Visual Web Developer 2010 'un yan yana yüklemeleri için ASP.NET MVC 4 yüklenmeye çalışılırken yükleme başarısız olur.** Visual Studio 2010 ve Visual Web Developer 2010 ASP.NET MVC 4 ile yan yana çalıştırmak için her iki Visual Studio sürümü zaten yükledikten sonra ASP.NET MVC 4 yüklemeniz gerekir.
- **Önkoşullar zaten kaldırılmış ASP.NET MVC 4 kaldırma başarısız olur.** ASP.NET MVC düzgün bir şekilde kaldırmak için 4you Visual Studio kaldırmadan önce ASP.NET MVC 4 kaldırmalısınız.
- **Varsayılan Web API projesi çalıştırmaya yanlış yok RegisterApis yöntemi kullanılarak yollar eklemek için kullanıcı doğrudan yönergeleri gösterir.** ASP.NET yönlendirme tablosunu kullanarak RegisterRoutes yöntemi yollar eklenmesi gerekir.
- **ASP.NET MVC 4 Beta yükleme, ASP.NET MVC 3 RTM uygulamaları keser.** Oluşturulmuş uygulamaları ASP.NET MVC 3 RTM'ye (değil ile ASP.NET MVC 3 araçları güncelleştirme sürüm) sürüm gerektiren aşağıdaki değişiklikleri yan yana ASP.NET MVC 4 Beta ile çalışmak için. Proje derleme hataları bu güncelleştirmeleri sonuçları yapmadan oluşturma. 

    **Gerekli güncelleştirmeleri**

  1. Kök Web.config dosyasında yeni bir ekleme *&lt;appSettings&gt;* anahtarla giriş *webPages:Version* ve değer *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından projeyi seçin. Sonra adı tekrar sağ tıklayıp Düzen *ProjectName*.csproj.
  3. Aşağıdaki derleme başvurularının bulun: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Bunları aşağıdakiyle değiştirin:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Değişiklikleri kaydedin, düzenleme ve projeye sağ tıklayın ve yeniden seçin proje (.csproj) dosyasını kapatın.
