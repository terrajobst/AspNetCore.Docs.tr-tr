---
title: ASP.NET Core temelleri
author: rick-anderson
description: ASP.NET Core uygulamalar oluşturmaya yönelik temel kavramları öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 3fbfc7c4c0d5e568339bc00a7cbe84a3932acf1f
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146361"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core temelleri

Bu makale, ASP.NET Core uygulamaları geliştirmeyi anlamak için önemli konulara genel bakış sağlar.

## <a name="the-startup-class"></a>Başlangıç sınıfı

`Startup` sınıfı şu konumda:

* Uygulamanın gerektirdiği hizmetler yapılandırıldı.
* İstek işleme işlem hattı tanımlandı.

*Hizmetler* , uygulama tarafından kullanılan bileşenlerdir. Örneğin, bir günlük bileşeni bir hizmettir. Hizmetleri yapılandırmak (veya *kaydettirmek*) için kod `Startup.ConfigureServices` yöntemine eklenir.

İstek işleme işlem hattı, bir dizi *Ara yazılım* bileşeni olarak oluşur. Örneğin, bir ara yazılım statik dosyalar için istekleri işleyebilir veya HTTP isteklerini HTTPS 'ye yeniden yönlendirebilir. Her bir ara yazılım bir `HttpContext` zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır ya da isteği sonlandırır. İstek işleme işlem hattını yapılandırma kodu `Startup.Configure` yöntemine eklenir.

Örnek bir `Startup` sınıfı aşağıda verilmiştir:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

## <a name="dependency-injection-services"></a>Bağımlılık ekleme (hizmetler)

ASP.NET Core, yapılandırılmış hizmetlerin bir uygulamanın sınıfları tarafından kullanılabilmesini sağlayan yerleşik bir bağımlılık ekleme (dı) çerçevesine sahiptir. Bir sınıftaki hizmetin bir örneğini almanın bir yolu, gerekli türde bir parametreye sahip bir Oluşturucu oluşturmaktır. Parametresi hizmet türü veya arabirim olabilir. Dı sistemi, çalışma zamanında hizmeti sağlar.

İşte bir Entity Framework Core bağlam nesnesi almak için DI kullanan bir sınıf. Vurgulanan çizgi bir Oluşturucu Ekleme örneğidir:

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

Dı yerleşik olarak kullanılıyorsa, isterseniz Control (IOC) kapsayıcısının bir üçüncü taraf Inversion öğesini eklemenize olanak sağlamak üzere tasarlanmıştır.

Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Ara yazılım

İstek işleme işlem hattı, bir dizi ara yazılım bileşeni olarak oluşur. Her bileşen bir `HttpContext` zaman uyumsuz işlemler gerçekleştirir ve sonra işlem hattında sonraki ara yazılımı çağırır ya da isteği sonlandırır.

Kurala göre, `Startup.Configure` yönteminde `Use...` uzantısı yöntemini çağırarak işlem hattına bir ara yazılım bileşeni eklenir. Örneğin, statik dosyaların işlenmesini etkinleştirmek için `UseStaticFiles`çağırın.

Aşağıdaki örnekteki vurgulanan kod, istek işleme işlem hattını yapılandırır:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

ASP.NET Core, zengin bir yerleşik ara yazılım kümesi içerir ve özel ara yazılım yazabilirsiniz.

Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

## <a name="host"></a>Konak

ASP.NET Core bir uygulama, başlangıçta bir *konak* oluşturur. Ana bilgisayar, uygulamanın tüm kaynaklarını kapsülleyen bir nesnedir, örneğin:

* Bir HTTP sunucusu uygulama
* Ara yazılım bileşenleri
* Günlüğe Kaydetme
* DI
* Yapılandırma

Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.

::: moniker range=">= aspnetcore-3.0"

İki konak mevcuttur: genel ana bilgisayar ve Web ana bilgisayarı. Genel ana bilgisayar önerilir ve Web konağı yalnızca geriye dönük uyumluluk için kullanılabilir.

Bir konak oluşturma kodu `Program.Main`:

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

`CreateDefaultBuilder` ve `ConfigureWebHostDefaults` yöntemleri, aşağıdaki gibi yaygın olarak kullanılan seçeneklerle bir konak yapılandırır:

* Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.
* *AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.
* Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.

Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

İki konak mevcuttur: Web Konağı ve genel ana bilgisayar. ASP.NET Core 2. x içinde, genel konak yalnızca Web dışı senaryolar içindir.

Bir konak oluşturma kodu `Program.Main`:

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

`CreateDefaultBuilder` yöntemi, aşağıdaki gibi yaygın olarak kullanılan seçeneklere sahip bir konak yapılandırır:

* Web sunucusu olarak [Kestrel](#servers) kullanın ve IIS tümleştirmesini etkinleştirin.
* *AppSettings. JSON*, appSettings 'ten yapılandırma yükleyin *. { Ortam adı}. JSON*, ortam değişkenleri, komut satırı bağımsız değişkenleri ve diğer yapılandırma kaynakları.
* Günlüğe kaydetme çıkışını konsola ve hata ayıklama sağlayıcılarına gönderin.

Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.

::: moniker-end

### <a name="non-web-scenarios"></a>Web dışı senaryolar

Genel ana bilgisayar, diğer uygulama türlerinin günlüğe kaydetme, bağımlılık ekleme (dı), yapılandırma ve uygulama ömür yönetimi gibi çapraz kesme çerçevesi uzantıları kullanmasına izin verir. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve <xref:fundamentals/host/hosted-services>.

## <a name="servers"></a>Sunucular

Bir ASP.NET Core uygulaması HTTP isteklerini dinlemek için HTTP sunucu uygulamasını kullanır. Sunucu, bir `HttpContext`oluşan [istek özellikleri](xref:fundamentals/request-features) kümesi olarak uygulamaya istekleri ister.

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

## <a name="environments"></a>Ortamlar

*Geliştirme*, *hazırlık*ve *üretim*gibi yürütme ortamları, ASP.NET Core birinci sınıf kavramlardır. `ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlayarak bir uygulamanın üzerinde çalıştığı ortamı belirtebilirsiniz. ASP.NET Core, uygulamanın başlangıcında bu ortam değişkenini okur ve değeri bir `IHostingEnvironment` uygulamasında depolar. Ortam nesnesi, uygulama tarafından her yerde DI aracılığıyla kullanılabilir.

`Startup` sınıfından aşağıdaki örnek kod, uygulamayı yalnızca geliştirmede çalıştırıldığında ayrıntılı hata bilgilerini sunacak şekilde yapılandırır:

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

## <a name="logging"></a>Günlüğe Kaydetme

ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler. Kullanılabilir sağlayıcılar şunları içerir:

* Konsolu
* Hata ayıklama
* Windows üzerinde olay Izleme
* Windows olay günlüğü
* TraceSource
* Azure App Service
* Azure Application Insights

Dı ve çağrı günlüğü yöntemlerinden `ILogger` nesne alarak uygulamanın kodundaki her yerden günlükleri yazın.

Oluşturucu Ekleme ve günlük yöntemi çağrılarını vurgulanmış bir `ILogger` nesnesi kullanan örnek kod aşağıda verilmiştir.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

`ILogger` arabirimi, günlük sağlayıcısına istediğiniz sayıda alanı geçirmenize olanak sağlar. Alanlar genellikle bir ileti dizesi oluşturmak için kullanılır, ancak sağlayıcı bunları bir veri deposuna ayrı alanlar olarak da gönderebilir. Bu özellik, günlük sağlayıcılarının [yapılandırılmış günlük olarak da bilinen anlam günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulamasına olanak tanır.

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

`IHttpClientFactory` bir uygulama `HttpClient` örnekleri oluşturmak için kullanılabilir. Fabrika:

* , Mantıksal `HttpClient` örneklerinin adlandırılması ve yapılandırılması için merkezi bir konum sağlar. Örneğin, *GitHub istemcisi kayıtlı ve GitHub 'a* erişebilecek şekilde yapılandırılabilir. Varsayılan istemci, diğer amaçlar için kaydedilebilir.
* , Bir giden istek ara yazılım işlem hattı oluşturmak için birden çok temsilci seçme işleyicisinin kaydını ve zincirlemeyi destekler. Bu düzen, ASP.NET Core gelen ara yazılım ardışık düzenine benzer. Bu model, önbelleğe alma, hata işleme, serileştirme ve günlüğe kaydetme dahil olmak üzere HTTP istekleri etrafında çapraz kesme sorunlarını yönetmek için bir mekanizma sağlar.
* Geçici hata işleme için popüler bir üçüncü taraf kitaplığı olan *Polly*ile tümleşir.
* `HttpClient` yaşam sürelerini el ile yönetirken gerçekleşen yaygın DNS sorunlarından kaçınmak için temel `HttpClientMessageHandler` örneklerinin biriktirmesini ve ömrünü yönetir.
* Fabrika tarafından oluşturulan istemcilerle gönderilen tüm istekler için yapılandırılabilir bir günlük deneyimi (`ILogger`aracılığıyla) ekler.

Daha fazla bilgi için bkz. <xref:fundamentals/http-requests>.

## <a name="content-root"></a>İçerik kökü

İçerik kökü, için temel yoldur:

* Uygulamayı barındıran yürütülebilir dosya ( *. exe*).
* Uygulamayı oluşturan derlenmiş derlemeler ( *. dll*).
* Uygulama tarafından kullanılan kod olmayan içerik dosyaları, örneğin:
  * Razor dosyaları ( *. cshtml*, *. Razor*)
  * Yapılandırma dosyaları ( *. JSON*, *. xml*)
  * Veri dosyaları ( *. db*)
* [Web kökü](#web-root), genellikle yayınlanan *Wwwroot* klasörü.

Geliştirme sırasında:

* İçerik kökü, projenin kök dizinini varsayılan olarak belirler.
* Projenin kök dizini şunu oluşturmak için kullanılır:
  * Uygulamanın, projenin kök dizinindeki kod olmayan içerik dosyalarının yolu.
  * [Web kökü](#web-root), genellikle projenin kök dizinindeki *Wwwroot* klasörü.

::: moniker range=">= aspnetcore-3.0"

[Konak oluşturulurken](#host)alternatif bir içerik kök yolu belirtilebilir. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#contentrootpath>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Konak oluşturulurken](#host)alternatif bir içerik kök yolu belirtilebilir. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#content-root>.

::: moniker-end

## <a name="web-root"></a>Web kökü

Web kökü, genel, kod olmayan statik kaynak dosyalarının temel yoludur, örneğin:

* Stil sayfaları ( *. css*)
* JavaScript ( *. js*)
* Görüntüler ( *. png*, *. jpg*)

Statik dosyalar yalnızca Web kök dizininden (ve alt dizinlerde) varsayılan olarak sunulur.

::: moniker range=">= aspnetcore-3.0"

Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir Web kökü belirtilebilir. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#webroot>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Web kök yolu varsayılan olarak *{Content root}/Wwwroot*olarak belirlenir, ancak [konak oluşturulurken](#host)farklı bir Web kökü belirtilebilir. Daha fazla bilgi için bkz. [Web root](xref:fundamentals/host/web-host#web-root).

::: moniker-end

Proje dosyasındaki [\<içerik > proje öğesi](/visualstudio/msbuild/common-msbuild-project-items#content) ile *Wwwroot* 'da dosya yayımlamayı önleyin. Aşağıdaki örnek, *Wwwroot/yerel* dizin ve alt dizinlerde içerik yayımlamayı engeller:

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

::: moniker range=">= aspnetcore-3.0"

Statik kimlik varlıklarının Web köküne yayımlanmasını engellemek için bkz. <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.

::: moniker-end

Razor ( *. cshtml*) dosyalarında, tilde işareti (`~/`) Web köküne işaret eder. `~/` başlayan bir yol, *sanal yol*olarak adlandırılır.

Daha fazla bilgi için bkz. <xref:fundamentals/static-files>.
