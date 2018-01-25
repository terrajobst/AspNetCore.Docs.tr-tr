---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: "ASP.NET ve Web Araçları 2012.2 sürüm notları | Microsoft Docs"
author: rick-anderson
description: "ASP.NET ve Web Araçları 2012.2 için sürüm notları."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: ab1642f1a3de298919aa9c6c1ddbd6bbb0cb99b5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET ve Web Araçları 2012.2 sürüm notları
====================
> Bu belgede, ASP.NET ve Web Araçları 2012.2 sürümü açıklanmaktadır. Visual Studio Web Tooling ve ASP.NET için bir güncelleştirmedir.


- [Yükleme notları](#_Installation)
- [Belgeler](#_Documentation)
- [Destek](#_Support)
- [Yazılım gereksinimleri](#_Software_Requirements)
- [ASP.NET ve Web Araçları 2012.2 yeni özellikler](#_New_Features_in)

    - [Araçları](#_Tooling)
    - [Web'de Yayımlama](#_Web_Publishing)
    - [ASP.NET MVC şablonları](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET kolay URL'leri](#_ASP.NET_Friendly_URLs)
- [Bilinen sorunlar ve yeni değişiklikler](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Yükleme notları

ASP.NET ve Web Araçları 2012.2 için Visual Studio 2012 kullanılarak yüklenebilir [Web Platformu yükleyicisi](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Visual Studio 2012 veya Visual Studio Express 2012 için gerekli olan Web güncelleştirmeyi budur. Visual Studio Express 2012 Web için Visual Studio yüklü değilse yüklenir.

Ayrıca ASP.NET ve Web Araçları 2012.2 el ile yükleyebilirsiniz. Visual Studio 2012 veya Visual Studio Express 2012 için Web yüklü olması gerekir. Ardından aşağıdaki yönergeleri kullanın: 

1. Karşıdan [ASP.NET ve Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) yükleyici İndirme Merkezi'nden.
2. İstendiğinde tıklatın çalıştırdığınızda. Daha sonra çalıştırılacak dosya da kaydedebilirsiniz.
3. Visual Studio güncelleştirmek sürümünü doğrulayın. Bu, Visual Studio, güncelleştirmek istediğiniz başlatarak yapabilirsiniz. Yardım menü öğesi'ye tıklayın.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Menü öğesi görürseniz &quot;hakkında Microsoft Visual Studio 2012 için Web&quot; sonra karşıdan [Web geliştirici araçları 2012.2 - Visual Studio Express 2012 için Web](https://go.microsoft.com/fwlink/?LinkID=282228). Aksi takdirde karşıdan [Web geliştirici araçları 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. İstendiğinde tıklatın çalıştırdığınızda. Daha sonra çalıştırılacak dosya da kaydedebilirsiniz.

> [!NOTE]
> ASP.NET ve Web Araçları 2012.2 sürüm SQL Server veri araçları içermez. SQL Server ve Windows Azure SQL veritabanlarını, veritabanı çevrimdışı proje destekli geliştirme, Şema karşılaştırma ve geliştirilmiş veritabanı dağıtım yetenekleri de dahil olmak üzere tooling daha zengin bir dizi sağlar. Daha fazla bilgi için veya SQL Server veri araçları yüklemek için ziyaret [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET ve Web Araçları 2012.2 ilgili diğer bilgileri, ASP.NET web sitesinden (https://www.asp.net) kullanılabilir.

<a id="_Support"></a>
## <a name="support"></a>Destek

ASP.NET ve Web Araçları 2012.2 resmi olarak yayımlanan ve desteklenir. Normal bir destek kanalıyla kullanabilirsiniz. Sorular için ASP.NET forumları nakledebilirsiniz ([https://forums.asp.net/](https://forums.asp.net/)), burada ASP.NET topluluk üyeleri resmi olmayan destek sık sağlayabilir.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET Web Araçları 2012.2 gerektirir ve Visual Studio 2012 veya Visual Studio Express 2012 için Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>ASP.NET ve Web Araçları 2012.2 yeni özellikler

Bu bölümde, ASP.NET ve Web Araçları 2012.2 sürümünde tanıtılan özellikleri açıklanmıştır.

<a id="_Tooling"></a>
### <a name="tooling"></a>Araçları

- Sayfa Denetçisi 

    - Sayfa Denetçisi'nin karşılık gelen JavaScript kodu sayfasına geri dinamik olarak eklenme öğeleri eşleme izin vererek JavaScript seçimi eşleme destekler.
    - CSS görme olanağı gerçek zamanlı olarak güncelleştirir.
    - Daha fazla bilgi için okuma [CSS otomatik eşitleme ve JavaScript seçimi sayfa denetçisi eşlemede](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Düzenleyici 

    - CoffeeScript, Mustache, Handlebars ve JsRender için söz dizimi vurgulama destekler.
    - HTML düzenleyicisi Boşaltılan bağlamaları için IntelliSense sağlar.
    - Daha az kullanarak dinamik CSS oluşturmayı etkinleştirmek için daha az düzenleme ve derleyici destekler.
    - JSON bir .NET sınıfı olarak yapıştırın. Bir C# veya VB.NET kod dosyası ve Visual Studio JSON yapıştırmak için bu Özel Yapıştır komut kullanarak otomatik olarak JSON öğesinden çıkarımı yapılan .NET sınıfları oluşturur.
- Mobil öykünücü desteği genişletilebilirlik kancaları ekler, böylece üçüncü taraf Öykünücüler VSIX yüklenebilir. Böylece geliştiriciler, çeşitli mobil aygıtlarda sitelerinde önizleyebilirsiniz yüklü Öykünücüler F5 açılır listede görünür. Scott Hanselman'ın blogu girişinde'deki bu özellik hakkında daha fazla bilgiyi [Visual Studio ile yeni BrowserStack tümleştirme](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Web'de Yayımlama

- Web sitesi projeleri yayımlama için Windows Azure Web siteleri de dahil olmak üzere Web Uygulama projeleri olarak aynı yayımlama deneyimi artık sahiptir.
- Seçici yayımlama &#8211; bir veya daha fazla dosyaları (sonra bir Web dağıtımı uç noktası yayımlamayı) aşağıdaki eylemleri gerçekleştirebilirsiniz: 

    - Seçili dosyaları yayımlayın.
    - Bir yerel dosya ve uzak bir dosya arasındaki farkı bakın.
    - Yerel dosya ile uzak dosya güncelleştirmek veya uzak dosya yerel dosya ile güncelleştirin.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC şablonları

- Yeni Facebook uygulama şablonu, Facebook Canvas uygulamalarını yazmayı kolaylaştırır. Birkaç basit adımda, oturum açmış bir kullanıcıdan veri alan ve arkadaşlarıyla entegre bir Facebook uygulaması oluşturabilirsiniz. Şablon tüm ayarlamaları kimlik doğrulaması, Facebook verilerine ve daha fazlasını erişme izinleri de dahil olmak üzere bir Facebook uygulaması oluşturmaya ilişkin halletmeniz için yeni bir kitaplık içerir. Facebook uygulama şablonu kullanma hakkında daha fazla bilgi için bkz: [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Yeni bir tek sayfalı uygulama MVC şablonu geliştiricilerin HTML 5, CSS 3 ve popüler Knockout ve ASP.NET Web API üstünde jQuery JavaScript kitaplıklarını kullanarak etkileşimli istemci tarafı web uygulamaları oluşturmalarına olanak sağlar. Şablon, RESTful sunucu API'si kullanan bir JavaScript HTML5 uygulaması oluşturmaya yönelik yaygın yöntemleri gösteren bir "Yapılacaklar" listesi uygulaması içerir. Hakkında daha fazla bilgi edinebilirsiniz [https://www.asp.net/single-page-application](../../../single-page-application/index.md).
- ASP.NET MVC yeni proje iletişim kutusuna yeni şablonlar ekler VSIX artık oluşturabilirsiniz. Nasıl yapabileceğinizi öğrenin: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes paket &#8211; MVC proje şablonları, geçici bir çözüm için MVC 4'te hata içeren yeni 'FixedDisplayModes' NuGet paketini içerecek şekilde güncelleştirildi. Pakette yer alan düzeltme hakkında daha fazla bilgi için bu blog gönderisine bakın ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) MVC ekibinden.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API bazı yeni özelliklerle geliştirilmiştir:

- ASP.NET Web API OData
- ASP.NET Web API izlemesi
- ASP.NET Web API Yardım sayfası

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData için herhangi bir veri kaynağı üzerinde zengin iş mantığı ile OData uç noktaları oluşturmak için ihtiyacınız esnekliği sağlar. ASP.NET Web API OData ile kullanıma sunmak istediğiniz OData semantiği miktarını denetler. ASP.NET Web API OData ASP.NET MVC 4 Proje şablonlarıyla dahil edilmiştir ve ayrıca Nuget'ten kullanılabilir ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData şu anda aşağıdaki özellikleri destekler:

- OData sorgu semantiği [Queryable] özniteliği uygulayarak etkinleştirin.
- Kolayca OData sorgularını doğrulamak ve desteklenen sorgu seçenekleri, işleçler ve işlevler kümesi kısıtlayın.
- Parametre bağlama ODataQueryOptions doğrudan sonra doğrulanması ve bir Iqueryable veya IEnumerable uygulanan sorgu bir soyut söz dizimi ağaç gösterimini alır.
- Disk belleği hizmet odaklı ve sonraki sayfa bağlantısı oluşturma [Queryable] özniteliği sonuç sınırlar belirterek etkinleştirin.
- Bir satır içi sayısını toplam $inlinecount kullanarak eşleşen kaynakları isteyin.
- Null yaymayı denetler.
- $Filter öğesinde hiçbir/All işleçler.
- Bir varlık veri modeli kuralı olarak Infer veya açıkça bir model Entity Framework kod-ilk için benzer bir şekilde özelleştirebilirsiniz.
- Sunmaya varlık EntitySetController türetme göre ayarlar.
- Gezinti özellikleri gösterme, bağlantılar düzenleme ve OData eylemleri uygulamak için basit, özelleştirilebilir kuralları.
- MapODataRoute genişletme yöntemi kullanarak yönlendirme Basitleştirilmiş.
- Birden çok EDM modelleri gösterme tarafından sürüm oluşturma için destek.
- İstemcileri (.NET, Windows Phone, Windows mağazası, vb.) oluşturmak için hizmet belgesini ve $metadata Web API'nize kullanıma sunar.
- OData Atom, JSON ve JSON verbose biçimleri için destek.
- Oluştur, Güncelleştir, kısmen (düzeltme eki) güncelleştirin ve varlıkları silme.
- Sorgulamak ve varlıklar arasındaki ilişkiler arabirimidir.
- Yollarınızı kadar wire ilişki bağlantılar oluşturabilirsiniz.
- Karmaşık türler.
- Varlık türü devralma.
- Koleksiyon Özellikleri.
- Numaralandırmalar.
- OData eylemler.
- WCF Veri Hizmetleri, yani ODataLib olarak aynı temel üzerine kurulu ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

ASP.NET Web API OData hakkında daha fazla bilgi için bkz: [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web API izlemesi

ASP.NET Web API izleme, web API izleme verileri .NET izleme ile tümleşir. Şimdi, Web API projesi şablonundaki varsayılan olarak etkindir. Web için veri izleme API'leri çıkış penceresine gönderilir ve IntelliTrace kullanılabilir hale getirilir. ASP.NET Web API Tracing sağlar, Web ile tümleştirme yoluyla Windows Azure üzerinde barındırılan API izleme bilgilerine [Windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Ayrıca yüklemek ve ASP.NET Web API izleme NuGet paketi kullanarak herhangi bir uygulamada ASP.NET Web API Tracing etkinleştirin ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Yapılandırma ve ASP.NET Web API Tracing kullanma hakkında daha fazla bilgi için bkz: [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web API Yardım sayfası

ASP.NET Web API Yardım sayfası artık varsayılan olarak Web API projesi şablonunda dahil edilmiştir. ASP.NET Web API Yardım sayfasında web HTTP uç noktaları, desteklenen HTTP yöntemleri, parametreleri ve örnek istek ve yanıt iletisi yükü de dahil olmak üzere API belgelerine otomatik olarak oluşturur. Belge açıklamaları kodunuzda gelen otomatik olarak alınır. ASP.NET Web API Yardım sayfası NuGet paketi kullanarak herhangi bir uygulama için ASP.NET Web API Yardım sayfasında da ekleyebilirsiniz ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Ayarlama ve ASP.NET Web API Yardım sayfasında bakın özelleştirme hakkında daha fazla bilgi için [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR WebSockets varsa kullanarak ve onu değilken, otomatik olarak geri başka teknikler denk gelen ASP.NET uygulamanız için gerçek zamanlı web yetenekleri eklemek basit kolaylaştırır.

ASP.NET SignalR kullanma hakkında daha fazla bilgi için bkz: [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET kolay URL'leri

ASP.NET FriendlyURLs temizleyici URL'leri (.aspx uzantısı olmadan) arayan üretmek web forms geliştiricileri için çok kolay hale getirir. Pek çok hiçbir yapılandırma gerektirir ve mevcut ASP.NET v4.0 uygulamaları ile kullanılabilir. FriendlyURLs özellik ayrıca, geliştiricilerin Masaüstü ve mobil görünüm arasında geçiş destekleyerek kullandıkları uygulamalar için mobil desteği eklemek kolaylaştırır.

Yükleme ve ASP.NET kolay URL'leri kullanma hakkında daha fazla bilgi için bkz: [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET ve Web Araçları 2012.2 sürümde olan önemli değişiklikler açıklanmaktadır.

### <a name="installation-issues"></a>Yükleme Sorunları

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Visual Studio 2012'in bozuk yükler

Bir ek SKU, Visual Studio 2012 ASP.NET ve Web Araçları 2012.2 yükleme onarım işlemi gerektirecek sonra yükleme. Aşağıdaki sırada göz önünde bulundurun:

1. Web için Visual Studio 2012 Express yükleme
2. ASP.NET ve Web Araçları 2012.2 yükleyin
3. Visual Studio 2012 Professional, Premium veya Ultimate yükleyin

2. adım, yalnızca Web Express için güncelleştirmeleri yükleme neden olur. 3. adımında yüklü ek SKU güncelleştirme içerdiğinden emin olmak için ASP.NET ve Web Araçları 2012.2 yüklü son SKU için güncelleştirmeleri yüklemeyi onarmak gerekir. Bu ayrıca, adım 1'deki SKU'ları ve 3 ters geçerlidir.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Visual Studio açık olduğunda, Microsoft ASP.NET ve Web Araçları 2012.2 yükleme

Visual Studio VS ise açık Microsoft ASP.NET ve Web Araçları 2012.2 yüklenmesi sırasında hatalı bir duruma bitirebilirsiniz. Kullanıcılar, Visual Studio'nun yüklemeye devam etmeden önce tüm örnekleri kapatmanız önerilir.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>ASP.NET ve Web Araçları 2012.2 Kurulum yüklemesinin ortasında iptal etme

ASP.NET ve Web Araçları 2012.2 iptal etme yüklemesinin ortasında Kurulum Visual Studio hatalı durumda bırakır. Bu sorun izleme adımları adresi için: 

- Program Kaldır'ı eklemek için Git
- Microsoft ASP.NET ve Web Araçları 2012.2, varsa kaldırın.
- Microsoft ASP.NET ve Web Araçları 2012.2 yeniden yükleme

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>ASP.NET ve Web Araçları 2012.2 ASP.NET MVC 4 kaldırıldıktan sonra şablonları ve Razor v2 Web sitesi şablonları eksik

ASP.NET ve Web Araçları 2012.2 kaldırma da tüm ASP.NET MVC 4 ve Razor v2 Web sitesi şablonları Visual Studio 2012'den kaldırılır.

ASP.NET MVC 4 ve Razor v2 Web sitesi şablonları yeniden yüklemek için Visual Studio 2012 yüklemenizi onarmak için geçici bir çözüm değildir.

### <a name="tooling-issues"></a>Araç sorunları

#### <a name="nuget-error-reported-during-project-creation"></a>NuGet hata proje oluşturma sırasında bildirildi

ASP.NET ve Web Araçları 2012.2 yükledikten sonra aşağıdaki hata bir MVC 4 proje oluşturulurken görebilirsiniz

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

ASP.NET ve Web Araçları 2012.2 NuGet 2.1 gelir ve Visual Studio 2012'de uzantısını yükseltir. Bazı durumlarda, doğru VSIX güncelleştirmek VSIX yükleyici başarısız olur. Aşağıdaki adımlar bu sorunu gidermek için izin verir:

1. Visual Studio 2012 yönetici olarak başlatın
2. Git Araçları -&gt;Uzantılar ve güncelleştirmeler ve NuGet kaldırın.
3. Visual Studio’yu kapatın
4. ASP.NET ve Web Araçları 2012.2 yükleme klasörüne gidin:

    1. Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Web için Visual Studio 2012 Express için: **Web için Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012**
5. NuGet yeniden NuGet.Tools.vsix çift tıklayın

### <a name="web-api-issues"></a>Web API sorunları

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Ayrıştırma sorunları $filter ve DateTime değişmez değerleri

OData URI ayrıştırıcısı kısmi datetime değişmez değerler doğru ayrıştırma başarısız olur. Örneğin, $filter başlangıç eq datetime'2012 =-12-31T12:00' düzgün ayrıştırmak başarısız olur. Geçici bir çözüm tam değişmez değer $filter kullanmaktır başlangıç eq datetime'2012 =-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData büyük küçük harf duyarsız özellik adlarını desteklemiyor.

OData büyük küçük harf duyarsız özellik adları OData sorgularını ve odata yolunu desteklemiyor. İş öğeleri bakın:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Kullanıcıların farklı büyük/küçük harf javascript istemci tarafı ve sunucu tarafı varsa, bunlar büyük olasılıkla bu sorunla karşılaşır. Bu sorunu odata Protokolü tasarım gereğidir. Ancak, çok sayıda kullanıcı, bu sorunu bildirir. Bunu çözmek için kullanıcıların kendi URL durumlarda düzeltmek gerekmez.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Varsayılan OData yönlendirme kuralları POST/PUT gezinti özelliği desteklemiyor.

Varsayılan OData yönlendirme kuralları POST/PUT gezinti özelliği desteklemiyor. İş öğesi bkz [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Bu varsayılan kuralları yaygın olarak kullanılan kural eksik.

Bunu çözmek için kullanıcılar desteklemek için yeni yönlendirme kuralı yayması gerekir.

### <a name="facebook-template-issues"></a>Facebook şablon sorunları

#### <a name="facebook-application-template-only-works-using-net-45"></a>Facebook uygulama şablonu, yalnızca .NET 4.5 kullanarak çalışır

ASP.NET MVC 4'te Facebook uygulama şablonu görmek için yeni proje iletişim kutusu framework açılır listesinde, .NET 4.5 seçmelisiniz.

#### <a name="real-time-update-controller"></a>Gerçek zamanlı güncelleştirme denetleyicisi

Facebook uygulaması şablonu kolayca tanır Facebook gerçek zamanlı güncelleştirmelerini işlemek için bir Web API denetleyicisi oluşturun. Geliştirme makinenizde NAT'nin arkasında ise, denetleyiciniz daha fazla ağ yapılandırması çalışmayabilir. Ayrıntılar için buraya bakın: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Sorgu dizesi değerlerini Facebook OAuth parametrelerle çakışıyor

Aşağıdaki alanları Facebook OAuth iletişim kutusu çağrısı ile çakışan URL yedekleyin. Kendi sorgu dizesi değerlerini aşağıdaki adlarla eklemeyin: kodu, hata, hata\_açıklama, hata\_nedeni.

#### <a name="using-page-inspector-with-facebook-template"></a>Sayfa denetçisi Facebook şablonu ile kullanma

Facebook uygulama hata ayıklama sırasında Visual Studio 2012'de sayfa denetçisi özelliği kullanamaz. Sayfa denetçisi IFRAMES şu anda desteklemiyor.

### <a name="single-page-application-template-issues"></a>Tek sayfalı uygulama şablonu sorunları

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Varsayılan MVC SPA projesi, yeni todo öğesini Düzenle çalıştırırken 1.9/Boşaltılan 2.2.1 güncelleştirme girin JQuery ile odak olayı düzgün şekilde işlenmez.

JQuery 1.9/Boşaltılan ile 2.2.1 güncelleştirmek, varsayılan MVC SPA proje çalıştırırken, yeni todo öğesini Düzenle girin artık odak geri yeni Yapılacaklar öğesi düzenleme kutusuna için yapılacaklar listesi ve yeni todo öğesini girdikten sonra.

Geçici çözüm başvuru [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)ve aşağıdaki örnek kod benzer düzeltme yapın:

File todo.model.js  
 todolist(Data) işlev, ekleme aşağıdaki:  
 **self.isSelected = ko.observable(false);**

todoList.prototype.addTodo işlev, aşağıdaki blacked metni ekleyin:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Index.cshtml dosya, aşağıdaki blacked metni ekleyin:  
 &lt;Form data-bind =&quot;gönderin: addTodo&quot;&gt;  
 &lt;input class=&quot;addTodo&quot; type=&quot;text&quot; data-bind=&quot;value: newTodoTitle, placeholder: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: { blur: addTodo }&quot; /&gt;  
 &lt;/ Form&gt;
