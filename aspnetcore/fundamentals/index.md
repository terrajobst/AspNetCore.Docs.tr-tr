---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamalar oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: fundamentals/index
ms.openlocfilehash: cff2afd62ed60648dc689d408dde56ecda18c261
ms.sourcegitcommit: 2d4c1732c4866ed26b83da35f7bc2ad021a9c701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70815646"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core temelleri

Bu makale, ASP.NET Core uygulamaları geliştirmeyi anlamak için önemli konulara genel bakış sağlar.

## <a name="the-startup-class"></a>Başlangıç sınıfı

`Startup` Sınıfı şu konumda:

* Uygulamanın gerektirdiği hizmetler yapılandırıldı.
* İstek işleme işlem hattı tanımlandı.

*Hizmetler* , uygulama tarafından kullanılan bileşenlerdir. Örneğin, bir günlük bileşeni bir hizmettir. Hizmetleri yapılandırmak (veya *kaydettirmek*) için kod `Startup.ConfigureServices` yöntemine eklenmiştir.

İstek işleme işlem hattı, bir dizi *Ara yazılım* bileşeni olarak oluşur. Örneğin, bir ara yazılım statik dosyalar için istekleri işleyebilir veya HTTP isteklerini HTTPS 'ye yeniden yönlendirebilir. Her bir ara yazılım bir `HttpContext` üzerinde zaman uyumsuz işlemler gerçekleştirir ve ardından işlem hattında sonraki ara yazılımı çağırır veya isteği sonlandırır. İstek işleme işlem hattını yapılandırma kodu `Startup.Configure` yöntemine eklenir.

Örnek `Startup` bir sınıf aşağıda verilmiştir:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

## <a name="dependency-injection-services"></a>Bağımlılık ekleme (hizmetler)

ASP.NET Core, yapılandırılmış hizmetlerin bir uygulamanın sınıfları tarafından kullanılabilmesini sağlayan yerleşik bir bağımlılık ekleme (dı) çerçevesine sahiptir. Bir sınıftaki hizmetin bir örneğini almanın bir yolu, gerekli türde bir parametreye sahip bir Oluşturucu oluşturmaktır. Parametresi hizmet türü veya arabirim olabilir. Dı sistemi, çalışma zamanında hizmeti sağlar.

İşte bir Entity Framework Core bağlam nesnesi almak için DI kullanan bir sınıf. Vurgulanan çizgi bir Oluşturucu Ekleme örneğidir:

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

Dı yerleşik olarak kullanılıyorsa, isterseniz Control (IOC) kapsayıcısının bir üçüncü taraf Inversion öğesini eklemenize olanak sağlamak üzere tasarlanmıştır.

Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Ara yazılım

İstek işleme işlem hattı, bir dizi ara yazılım bileşeni olarak oluşur. Her bileşen bir `HttpContext` üzerinde zaman uyumsuz işlemler gerçekleştirir ve ardından işlem hattında sonraki ara yazılımı çağırır veya isteği sonlandırır.

Kurala göre, `Use...` `Startup.Configure` yöntemi içindeki genişletme yöntemi çağrılarak işlem hattına bir ara yazılım bileşeni eklenir. Örneğin, statik dosyaları işlemeyi etkinleştirmek için çağırın `UseStaticFiles`.

Aşağıdaki örnekteki vurgulanan kod, istek işleme işlem hattını yapılandırır:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

ASP.NET Core, zengin bir yerleşik ara yazılım kümesi içerir ve özel ara yazılım yazabilirsiniz.

Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

## <a name="host"></a>Ana bilgisayar

ASP.NET Core bir uygulama, başlangıçta bir *konak* oluşturur. Ana bilgisayar, uygulamanın tüm kaynaklarını kapsülleyen bir nesnedir, örneğin:

* Bir HTTP sunucusu uygulama
* Ara yazılım bileşenleri
* Günlüğe Kaydetme
* IÇERIK
* Yapılandırma

Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.

::: moniker range=">= aspnetcore-3.0"

İki konak mevcuttur: genel ana bilgisayar ve Web ana bilgisayarı. Genel ana bilgisayar önerilir ve Web konağı yalnızca geriye dönük uyumluluk için kullanılabilir.

Bir konak `Program.Main`oluşturma kodu:

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

`CreateDefaultBuilder` Ve`ConfigureWebHostDefaults` yöntemleri, aşağıdaki gibi yaygın olarak kullanılan seçeneklerle bir ana bilgisayar yapılandırır:

* Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.
* *AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.
* Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.

Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

İki konak mevcuttur: Web Konağı ve genel ana bilgisayar. ASP.NET Core 2. x içinde, genel konak yalnızca Web dışı senaryolar içindir.

Bir konak `Program.Main`oluşturma kodu:

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

`CreateDefaultBuilder` Yöntemi, aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir ana bilgisayar yapılandırır:

* Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.
* *AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.
* Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.

Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.

::: moniker-end

### <a name="non-web-scenarios"></a>Web dışı senaryolar

Genel ana bilgisayar, diğer uygulama türlerinin günlüğe kaydetme, bağımlılık ekleme (dı), yapılandırma ve uygulama ömür yönetimi gibi çapraz kesme çerçevesi uzantıları kullanmasına izin verir. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/hosted-services>.

## <a name="servers"></a>Sunucular

Bir ASP.NET Core uygulaması HTTP isteklerini dinlemek için HTTP sunucu uygulamasını kullanır. Sunucu, bir `HttpContext`öğesine oluşturulan [istek özellikleri](xref:fundamentals/request-features) kümesi olarak uygulamaya istekleri ister.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:

* *Kestrel* , platformlar arası bir Web sunucusudur. Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır. ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.
* *IIS HTTP sunucusu* , IIS kullanan bir Windows sunucusudur. Bu sunucu ile, ASP.NET Core uygulaması ve IIS aynı işlemde çalışır.
* *Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar. ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir. Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar. ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir. Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core aşağıdaki sunucu uygulamalarını sağlar:

* *Kestrel* , platformlar arası bir Web sunucusudur. Kestrel, genellikle [IIS](https://www.iis.net/)kullanılarak ters bir ara sunucu yapılandırmasında çalıştırılır. ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir.
* *Http. sys* , IIS ile kullanılmayan bir Windows sunucusudur.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar. ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir. Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core, *Kestrel* platformlar arası sunucu uygulamasını sağlar. ASP.NET Core 2,0 veya üzeri sürümlerde, Kestrel doğrudan Internet 'e açık olan bir genel kullanıma yönelik uç sunucu olarak çalıştırılabilir. Kestrel, genellikle [NGINX](https://nginx.org) veya [Apache](https://httpd.apache.org/)ile ters proxy yapılandırmasında çalıştırılır.

---

::: moniker-end

Daha fazla bilgi için bkz. <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Yapılandırma

ASP.NET Core, ayarları sıralı bir yapılandırma sağlayıcıları kümesinden ad-değer çiftleri olarak alan bir yapılandırma çerçevesi sağlar. *. JSON* dosyaları, *. xml* dosyaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri gibi çeşitli kaynaklar için yerleşik yapılandırma sağlayıcıları vardır. Ayrıca, özel yapılandırma sağlayıcıları da yazabilirsiniz.

Örneğin, yapılandırmanın *appSettings. JSON* ve ortam değişkenlerinden geldiğini belirtebilirsiniz. Ardından, *ConnectionString* değeri istendiğinde, Framework ilk olarak *appSettings. JSON* dosyasına bakar. Değer aynı zamanda bir ortam değişkeninde bulunursa, ortam değişkeninin değeri öncelikli olur.

Parolalar gibi gizli yapılandırma verilerini yönetmek için ASP.NET Core bir [gizli dizi Yöneticisi aracı](xref:security/app-secrets)sağlar. Üretim gizli dizileri için [Azure Key Vault](xref:security/key-vault-configuration)önerilir.

Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

## <a name="options"></a>Seçenekler

Mümkün olduğunda yapılandırma değerlerini depolamak ve almak için *Seçenekler deseninin* ASP.NET Core. Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.

Örneğin, aşağıdaki kod WebSockets seçeneklerini ayarlar:

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.

## <a name="environments"></a>Lý

*Geliştirme*, *hazırlık*ve *üretim*gibi yürütme ortamları, ASP.NET Core birinci sınıf kavramlardır. `ASPNETCORE_ENVIRONMENT` Ortam değişkenini ayarlayarak bir uygulamanın çalıştığı ortamı belirtebilirsiniz. ASP.NET Core, uygulamanın başlangıcında bu ortam değişkenini okur ve değeri bir `IHostingEnvironment` uygulamada depolar. Ortam nesnesi, uygulama tarafından her yerde DI aracılığıyla kullanılabilir.

`Startup` Sınıfından aşağıdaki örnek kod, uygulamayı yalnızca geliştirmede çalıştırıldığında ayrıntılı hata bilgilerini sunacak şekilde yapılandırır:

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

## <a name="logging"></a>Günlüğe Kaydetme

ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler. Kullanılabilir sağlayıcılar şunları içerir:

* Konsol
* Hata ayıklama
* Windows üzerinde olay Izleme
* Windows olay günlüğü
* TraceSource
* Azure uygulama hizmeti
* Azure Application Insights

Dı ve çağrı günlüğü yöntemlerinden bir `ILogger` nesne alarak uygulamanın kodundaki her yerden günlükleri yazın.

Oluşturucu Ekleme ve günlük yöntemi çağrılarını vurgulanmış `ILogger` bir nesne kullanan örnek kod aşağıda verilmiştir.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

`ILogger` Arabirim, günlük sağlayıcısına istediğiniz sayıda alanı geçirmenize olanak sağlar. Alanlar genellikle bir ileti dizesi oluşturmak için kullanılır, ancak sağlayıcı bunları bir veri deposuna ayrı alanlar olarak da gönderebilir. Bu özellik, günlük sağlayıcılarının [yapılandırılmış günlük olarak da bilinen anlam günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulamasına olanak tanır.

Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

## <a name="routing"></a>Yönlendirme

*Yol* , bir işleyiciye EŞLENMIŞ bir URL örüncidir. İşleyici genellikle bir Razor sayfası, MVC denetleyicisindeki bir eylem yöntemi veya bir ara yazılım olur. ASP.NET Core yönlendirme, uygulamanız tarafından kullanılan URL 'Ler üzerinde denetim sağlar.

Daha fazla bilgi için bkz. <xref:fundamentals/routing>.

## <a name="error-handling"></a>Hata işleme

ASP.NET Core, hataları işlemeye yönelik yerleşik özellikler içerir, örneğin:

* Geliştirici özel durum sayfası
* Özel hata sayfaları
* Statik durum kodu sayfaları
* Başlatma özel durum işleme

Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.

## <a name="make-http-requests"></a>HTTP isteğinde bulunma

Uygulamasının bir uygulamasına `IHttpClientFactory` örnek oluşturmak `HttpClient` için kullanılabilir. Fabrika:

* , Mantıksal `HttpClient` örnekleri adlandırmak ve yapılandırmak için merkezi bir konum sağlar. Örneğin, *GitHub istemcisi kayıtlı ve GitHub 'a* erişebilecek şekilde yapılandırılabilir. Varsayılan istemci, diğer amaçlar için kaydedilebilir.
* , Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok temsilci seçme işleyicisinin kaydını ve zincirlemeyi destekler. Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer. Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.
* Geçici hata işleme için popüler bir üçüncü taraf kitaplığı olan *Polly*ile tümleşir.
* Yaşam sürelerini el ile yönetirken `HttpClientMessageHandler` `HttpClient` gerçekleşen yaygın DNS sorunlarından kaçınmak için temeldeki örneklerin biriktirmesini ve ömrünü yönetir.
* Fabrika tarafından oluşturulan istemciler aracılığıyla gönderilen tüm `ILogger`istekler için yapılandırılabilir bir günlüğe kaydetme deneyimi ekler (aracılığıyla).

Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.

## <a name="content-root"></a>İçerik kökü

İçerik kökü, uygulama tarafından kullanılan Razor dosyaları gibi özel içeriklerin temel yoludur. Varsayılan olarak, içerik kökü, uygulamayı barındıran yürütülebilir dosyanın temel yoludur. [Konak oluşturulurken](#host)alternatif bir konum belirtilebilir.

::: moniker range=">= aspnetcore-3.0"

Daha fazla bilgi için bkz. [içerik kökü](xref:fundamentals/host/generic-host#content-root).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Daha fazla bilgi için bkz. [içerik kökü](xref:fundamentals/host/web-host#content-root).

::: moniker-end

## <a name="web-root"></a>Web kökü

Web kökü ( *Webroot*olarak da bilinir), genel, STATIK kaynakların CSS, JavaScript ve resim dosyaları gibi temel yoludur. Statik dosyalar ara yazılımı, dosyaları yalnızca Web kök dizininden (ve alt dizinlerden) varsayılan olarak sunar. Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir konum belirtilebilir.

::: moniker range=">= aspnetcore-3.0"

Daha fazla bilgi için bkz. [Webroot](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#webroot)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Daha fazla bilgi için bkz. [Web root](/aspnet/core/fundamentals/host/web-host#webroot).

::: moniker-end

Razor ( *. cshtml*) dosyalarında, tilde işareti `~/` Web köküne işaret eder. İle `~/` başlayan yollar sanal yollar olarak adlandırılır.

Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.
