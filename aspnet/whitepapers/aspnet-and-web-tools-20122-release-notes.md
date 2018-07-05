---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: ASP.NET ve Web Araçları 2012.2 sürüm notları | Microsoft Docs
author: rick-anderson
description: ASP.NET ve Web Araçları 2012.2 sürüm notları.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: a5558648450650f9939a490ed669e4918e7ee36d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390470"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET ve Web Araçları 2012.2 sürüm notları
====================
> Bu belgede ASP.NET ve Web Araçları 2012.2 sürüm açıklanmaktadır. Visual Studio Web Araçları ve ASP.NET için bir güncelleştirmedir.


- [Yükleme notları](#_Installation)
- [Belgeler](#_Documentation)
- [Destek](#_Support)
- [Yazılım gereksinimleri](#_Software_Requirements)
- [ASP.NET ve Web Araçları 2012.2 yeni özellikler](#_New_Features_in)

    - [Araç kullanımı](#_Tooling)
    - [Web'de Yayımlama](#_Web_Publishing)
    - [ASP.NET MVC şablonları](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET kolay URL'leri](#_ASP.NET_Friendly_URLs)
- [Bilinen sorunlar ve yeni değişiklikler](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Yükleme notları

ASP.NET ve Web Araçları 2012.2, Visual Studio 2012 için kullanılarak yüklenebilir [Web Platformu yükleyicisi](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Visual Studio 2012 veya Visual Studio Express 2012 için gerekli olan Web budur. Visual Studio Express 2012 Web için Visual Studio yüklü değilse yüklenir.

Aynı zamanda ASP.NET ve Web Araçları 2012.2 el ile yükleyebilirsiniz. Visual Studio 2012 veya Visual Studio Express 2012 için Web yüklü olması gerekir. Ardından, aşağıdaki yönergeleri kullanın: 

1. İndirme [ASP.NET ve Frameworks 2012.2 Web](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) yükleyicisini İndirme Merkezi'nden.
2. Çalıştırdığınızda istendiğinde tıklayın. Ayrıca, daha sonra çalıştırılacak dosyanın kaydedebilirsiniz.
3. Visual Studio, güncelleştirme sürümünü doğrulayın. Güncelleştirmek istediğiniz Visual Studio başlatarak bunu yapabilirsiniz. Yardım menü öğesi'ye tıklayın.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Menü öğesi görürseniz &quot;hakkında Microsoft Visual Studio 2012 için Web&quot; sonra indirmeniz [Web geliştirici araçları 2012.2 - Visual Studio Express 2012 için Web](https://go.microsoft.com/fwlink/?LinkID=282228). Aksi takdirde indirme [Web geliştirici araçları 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Çalıştırdığınızda istendiğinde tıklayın. Ayrıca, daha sonra çalıştırılacak dosyanın kaydedebilirsiniz.

> [!NOTE]
> ASP.NET ve Web Araçları 2012.2 sürüm, SQL Server veri araçları içermez. SQL Server ve Windows Azure SQL veritabanlarını çevrimdışı proje destekli geliştirme, Şema karşılaştırma ve Gelişmiş veritabanı dağıtım özellikleri de dahil olmak üzere araç veritabanının daha zengin bir dizi sağlar. SQL Server veri Araçları'nı yüklemek için ya da daha fazla bilgi için ziyaret [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET ve Web Araçları 2012.2 ile ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Destek

ASP.NET ve Web Araçları 2012.2 resmi olarak yayımlanan ve desteklenir. Normal destek kanalınızı kullanabilirsiniz. ASP.NET forumları için de soru gönderebilir ([https://forums.asp.net/](https://forums.asp.net/)), burada ASP.NET topluluk üyelerinin resmi olmayan destek sık sağlayabilir.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET ve Web Araçları 2012.2 gerektirir Visual Studio 2012 veya Visual Studio Express 2012 için Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>ASP.NET ve Web Araçları 2012.2 yeni özellikler

Bu bölümde ASP.NET ve Web Araçları 2012.2 sürümünde sunulan özellikler açıklanır.

<a id="_Tooling"></a>
### <a name="tooling"></a>Araç kullanımı

- Sayfa Denetçisi 

    - Page Inspector'ı sayfasına karşılık gelen JavaScript kodunu dinamik olarak eklenen öğeleri eşlemek izin verme JavaScript seçimi eşleme destekler.
    - CSS görme olanağı, gerçek zamanlı olarak güncelleştirir.
    - Daha fazla bilgi için okuma [CSS otomatik eşitleme ve sayfa denetçisi JavaScript eşlemesini seçimi](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Düzenleyici 

    - CoffeeScript, Bıyık, Gidon ve JsRender için söz dizimi vurgulama destekler.
    - HTML düzenleyicisi altını bağlamaları için IntelliSense sağlar.
    - Daha az düzenleme ve derleyici daha az kullanarak dinamik CSS oluşturmayı destekler.
    - Bir .NET sınıfı olarak JSON Yapıştır. .NET sınıfları JSON'dan çıkarılan bir C# veya VB.NET kodu dosyası ve Visual Studio JSON yapıştırmak için bu Özel Yapıştır komut kullanarak otomatik olarak oluşturur.
- Mobil öykünücü desteği genişletilebilirlik kancaları ekler; böylece bir VSIX üçüncü taraf öykünücüleri yüklenebilir. Böylece geliştiriciler, çeşitli mobil cihazlarda sitelerinde önizleyebilirsiniz yüklü öykünücüleri F5 açılır menüde görünür. Scott Hanselman'ın blog girişinde'deki bu özellik hakkında daha fazla bilgiyi [yeni BrowserStack Visual Studio tümleştirmesi sayesinde](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Web'de Yayımlama

- Web sitesi projeleri artık Web uygulaması projelerinde yayımlama için Windows Azure Web siteleri dahil olmak üzere aynı yayımlama deneyimini var.
- Seçici yayımlama &#8211; için bir veya daha fazla dosya (sonra bir Web dağıtımı uç noktası yayımlama) aşağıdaki eylemleri gerçekleştirebilirsiniz: 

    - Seçili dosyaları yayımlayın.
    - Uzak bir dosyaya bir yerel dosya arasındaki farkı bakın.
    - Yerel dosya ile uzak dosya güncelleştirin veya uzak dosya yerel dosya ile güncelleştirin.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC şablonları

- Yeni Facebook uygulama şablonu, Facebook Canvas uygulamalarını yazmayı kolaylaştırır. Birkaç basit adımda, oturum açmış bir kullanıcıdan veri alan ve arkadaşlarıyla entegre olan bir Facebook uygulaması oluşturabilirsiniz. Şablon ilişkin kimlik doğrulama, izinler, Facebook verilerini ve daha fazlasını erişim de dahil olmak üzere Facebook uygulaması oluşturmaya tüm ayarlamaları halletmeniz için yeni bir kitaplık içerir. Facebook uygulaması şablonu kullanma hakkında daha fazla bilgi için bkz. [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Yeni bir tek sayfalı uygulama MVC şablonu geliştiricilerin HTML 5, CSS 3 ve popüler Knockout ve jQuery JavaScript kitaplıklarını, ASP.NET Web API üstünde etkileşimli istemci tarafı web uygulamaları geliştirebilirsiniz olanak tanır. Şablon, RESTful sunucu API'si kullanan bir JavaScript HTML5 uygulaması oluşturmaya yönelik yaygın yöntemleri gösteren bir "Yapılacaklar" listesi uygulaması içerir. Daha fazla bilgi edinebilirsiniz [ https://www.asp.net/single-page-application ](../single-page-application/index.md).
- Artık ASP.NET MVC yeni proje iletişim kutusuna yeni şablonlar ekleyen bir VSIX oluşturabilirsiniz. Nasıl yapacağınızı buradan öğrenin: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes paket &#8211; MVC proje şablonları, geçici bir çözüm için MVC 4'te hata içeren yeni bir 'FixedDisplayModes' NuGet paketini içerecek şekilde güncelleştirildi. Pakette yer alan düzeltme hakkında daha fazla bilgi için bu blog gönderisine bakın ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) MVC ekibinden.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API'si birkaç yeni özelliklerle geliştirilmiştir:

- ASP.NET Web API OData
- ASP.NET Web API izlemesi
- ASP.NET Web API Yardım sayfası

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData herhangi bir veri kaynağı üzerinde zengin iş mantığı ile OData uç noktaları oluşturmak için gereken esnekliği sunar. ASP.NET Web API OData ile kullanıma sunmak istediğiniz bir OData semantiği miktarını denetler. ASP.NET Web API OData ASP.NET MVC 4 proje şablonları bulunur ve Nuget'ten kullanılabilir ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData, şu anda aşağıdaki özellikleri destekler:

- OData sorgu semantiği [Queryable] özniteliği uygulayarak etkinleştirin.
- Kolayca OData sorgularını doğrulamak ve desteklenen sorgu seçenekleri, işleç ve işlevlerini kümesini sınırlayabilirsiniz.
- Ardından doğrulandı ve bir IEnumerable veya Iqueryable uygulanan sorgu soyut sözdizimi ağacını gösterimini doğrudan alınacağı ODataQueryOptions bağlama parametresi.
- Hizmet odaklı sayfalama ve sonraki sayfa bağlantısını oluşturma [Queryable] özniteliği sonucu sınırlar belirterek etkinleştirin.
- İstek bir satır sayısı $inlinecount kullanarak eşleşen kaynakların toplam sayısı.
- Null yaymayı denetler.
- $Filter/All işleci.
- Bir varlık veri modeli, kural olarak tanım Çıkarsama veya açıkça bir model Entity Framework Code-First benzer bir şekilde özelleştirebilirsiniz.
- Expose varlık EntitySetController türetme tarafından ayarlar.
- Gezinti özellikleri kullanıma sunan ve OData eylemleri uygulama bağlantıları yönetmek için basit, özelleştirilebilir kuralları.
- Basitleştirilmiş MapODataRoute genişletme yöntemi kullanarak yönlendirme.
- Birden çok EDM modeli gösterme tarafından sürüm oluşturma desteği.
- İstemciler (.NET, Windows Phone, Windows Store, vb.) oluşturabilmek hizmet belgesi ve $metadata Web API'niz için kullanıma sunar.
- OData Atom, JSON ve JSON verbose biçimleri için destek.
- Oluşturma, güncelleştirme, kısmen (düzeltme) güncelleştirmesi ve varlıkları silin.
- Sorgu ve varlıklar arasındaki ilişkilerin arabirimidir.
- En fazla yollarınızı wire ilişki bağlantılarını oluştur.
- Karmaşık türler.
- Varlık türü devralma.
- Koleksiyon Özellikleri.
- Sabit listeleri.
- OData eylemleri.
- WCF Veri Hizmetleri, yani ODataLib ile aynı temel üzerine ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

ASP.NET Web API OData hakkında daha fazla bilgi için bkz. [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web API izlemesi

ASP.NET Web API izleme, web API'leri izleme verileri .NET izleme ile tümleştirilir. Şimdi Web API proje şablonunu varsayılan olarak etkinleştirilir. Web için veri izleme API'leri, çıkış penceresine gönderilen ve IntelliTrace kullanılabilir hale gelir. ASP.NET Web API izleme sayesinde Web tümleştirmesi sayesinde Windows Azure'da barındırılan, API'nizi izleme bilgilerine [Windows Azure tanılama](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Ayrıca yükleme ve ASP.NET Web API izleme NuGet paketini kullanarak herhangi bir uygulamada ASP.NET Web API izlemeyi etkinleştir ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Yapılandırma ve ASP.NET Web API izleme kullanma hakkında daha fazla bilgi için bkz. [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web API Yardım sayfası

ASP.NET Web API Yardım sayfası artık varsayılan olarak Web API proje şablonunu dahil edilmiştir. ASP.NET Web API Yardım sayfası, web API'leri dahil olmak üzere HTTP uç noktaları, desteklenen HTTP yöntemleri, parametreleri ve örnek istek ve yanıt iletisi yükü belgelerine otomatik olarak oluşturur. Belgeleri otomatik olarak kodunuza yorumlar öğesinden alınır. ASP.NET Web API Yardım sayfası NuGet paketini kullanarak herhangi bir uygulama için ASP.NET Web API Yardım sayfasında de ekleyebilirsiniz ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Ayarlama ve ASP.NET Web API Yardım sayfasında bakın özelleştirme hakkında daha fazla bilgi için [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR kullanarak varsa ve bunu olmadığı durumlarda otomatik olarak diğer teknikleri ile dönülüyor ASP.NET uygulamanız için gerçek zamanlı web özellikleri eklemenizi kolaylaştırır.

ASP.NET SignalR kullanma hakkında daha fazla bilgi için bkz. [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET kolay URL'leri

ASP.NET FriendlyURLs Temizleyicisi URL'leri (.aspx uzantısı olmadan) bakarak üretmek web forms geliştiriciler için çok kolay hale getirir. Bu, hiçbir çok az yapılandırma gerektirir ve mevcut ASP.NET v4.0 uygulamaları ile kullanılabilir. FriendlyURLs özellik ayrıca, geliştiricileri, masaüstü ve mobil görünümler arasında geçiş destekleyerek uygulamalarını, mobil desteği eklemek için kolaylaştırır.

Yükleme ve ASP.NET kolay URL'leri kullanma hakkında daha fazla bilgi için bkz. [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET ve Web Araçları 2012.2 sürümdeki bozucu değişiklikleri açıklanmaktadır.

### <a name="installation-issues"></a>Yükleme Sorunları

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Visual Studio 2012'in sıralamaya yükler

Bir ek SKU Visual Studio 2012, ASP.NET ve Web Araçları 2012.2 yükleme, onarım işlemi gerektirecek sonra yükleme. Aşağıdakiler göz önünde bulundurun:

1. Web için Visual Studio 2012 Express yükleyin
2. ASP.NET ve Web Araçları 2012.2 yükleme
3. Visual Studio 2012 Professional, Premium veya Ultimate yükleyin

2. adım, güncelleştirmeleri yükleme için Web için Express yalnızca neden olur. 3. adımda yüklü ek SKU güncelleştirme içerdiğinden emin olmak için ASP.NET ve Web Araçları 2012.2 son SKU için yüklü güncelleştirmeleri yüklemeyi onarmak gerekir. Adım 1'deki SKU'ları ve 3 ters de geçerlidir.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Visual Studio açık olduğunda, Microsoft ASP.NET ve Web Araçları 2012.2 yükleme

Visual Studio VS ise açık Microsoft ASP.NET ve Web Araçları 2012.2 yüklemesi sırasında hatalı durumda çıkabilir. Kullanıcılar yüklemeye devam etmeden önce Visual Studio'nun tüm örneklerini kapatın önerilir.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>ASP.NET ve Web Araçları 2012.2 Kurulum ortasında yükleme iptal ediliyor

ASP.NET ve Web Araçları 2012.2 iptal Kurulum yükleme ortasında Visual Studio hatalı bir durumda bırakır. Bu sorun izleme adımları adresini belirlemek için: 

- Program Kaldır'ı eklemek için Git
- Microsoft ASP.NET ve Web Araçları 2012.2, varsa kaldırın.
- Microsoft ASP.NET ve Web Araçları 2012.2 yükleyin

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>ASP.NET ve Web Araçları 2012.2 ASP.NET MVC 4 kaldırdıktan sonra şablonları ve Razor v2 Web sitesi şablonları eksik

ASP.NET ve Web Araçları 2012.2 kaldırma da tüm ASP.NET MVC 4 ve Razor v2 Web sitesi şablonları Visual Studio 2012'den kaldırır.

ASP.NET MVC 4 ve Razor v2 Web sitesi şablonları yeniden yüklemek için Visual Studio 2012 yüklemenizi onarmak için çözüm olabilir.

### <a name="tooling-issues"></a>Araçları sorunları

#### <a name="nuget-error-reported-during-project-creation"></a>Proje oluşturma sırasında bildirilen NuGet hata

ASP.NET ve Web Araçları 2012.2 yükledikten sonra aşağıdaki hata MVC 4 projelerini oluştururken görebilirsiniz

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

ASP.NET ve Web Araçları 2012.2 NuGet 2.1 gelir ve Visual Studio 2012'de uzantı yükseltir. VSIX yükleyici, bazı durumlarda, VSIX doğru şekilde güncelleştirmek başarısız olur. Aşağıdaki adımlar bu sorunu gidermek için izin verir:

1. Visual Studio 2012 yönetici olarak başlatın.
2. Git Araçları -&gt;Uzantılar ve güncelleştirmeler ve NuGet kaldırın.
3. Visual Studio’yu kapatın
4. ASP.NET ve Web Araçları 2012.2 yükleme klasörüne gidin:

    1. Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Web için Visual Studio 2012 Express: **Web için Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012**
5. NuGet yeniden NuGet.Tools.vsix çift tıklayın

### <a name="web-api-issues"></a>Web API sorunları

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>$Filter ve DateTime değişmez değerleri ayrıştırma sorunları

OData URI ayrıştırıcısı kısmi datetime değişmez değerler doğru ayrıştırılamaz. Örneğin, $filter başlangıç eq datetime'2012 =-12-31T12:00' düzgün ayrıştırılamaz. Geçici bir çözüm tam değişmez değeri $filter kullanmaktır başlangıç eq datetime'2012 =-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData büyük küçük harf duyarsız özellik adlarını desteklemez.

OData, OData sorgularını ve odata yolu büyük küçük harf duyarsız özellik adlarını desteklemez. İş öğeleri bakın:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Kullanıcılarınız farklı büyük/küçük harf javascript istemci tarafı ve sunucu tarafı varsa, bunlar büyük olasılıkla bu sorunla karşılaşır. Bu sorun, odata Protokolü tasarım gereğidir. Ancak, çok sayıda kullanıcı, bu sorunu raporlar. Bunu çözmek için kullanıcıların kendi URL durumlarda düzeltmek gerekmez.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Varsayılan OData yönlendirme kuralları POST/PUT gezinti özelliği desteklemiyor.

Varsayılan OData yönlendirme kuralları POST/PUT gezinti özelliği desteklemiyor. İş öğesi bkz [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Yaygın olarak kullanılan bu kurala varsayılan kuralları eksik.

Bunu çözmek için onu desteklemek için yeni yönlendirme kuralı genişletmek kullanıcıların gerekir.

### <a name="facebook-template-issues"></a>Facebook şablon sorunları

#### <a name="facebook-application-template-only-works-using-net-45"></a>Facebook uygulama şablonu, yalnızca .NET 4.5 kullanarak çalışır

.NET 4.5, ASP.NET MVC 4'te Facebook uygulama şablonu görmek için yeni proje iletişim kutusunda framework açılan listedeki seçmeniz gerekir.

#### <a name="real-time-update-controller"></a>Gerçek zamanlı güncelleştirme denetleyicisi

Kullanıcının Facebook uygulama şablonu sağlar, kolayca Facebook gerçek zamanlı güncelleştirmeler işlemek için bir Web API denetleyicisi oluşturun. Geliştirme makinenizde NAT arkasında ise, denetleyiciniz daha fazla ağ yapılandırma çalışmayabilir. Ayrıntılar için buraya bakın: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Sorgu dizesi değerlerini Facebook OAuth parametrelerle çakışıyor

Aşağıdaki alanları Facebook OAuth iletişim kutusunun çağrısı ile çakışan ayrıntılara dönüş URL'si. Şu adları kendi sorgu dize değerleriyle eklemeyin: kod, hata, hata\_açıklaması, hata\_nedeni.

#### <a name="using-page-inspector-with-facebook-template"></a>Facebook şablon ile sayfa denetçisini kullanma

Facebook uygulamanızı hata ayıklama sırasında Visual Studio 2012'de sayfa denetçisi özelliğini kullanamazsınız. Sayfa denetçisi IFRAMES şu anda desteklemiyor.

### <a name="single-page-application-template-issues"></a>Tek sayfalı uygulama şablonu sorunları

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Varsayılan MVC SPA proje, yeni todo öğesini düzenleme çalıştırırken 1.9/Knockout 2.2.1 güncelleştirme girin JQuery ile odak olayı düzgün işlenmiyor.

JQuery 1.9/Knockout ile 2.2.1 güncelleştirme, varsayılan MVC SPA proje çalıştırırken, yeni todo öğesini düzenleme girin artık odak geri yeni todo öğesini düzenleme kutusuna yapılacaklar listesine yeni todo öğesini girdikten sonra.

Geçici çözüm başvurusuna [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html), aşağıdaki örnek kod için benzer düzeltme yapın:

Dosya todo.model.js  
 todolist(Data) işlev, ekleme aşağıdaki:  
 **self.isSelected ko.observable(false); =**

todoList.prototype.addTodo işlev, aşağıdaki blacked metni ekleyin:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Index.cshtml dosyası, aşağıdaki blacked metni ekleyin:  
 &lt;Form data-bind =&quot;gönderin: addTodo&quot;&gt;  
 &lt;Giriş sınıfı =&quot;addTodo&quot; türü =&quot;metin&quot; data-bind =&quot;değer: newTodoTitle, yer tutucu: 'eklemek için buraya Type', blurOnEnter: true, **hasfocus: IsSelected**, olay: {bulanıklaştıran: addTodo}&quot; /&gt;  
 &lt;tanımlanması&gt;
