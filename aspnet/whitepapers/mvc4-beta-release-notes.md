---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "Bu belgede bu sürüm Visual Studio 2010 için ASP.NET MVC 4 Beta açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 4af2df61ab4507b1f100d6bb75777da1168c5a75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Bu belgede bu sürüm Visual Studio 2010 için ASP.NET MVC 4 Beta açıklanmaktadır.
> 
> > [!NOTE]
> > Bu en son sürüm değil. ASP.NET MVC 4 RC sürüm notları kullanılabilir [burada](mvc4-release-notes.md).


- [Yükleme notları](#_Toc303253802)
- [Belgeler](#_Toc303253803)
- [Destek](#_Toc303253804)
- [Yazılım gereksinimleri](#_Toc303253805)
- [ASP.NET MVC 4 için bir ASP.NET MVC 3 projesinin yükseltme](#_Toc303253806)
- [ASP.NET MVC 4 Beta sürümündeki yeni özellikler](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET tek sayfa uygulaması](#_Toc317096198)
    - [Varsayılan proje şablonları geliştirmeler](#_Toc303253808)
    - [Mobil proje şablonu](#_Toc303253809)
    - [Görüntü modları](#_Toc303253810)
    - [jQuery Mobile, görünüm değiştirici ve tarayıcı geçersiz kılma](#_Toc303253811)
    - [Visual Studio'da kod oluşturma için tarif](#_Toc303253812)
    - [Zaman uyumsuz denetleyicileri görev desteği](#_Toc303253813)
    - [Azure SDK'sı](#_Toc303253814)
    - [Bilinen sorunlar ve yeni değişiklikler](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Yükleme notları

Visual Studio 2010 için ASP.NET MVC 4 Beta yüklenebilir [ASP.NET MVC 4 giriş sayfası](../mvc/mvc4.md) Web Platformu Yükleyicisi'ni kullanarak.

ASP.NET MVC 4 Beta yüklemeden önce ASP.NET MVC 4 önceden yüklenmiş tüm önizlemesini kaldırmanız gerekir.

Bu sürüm, .NET Framework 4.5 Developer Preview ile uyumlu değil. ASP.NET MVC 4 Beta yüklemeden önce .NET 4.5 Developer Preview kaldırmanız gerekir.

ASP.NET MVC 4 yüklenebilir ve yan yana çalıştırabilirsiniz ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Belgeler

ASP.NET MVC için belgeleri MSDN Web sitesinde şu URL'de bulunmaktadır:

[https://go.microsoft.com/fwlink/?LinkId=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Öğreticiler ve diğer bilgileri ASP.NET MVC hakkında ASP.NET Web sitesi MVC 4 sayfasında kullanılabilir ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Destek

Bu bir önizleme sürümüdür ve resmi olarak desteklenmez. Bu sürüm ile birlikte çalışma hakkında sorularınız varsa, bunları ASP.NET MVC forumuna gönderin ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), burada ASP.NET topluluk üyeleri resmi olmayan destek sık sağlayabilir.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

Visual Studio için ASP.NET MVC 4 bileşenleri PowerShell 2.0 ve Visual Studio 2010 Service Pack 1 veya Visual Web Developer Express 2010 Service Pack 1 gerektirir.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET MVC 4 için bir ASP.NET MVC 3 projesinin yükseltme

ASP.NET MVC 4, ASP.NET MVC 4 için ASP.NET MVC 3 uygulama yükseltme zamanı seçerken esneklik aynı bilgisayarda ASP.NET MVC 3 yan yana yüklenebilir.

Yükseltmek için en basit yolu, yeni ASP.NET MVC 4 proje ve kopya oluşturmak için tüm görünümleri, denetleyicileri, kod ve içerik dosyaları var olan MVC 3 projeden yeni projeye ve ardından derlemeyi güncelleştirmek için eski proje eşleşecek şekilde yeni projede başvurur ' dir. MVC 3 projesinin Web.config dosyasında değişiklik yaptıysanız, MVC 4 proje Web.config dosyasına de bu değişiklikleri birleştirmeniz gerekir.

El ile sürüm 4 varolan bir ASP.NET MVC 3 uygulamaya yükseltmek için aşağıdakileri yapın:

1. (Bir olduğundan proje, görünümler klasöründe, diğeri her alanı projenizdeki görünümleri klasöründeki kökündeki) projedeki tüm Web.config dosyasında aşağıdaki metni, her örneği değiştirin:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    Aşağıdaki karşılık gelen metinle:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Kök Web.config dosyasında güncelleştirme *webPages:Version* "2.0.0.0" öğesine ve yeni bir ekleme *PreserveLoginUrl* "true" değerini sahip anahtarı:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Çözüm Gezgini'nde referansı silme *System.Web.Mvc* (işaret ettiği sürüm 3 DLL). Bir başvuru ekleyin *System.Web.Mvc* (v4.0.0.0). Özellikle, derleme başvurularını güncelleştirmek için aşağıdaki değişiklikleri yapın. Ayrıntıları aşağıdadır:

    1. Çözüm Gezgini'nde, aşağıdaki derlemeler başvuruları silin: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Aşağıdaki derlemelerine başvurular ekleyin: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. Çözüm Gezgini'nde proje adına sağ tıklayın ve sonra projeyi seçin. Daha sonra yeniden adına sağ tıklayın ve Düzenle'yi seçin *ProjectName*.csproj.
5. Bulun *ProjectTypeGuids* öğesi ve {E3E379DF-F4C6-4180-9B81-6769533ABE47} {E53F8FEA-EAE0-44A6-8774-FFD645390401} değiştirin.
6. Değişiklikleri kaydetmek, düzenlemekte olduğunuz projesi (.csproj) dosyasını kapatın, projeye sağ tıklayın ve sonra projeyi yeniden yükle seçin.
7. Proje ASP.NET MVC'ın önceki sürümlerini kullanan derlenmiş tüm üçüncü taraf kitaplıklar başvuruyorsa, kök Web.config dosyasını açın ve aşağıdaki üç ekleyin *bindingRedirect* altında öğelerin  *Yapılandırma* bölümü: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>ASP.NET MVC 4 Beta sürümündeki yeni özellikler

Bu bölümde tanıtılmıştır özellikleri açıklanmıştır ASP.NET MVC 4 Beta sürümünde.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

HTTP Hizmetleri oluşturmak için yeni bir çerçeve tarayıcılar ve mobil cihazlar dahil olmak üzere istemcileri geniş bir yelpazedeki ulaşabilir, ASP.NET MVC 4 şimdi ASP.NET Web API içerir. ASP.NET Web API de RESTful hizmetlerini geliştirmek için ideal bir platformdur.

ASP.NET Web API aşağıdaki özellikleri destekler:

- **Modern HTTP programlama modeli:** doğrudan erişmek ve HTTP istekleri ve yeni, kesin türü belirtilmiş HTTP nesne modelini kullanarak, Web API'leri yanıtları değiştirmek. Aynı programlama modeli ve HTTP ardışık düzen yeni HttpClient türü istemcinize simetrik olarak kullanılabilir.
- **Tam yollar için destek**: Web API artık her zaman bir rota parametrelerinin ve sınırlamalar da dahil olmak üzere Web yığını parçası olan rota özellikler kümesini destekler. Ayrıca, artık [HttpPost] gibi öznitelikleri uygulamak gereken şekilde eylemlerine eşleme kuralları için tam destek, sınıflar ve yöntemler vardır.
- **İçerik anlaşması**: istemci ve sunucu birlikte API'den döndürülen veriler için doğru biçimde belirlemek için çalışabilir. Biz XML, JSON ve Form URL'SİNİN kodlanmış biçimleri için varsayılan destek sağlar ve bu destek, kendi biçimlendiricileri ekleyerek genişletebilir veya hatta varsayılan içerik anlaşması stratejisi değiştirin.
- **Model bağlama ve doğrulama:** Model bağlayıcıları çeşitli kısımlarını bir HTTP isteği verileri ayıklar ve bu ileti bölümleri Web API eylemleri tarafından kullanılan .NET nesnelerini dönüştürmek için kolay bir yol sağlar.
- **Filtreler:** Web API'leri artık [Authorize] özniteliği gibi bilinen filtreler gibi filtreler destekler. Yazar ve Eylemler, yetkilendirme ve özel durum işleme için kendi filtreleri takın.
- **Sorgu oluşturma:** yalnızca Iqueryable döndürerek&lt;T&gt;, Web API OData URL kurallarına sorgulama destekleyecektir.
- **Geliştirilmiş HTTP ayrıntıları edilebilirliğini:** statik bağlam nesneleri ayar HTTP ayrıntıları yerine Web API Eylemler artık HttpRequestMessage ve bilgisayarın HttpResponseMessage örnekleri ile çalışabilirsiniz. Bu nesneler genel sürümleri HTTP türlerine ek olarak, özel türleriyle çalışmasına izin vermek için de mevcut.
- **Ters çevirmeyi denetim (IOC) DependencyResolver aracılığıyla geliştirilmiş:** Web API artık MVC'nin bağımlılık çözümleyici tarafından uygulanan hizmet bulucu deseni birçok farklı tesisler için örnek elde için kullanır.
- **Kod tabanlı yapılandırma:** , config bırakarak dosyaları temizleme, Web API yapılandırmasını yalnızca kod üzerinden gerçekleştirilir.
- **Kendini barındırma:** Web API'leri barındırılması IIS yanı sıra kendi işleminde hala gücünden yolların ve diğer Web API özelliklerini kullanırken.

ASP.NET Web API hakkında daha fazla ayrıntı için lütfen adresi ziyaret edin [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET tek sayfa uygulaması

ASP.NET MVC 4 artık JavaScript ve Web API'leri kullanarak önemli istemci tarafı etkileşimler ile tek sayfa uygulamaları oluşturmak için deneyimi erken bir önizlemesini içerir. Bu destek içerir:

- Önbelleğe alınan veriler ile daha zengin yerel etkileşim için JavaScript kitaplıkları kümesi
- İş ve DAL destek birimi için ek Web API bileşenleri
- Hızlıca başlamak için yapı iskelesi ile bir MVC proje şablonu

ASP.NET MVC 4'te tek sayfa uygulaması hakkında daha fazla ayrıntı desteği için lütfen şu adresi ziyaret [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Varsayılan proje şablonları geliştirmeler

Yeni ASP.NET MVC 4 proje oluşturmak için kullanılan şablon daha modern görünümlü bir Web sitesi oluşturmak için güncelleştirilmiştir:

![](mvc4-beta-release-notes/_static/image1.png)

Yüzeysel geliştirmeleri yanı sıra var. Yeni şablon işlevleri geliştirilmiştir. Şablon hem masaüstü tarayıcıları, hem de mobil tarayıcılar hiçbir özelleştirme gerektirmeden iyi aramak için Uyarlamalı işleme adında bir teknik kullanır.

![](mvc4-beta-release-notes/_static/image2.png)

Eylem Uyarlamalı işlemede görmek için mobil öykünücü kullanın veya yalnızca masaüstü tarayıcı penceresini daha küçük olacak şekilde yeniden boyutlandırmayı deneyin. Tarayıcı penceresini küçük aldığında, sayfanın düzenini değiştirir.

Başka bir varsayılan proje şablonu için daha zengin bir kullanıcı Arabirimi sağlamak için JavaScript kullanımını yeniliktir. Şablonda kullanılan oturum açma ve kaydetme bağlantılar zengin oturum açma ekranı sunmak için kullanıcı Arabirimi iletişim kutusu jQuery kullanma örnekleri şunlardır:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobil proje şablonu

Yeni bir proje başlatıyorsanız ve tablet tarayıcılar ve mobil için özel olarak bir site oluşturmak isterseniz, yeni mobil uygulama proje şablonu kullanabilirsiniz. Bu jQuery Mobile, dokunma özelliği iyileştirilmiş bir kullanıcı Arabirimi oluşturmak için bir açık kaynak kitaplığı temel alır:

![](mvc4-beta-release-notes/_static/image4.png)

Bu şablon Internet uygulama şablonu aynı uygulama yapısını içerir (ve denetleyici kodlarının neredeyse aynıdır), ancak iyi görünür ve iyi dokunma tabanlı mobil cihazlarda hareket etmesi için jQuery Mobile'ı kullanmaya stili. Yapı ve mobil UI stil hakkında daha fazla bilgi için bkz: [jQuery mobil proje Web sitesi](http://jquerymobile.com/).

Mobil iyileştirilmiş görünümlerine eklemek istediğiniz veya tek bir site oluşturmak istiyorsanız, masaüstü ve mobil tarayıcılar farklı stilde görünümlerine hizmet veren bir masaüstü odaklı siteniz varsa, yeni görüntü modları özelliğini kullanabilirsiniz. (Sonraki bölüme bakın.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Görüntü modları

Yeni görüntü modları özelliği isteği yapan tarayıcının bağlı olarak görünümleri seçin bir uygulama sağlar. Örneğin, bir masaüstü tarayıcısı giriş sayfası isterse, uygulama Views\Home\Index.cshtml şablonu kullanabilir. Bir mobil tarayıcı giriş sayfası isterse, uygulama Views\Home\Index.mobile.cshtml şablonu döndürebilir.

Ayrıca düzenleri ve kısmi belirli tarayıcı türleri için geçersiz kılınabilir. Örneğin:

- Görünümler/paylaşılan klasörünü hem içeriyorsa \_Layout.cshtml ve \_Layout.mobile.cshtml şablonları, uygulama kullanacağı varsayılan \_mobil tarayıcılar ve gelenisteklerisırasındaLayout.mobile.cshtml\_Diğer istekleri sırasında Layout.cshtml.
- Her ikisi de bir klasör içeriyorsa, \_MyPartial.cshtml ve \_MyPartial.mobile.cshtml, yönerge @Html.Partial("\_MyPartial") kılacak \_MyPartial.mobile.cshtml mobil gelen istekleri sırasında Tarayıcılar ve \_diğer istekleri sırasında MyPartial.cshtml.

Daha özel görünümler, düzenler veya diğer aygıtlar için kısmi görünümleri oluşturmak istiyorsanız, yeni bir kaydedebilirsiniz *DefaultDisplayMode* örneği, bir istek belirli koşullar sağladığında aramak için ad belirtin. Örneğin, aşağıdaki kodu ekleyebilirsiniz *uygulama\_Başlat* "iPhone" dizesi Apple iPhone tarayıcı bir istekte bulunduğunda, geçerli bir görüntü modu kaydetmek için Global.asax dosyasındaki yöntemi:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Bir Apple iPhone tarayıcı bir istekte bulunduğunda bu kodu çalıştıktan sonra uygulamanız görünümler/paylaşılan kullanacak\\(varsa) _Layout.iPhone.cshtml düzeni.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, görünüm değiştirici ve tarayıcı geçersiz kılma

jQuery Mobile, dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için bir açık kaynak kitaplığıdır. JQuery Mobile'ı ASP.NET MVC 4 uygulamayla kullanmak istiyorsanız, indirin ve başlamanıza yardımcı olacak bir NuGet paketi yükleyin. Visual Studio Paket Yöneticisi Konsolu'ndan yüklemek için aşağıdaki komutu yazın:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Bu, jQuery Mobile ve aşağıdakiler de dahil olmak üzere bazı yardımcı dosyaları yükler:

- Görünümler/paylaşılan/\_jQuery Mobile tabanlı bir düzen Layout.Mobile.cshtml.
- Görünümler/paylaşılan oluşan bir görünüm değiştiricibileşeni/\_ViewSwitcher.cshtml kısmi Görünüm ve ViewSwitcherController.cs denetleyicisi.

Paketi yükledikten sonra bir mobil tarayıcı kullanarak uygulamanızı çalıştırın (veya eşdeğer Firefox gibi [kullanıcı aracısı değiştirici](http://chrispederick.com/work/user-agent-switcher/) eklenti). JQuery Mobile Düzen işlemesi nedeniyle sayfalarınızı oldukça farklı görünüyor görürsünüz ve stil oluşturma. Bu yararlanmak için aşağıdakileri yapabilirsiniz:

- Mobile özgü görünüm geçersiz kılmaları altında açıklandığı gibi oluşturmak [görüntü modları](#_Toc303253810) önceki (örneğin, Views\Home\Index.cshtml mobil tarayıcılar için geçersiz kılmak için Views\Home\Index.mobile.cshtml oluşturma).
- Okuma [jQuery mobil belgelerine](http://jquerymobile.com/) mobil görünümlerde dokunma özelliği iyileştirilmiş kullanıcı Arabirimi öğeleri ekleme hakkında daha fazla bilgi için.

Bir mobil iyileştirilmiş web sayfaları için metni bir şey Masaüstü görünümünde veya kullanıcıların bir masaüstü sürümüne geçiş sağlayan sitenin tam modu gibi olan bağlantı eklemek için kuralıdır. JQuery.Mobile.MVC paketi bu amaç için bir örnek görünüm değiştirici bileşeni içerir. Görünümler/paylaşılan varsayılan olarak kullanılan\\_Layout.Mobile.cshtml Görünüm ve sayfa işlendiğinde şöyle görünür:

![](mvc4-beta-release-notes/_static/image5.png)

Ziyaretçilerin bağlantıya tıklarsanız, aynı sayfa Masaüstü sürümüne geçiş.

Masaüstü düzeninizi görünüm değiştirici varsayılan olarak dahil edilmez çünkü ziyaretçileri mobil moduna almanın bir yolu olmaz. Bunu etkinleştirmek için aşağıdaki başvurusunu ekleyin  *\_ViewSwitcher* Masaüstü düzeninizi yalnızca içinde  *&lt;gövde&gt;*  öğe:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Görünüm değiştirici tarayıcı geçersiz kılma adlı yeni bir özellik kullanır. Bu özellik bunların çıkıyormuş gibi istekleri kabul uygulamanızı sağlar olandan farklı bir tarayıcıdan (kullanıcı aracısı) gerçekte gelen oldukları. Aşağıdaki tabloda tarayıcı geçersiz kılma sağlayan yöntemler listelenmiştir.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | İsteğin asıl kullanıcı aracısı değerini belirtilen kullanıcı aracısını kullanarak geçersiz kılar. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Geçersiz kılma belirtilmişse isteğin Kullanıcı aracısını geçersiz kılma değeri veya asıl kullanıcı aracısı dizesi döndürür. |
| `HttpContext.GetOverriddenBrowser()` | Döndürür bir *HttpBrowserCapabilitiesBase* istek için ayarlanmış kullanıcı aracısı karşılık gelen örnek (gerçek veya geçersiz kılınan). Bu değer gibi özellikleri almak için kullanabileceğiniz *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Geçerli istek için hiçbir geçersiz kılınan Kullanıcı aracısını kaldırır. |

Tarayıcı geçersiz kılmak, ASP.NET MVC 4 çekirdek özelliğidir ve jQuery.Mobile.MVC paket yüklemeyin olsa bile kullanılabilir. Ancak, yalnızca görünüm, Düzen ve kısmi görünüm seçimi etkiler — bağımlı herhangi bir ASP.NET özelliği etkilemez *Request.Browser* nesnesi.

Varsayılan olarak, kullanıcı aracısı geçersiz kılma tanımlama bilgisi kullanılarak depolanır. Geçersiz kılma başka bir yerde (örneğin, bir veritabanında) depolamak istiyorsanız, varsayılan sağlayıcı değiştirebilirsiniz (*BrowserOverrideStores.Current*). Bu sağlayıcı için belgeleri ASP.NET MVC daha sonraki bir sürümüne eşlik etmek üzere kullanılabilir.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Visual Studio'da kod oluşturma için tarif

NuGet kullanarak yükleyebileceğiniz paketleri tabanlı çözüm özgü kodu oluşturmak Visual Studio yeni tarif özelliği sağlar. Tarif framework yerleşik kod oluşturucuları Ekle, alan, denetleyici Ekle, Görünüm Ekle için değiştirip için de kullanabilirsiniz kod oluşturma eklenti yazmak geliştiriciler için kolaylaştırır. Tarif NuGet paketleri olarak dağıtılan olduğundan kolayca kaynak denetimine iade ve projedeki tüm geliştiriciler ile otomatik olarak paylaşılan. Bir çözüm başına temelinde ayrıca kullanılabilir.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Zaman uyumsuz denetleyicileri görev desteği

Bir nesne türü döndüren olarak tek yöntemleri zaman uyumsuz eylem yöntemleri şimdi yazabilirsiniz *görev* veya *görev&lt;ActionResult&gt;*.

Örneğin, Visual C# 5 kullanıyorsanız (veya kullanarak [Async CTP'nin](https://msdn.microsoft.com/en-us/vstudio/async.aspx)), şuna benzeyen bir zaman uyumsuz eylem yöntemini oluşturabilirsiniz:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

Önceki eylem yöntemindeki çağrıları *newsService.GetHeadlinesAsync* ve *sportsService.GetScoresAsync* zaman uyumsuz olarak adlandırılır ve iş parçacığı havuzunun bir iş parçacığından engellemez.

Döndüren zaman uyumsuz eylem yöntemleri *görev* örnekleri de zaman aşımları destekler. Eylem yöntemi edilebilen yapmak için türünde bir parametre eklemek *CancellationToken* eylem yöntemi imzası. Aşağıdaki örnek, bir zaman aşımı süresi (milisaniye) 2500 sahip ve görüntüleyen bir zaman uyumsuz eylem yöntemini gösterir. bir *süresi sona erdi* zaman aşımı olursa istemciye görüntüleyin.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK'sı

ASP.NET MVC 4 Beta Windows Azure SDK'sını Eylül 2011 1.5 sürümünü destekler.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

- **ASP.NET MVC 4 Beta yükledikten sonra Visual Studio 2010 Service Pack 1 CSHTML/VBHTML Düzenleyicisi CSHTML/VBHTML düzenleyicisinde parçacığını veya JavaScript içindeki cshtml veya vbhtml dosyaları yazdıktan sonra uzun süre duraklatın.** Bu, az önce oluşturduğunuz ve henüz derlenmiş ASP.NET MVC 4 uygulamalarında oluşur.

    Derlemeleri bin klasöründe almak için projeyi derlemek için geçici bir çözüm değildir. Not, bin klasöründen derlemeler kaldıran projeyi temizleyin, düzenleyici sorun geri dönün.

    Bu bir sonraki sürümde düzeltilecektir.
- **C# proje şablonları için Visual Studio 11 Beta Global.asax.cs yanlış bağlantı dizesinde içerir.** Uygulama içinde belirtilen varsayılan bağlantı\_Visual Studio 11 Beta'da oluşturulan projeleri atlanmayan bir ters eğik çizgi içeren bir yerel veritabanı bağlantı dizesi içeren için başlangıç yöntemi (\) karakter. Bu, bir SqlException oluşturan bir Entity Framework DbContext erişim girişimleri sırasında bağlantı hatası sonuçlanır.

    Bu sorunu düzeltmek için uygulama ters eğik çizgi karakteri kaçış\_şu şekilde görünmesi Global.asax.cs yöntemi Başlat:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **.NET 4.5 hedef ASP.NET MVC 4 uygulamaları .NET 4.0 altında çalıştırdığınızda System.Net.Http.dll derleme erişme girişimi sırasında bir FileLoadException durum oluşturur.** ASP.NET MVC 4 uygulamaları .NET 4.5 altında oluşturulan içeren bir bağlama hangi bildiren bir FileLoadException sonuçlanır yeniden yönlendirme "yükleyemedi dosya veya derleme 'System.Net.Http' ya da bağımlılıklarından biri." uygulamanın ne zaman .NET 4.0 yüklü bir sistem üzerinde yürütülür. Bu sorunu düzeltmek için web.config dosyasından aşağıdaki bağlama yeniden yönlendirmesinin kaldırın:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Derleme bağlama öğesi değiştirilmiş web.config dosyasında aşağıdaki gibi görünmelidir:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **Visual Basic projeleri "Denetleyici Ekle" öğesini şablonunda çağrıldığında yanlış bir ad alanı oluşturur *** gelen bir alanı içinde.** Visual Basic kullanan ASP.NET MVC projesindeki bir alan için bir denetleyici eklediğinizde öğe şablonu yanlış ad alanı Denetleyicisi'nde ekler. Denetleyicideki tüm eylem gittiğinizde sonuç "dosya bulunamadı" hatası verilir.  
  
 Oluşturulan ad alanı her şeyi kök ad alanı sonra atlar. Örneğin, oluşturulan ad alanıdır *RootNamespace* olmalıdır, ancak *RootNamespace.Areas.AreaName.Controllers* .
- **Razor görüntüleme Altyapısı'deki son değişiklikler.** Yeniden yazımı Razor ayrıştırıcısı, bir parçası olarak, aşağıdaki türlerden sıradan kaldırıldığını *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Aşağıdaki yöntemlerden ayrıca kaldırıldı: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Bir ASP.NET MVC 4 uygulamaları/bin dizininde WebMatrix.WebData.dll dahil olduğunda, form kimlik doğrulaması için URL kazanır.** (Örneğin, "ASP.NET Web sayfaları ile Razor sözdizimi" dağıtılabilir bağımlılıklar ekleme iletişim kullanırken seçerek) uygulamanıza WebMatrix.WebData.dll derleme ekleme/account/oturum açma kimlik doğrulaması oturum açma yeniden yönlendirme kılar yerine / Hesap/ASP.NET MVC hesap denetleyicisi varsayılan beklendiği gibi oturum açın. Bu davranışı önlemek ve web.config kimlik bölümünde belirtilen URL zaten kullanmak için PreserveLoginUrl adlı appSetting ekleyin ve bunu true olarak ayarlayın: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Visual Studio 2010 ve Visual Web Developer 2010 yan yana yüklemeler için ASP.NET MVC 4 yüklemeye çalışırken yüklemek NuGet Paket Yöneticisi başarısız.** Her iki sürümü Visual Studio'nun zaten yüklendikten sonra Visual Studio 2010 ve Visual Web Developer 2010 ASP.NET MVC 4 yan yana çalıştırmak için ASP.NET MVC 4 yüklemeniz gerekir.
- **Önkoşullar zaten kaldırdınız ASP.NET MVC 4 kaldırma başarısız olur.** ASP.NET MVC düzgün bir şekilde kaldırmak için 4you ASP.NET MVC 4 Visual Studio kaldırmadan önce kaldırmanız gerekir.
- **Varsayılan Web API projesi çalıştırmaya yanlış yok RegisterApis yöntemini kullanarak yollar ekleme kullanıcıya doğrudan yönergeleri gösterir.** ASP.NET yönlendirme tablosunu kullanarak RegisterRoutes yönteminde yollar eklenmesi gerekir.
- **ASP.NET MVC 4 Beta yükleme, ASP.NET MVC 3 RTM uygulamaları keser.** Oluşturulan ASP.NET MVC 3 uygulamaları RTM'ye (ASP.NET MVC 3 araçları güncelleştirme sürümüyle değil) yayın gerektiren aşağıdaki değişiklikleri yana birimi ASP.NET MVC 4 Beta ile çalışması için. Proje derleme hataları bu güncelleştirmeleri sonuçları yapmadan oluşturma. 

    **Gerekli güncelleştirmeleri**

    1. Kök Web.config dosyasında yeni Ekle  *&lt;appSettings&gt;*  anahtar girişle *webPages:Version* ve değeri *1.0.0.0*.

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. Çözüm Gezgini'nde proje adına sağ tıklayın ve sonra projeyi seçin. Daha sonra yeniden adına sağ tıklayın ve Düzenle'yi seçin *ProjectName*.csproj.
    3. Aşağıdaki derleme başvurularını bulun: 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        Bunları aşağıdakiyle değiştirin:

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. Değişiklikleri kaydetmek, düzenlemekte olduğunuz ve projeye sağ tıklayın ve yeniden yükle seçeneğini belirleyin (.csproj) proje dosyasını kapatın.
