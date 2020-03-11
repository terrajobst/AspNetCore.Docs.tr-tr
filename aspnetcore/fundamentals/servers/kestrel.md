---
title: ASP.NET Core Web sunucusu uygulamasını Kestrel
author: rick-anderson
description: ASP.NET Core platformlar arası Web sunucusu olan Kestrel hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 8d96118800c47b2c551726342bf4cfba9671a09e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667372"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core Web sunucusu uygulamasını Kestrel

[Tom Dykstra](https://github.com/tdykstra), [Chris](https://github.com/Tratcher), ve [Stephen halter](https://twitter.com/halter73) tarafından

::: moniker range=">= aspnetcore-3.0"

Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index). Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.

Kestrel aşağıdaki senaryoları destekler:

* HTTPS
* [WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme
* NGINX 'in arkasında yüksek performans için UNIX Yuvaları
* HTTP/2 (macOS&dagger;hariç)

&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.

Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="http2-support"></a>HTTP/2 desteği

Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:

* İşletim sistemi&dagger;
  * Windows Server 2016/Windows 10 veya üzeri&Dagger;
  * OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)
* Hedef Framework: .NET Core 2,2 veya üzeri
* [Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı
* TLS 1.2 veya sonraki bir bağlantı

&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.
&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir. Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır. TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.

Bir HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.

HTTP/2 varsayılan olarak devre dışıdır. Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Ters ara sunucu ile Kestrel ne zaman kullanılır?

Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir. Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.

Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

Ters Proxy yapılandırmasında kullanılan Kestrel:

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.

Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez. Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel `Host` ' den bağımsız olarak bu bağlantı noktası için tüm trafiği işler. Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.

Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.

Ters proxy:

* , Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.
* Ek bir yapılandırma ve savunma katmanı sağlayın.
* , Mevcut altyapıyla daha iyi tümleşebilir.
* Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın. Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.

> [!WARNING]
> Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.

## <a name="kestrel-in-aspnet-core-apps"></a>ASP.NET Core uygulamalarda Kestrel

ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır. *Program.cs*içinde <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> yöntemi <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>çağırır:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

Konak oluşturma hakkında daha fazla bilgi için <xref:fundamentals/host/generic-host#set-up-a-host>*ana bilgisayar* ve *Varsayılan Oluşturucu ayarlarını* ayarlama bölümüne bakın.

`ConfigureWebHostDefaults`çağrıldıktan sonra ek yapılandırma sağlamak için `ConfigureKestrel`kullanın:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

## <a name="kestrel-options"></a>Kestrel seçenekleri

Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın. `Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.

Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

Bu makalenin ilerleyen kısımlarında gösterilen örneklerde, Kestrel seçenekleri C# kodda yapılandırılır. Ayrıca, Kestrel seçenekleri bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir. Örneğin, [dosya yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#file-configuration-provider) bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    },
    "DisableStringReuse": true
  }
}
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> ve [uç nokta yapılandırması](#endpoint-configuration) yapılandırma sağlayıcılarından yapılandırılabilir. Kod içinde C# kalan Kestrel yapılandırması yapılandırılmalıdır.

Aşağıdaki yaklaşımlardan **birini** kullanın:

* `Startup.ConfigureServices`'de Kestrel yapılandırma:

  1. `Startup` sınıfına bir `IConfiguration` örneği ekleyin. Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.
  2. `Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin:

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* Ana bilgisayarı oluştururken Kestrel yapılandırma:

  *Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          });
  ```

Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.

### <a name="keep-alive-timeout"></a>Etkin tut zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar. Varsayılan olarak 2 dakikadır.

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a>İstemci bağlantıları üst sınırı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır. Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).

### <a name="maximum-request-body-size"></a>En büyük istek gövdesi boyutu

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.

ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur. `MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.

Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.

### <a name="minimum-request-body-data-rate"></a>En az istek gövdesi veri hızı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler. Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez. Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.

Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.

Yanıt için bir minimum oran da geçerlidir. İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında `RequestBody` veya `Response` sahip olma dışında aynıdır.

*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

Önceki örnekte başvurulan <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> HTTP/2 istekleri için `HttpContext.Features` mevcut değildir, çünkü bir istek temelli olarak hız sınırlarını değiştirmek, protokolün istek çoğullama desteği nedeniyle HTTP/2 için genel olarak desteklenmez. Ancak, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> HTTP/2 istekleri için `HttpContext.Features` olmaya devam eder, çünkü okuma hızı sınırı, bir HTTP/2 isteği için bile `IHttpMinRequestBodyDataRateFeature.MinDataRate` `null` olarak ayarlanarak tamamen istek temelli olarak *devre dışı* bırakılabilir. `IHttpMinRequestBodyDataRateFeature.MinDataRate` okumaya veya `null` dışında bir değere ayarlamaya çalışırken, bir HTTP/2 isteği verilen bir `NotSupportedException` atılmaya neden olur.

`KestrelServerOptions.Limits` aracılığıyla yapılandırılan sunucu genelindeki hız sınırları hem HTTP/1. x hem de HTTP/2 bağlantıları için geçerlidir.

### <a name="request-headers-timeout"></a>İstek üst bilgileri zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar. Varsayılan değer 30 saniyedir.

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a>Bağlantı başına en fazla akış

`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar. Fazlalık akışlar reddedildi.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

Varsayılan değer 100’dür.

### <a name="header-table-size"></a>Üst bilgi tablosu boyutu

HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar. `Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar. Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

Varsayılan değer 4096 ' dir.

### <a name="maximum-frame-size"></a>En büyük çerçeve boyutu

`Http2.MaxFrameSize`, sunucu tarafından alınan veya gönderilen HTTP/2 bağlantı çerçevesi yükünün izin verilen en büyük boyutunu belirtir. Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

Varsayılan değer 2 ^ 14 ' dir (16.384).

### <a name="maximum-request-header-size"></a>En fazla istek üst bilgi boyutu

`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir. Bu sınır, sıkıştırılmış ve sıkıştırılmamış temsillerinde hem ad hem de değer için geçerlidir. Değer sıfırdan büyük (0) olmalıdır.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

Varsayılan değer 8.192 ' dir.

### <a name="initial-connection-window-size"></a>İlk bağlantı pencere boyutu

`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir. İstekler aynı zamanda `Http2.InitialStreamWindowSize`ile sınırlıdır. Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

Varsayılan değer 128 KB 'tır (131.072).

### <a name="initial-stream-window-size"></a>İlk akış pencere boyutu

`Http2.InitialStreamWindowSize`, istek (Stream) başına bir seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir. İstekler aynı zamanda `Http2.InitialConnectionWindowSize`ile sınırlıdır. Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

Varsayılan değer 96 KB 'tır (98.304).

### <a name="synchronous-io"></a>Zaman uyumlu GÇ

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler. Varsayılan değer: `false`.

> [!WARNING]
> Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur. Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.

Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a>Uç nokta yapılandırması

Varsayılan olarak, ASP.NET Core bağlar:

* `http://localhost:5000`
* `https://localhost:5001` (yerel bir geliştirme sertifikası mevcut olduğunda)

Kullanarak URL 'Leri belirtin:

* ortam değişkeni `ASPNETCORE_URLS`.
* `--urls` komut satırı bağımsız değişkeni.
* `urls` ana bilgisayar yapılandırma anahtarı.
* `UseUrls` genişletme yöntemi.

Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS). Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).

Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.

Geliştirme sertifikası oluşturuldu:

* [.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.
* [Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.

Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.

Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.

Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerini çağırın.

`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).

`KestrelServerOptions` yapılandırması:

### <a name="configureendpointdefaultsactionlistenoptions"></a>ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)

Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir. `ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a>ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)

Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir. `ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.

### <a name="configureiconfiguration"></a>Yapılandırma (Iconation)

Giriş olarak bir <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur. Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.

### <a name="listenoptionsusehttps"></a>ListenOptions. UseHttps

Kestrel 'i HTTPS kullanacak şekilde yapılandırın.

`ListenOptions.UseHttps` uzantıları:

* `UseHttps` &ndash; varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın. Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` parametreleri:

* `filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.
* X. 509.440 sertifika verilerine erişmek için gereken parola `password`.
* `configureOptions`, `HttpsConnectionAdapterOptions`yapılandırmak için `Action`. `ListenOptions`döndürür.
* `storeName`, sertifikanın yükleneceği sertifika deposudur.
* `subject`, sertifikanın konu adıdır.
* `allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.
* `location`, sertifikanın yükleneceği mağaza konumudur.
* `serverCertificate` X. 509.440 sertifikasıdır.

Üretimde HTTPS 'nin açıkça yapılandırılması gerekir. En azından, varsayılan bir sertifika sağlanmalıdır.

Daha sonra açıklanan desteklenen yapılandırma:

* Yapılandırma yok
* Varsayılan sertifikayı yapılandırmadan Değiştir
* Koddaki varsayılanları değiştirme

*Yapılandırma yok*

Kestrel `http://localhost:5000` ve `https://localhost:5001` dinler (varsayılan bir sertifika varsa).

<a name="configuration"></a>

*Varsayılan sertifikayı yapılandırmadan Değiştir*

Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır. Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir. Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.

Aşağıdaki *appSettings. JSON* örneğinde:

* Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.
* Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **varsayılan** veya geliştirme sertifikası > **Sertifikalar** altında tanımlanan sertifikaya geri döner.

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },
      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },
      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },
      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },
      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir. Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirlenebilir:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Şema notları:

* Uç nokta adları büyük/küçük harfe duyarlıdır. Örneğin, `HTTPS` ve `Https` geçerlidir.
* Her uç nokta için `Url` parametresi gereklidir. Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.
* Bu uç noktalar, üst düzey `Urls` yapılandırmasında tanımlananlara eklemek yerine bunların yerini alır. `Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.
* `Certificate` bölümü isteğe bağlıdır. `Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır. Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.
* `Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.
* Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.
* `options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemi ile bir `KestrelConfigurationLoader` döndürür:

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

`KestrelServerOptions.ConfigurationLoader`, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından sağlana gibi var olan yükleyicisindeki yinelenmeye devam etmek için doğrudan erişilebilir.

* Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.
* `options.Configure(context.Configuration.GetSection("{SECTION}"))` başka bir bölümle yeniden çağırarak birden çok yapılandırma yüklenebilir. `Load` önceki örneklerde açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır. Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` çağırmaz.
* `KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir. Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.

*Koddaki varsayılanları değiştirme*

`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions`için varsayılan ayarları değiştirmek üzere kullanılabilir. `ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });

    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.SslProtocols = SslProtocols.Tls12;
    });
});
```

*SNı için Kestrel desteği*

[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir. SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir. İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.

Kestrel `ServerCertificateSelector` geri çağırma aracılığıyla SNı destekler. Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.

SNı desteği şunları gerektirir:

* Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor. `net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`. Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.
* Tüm Web siteleri aynı Kestrel örneğinde çalışır. Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ListenAnyIP(5005, listenOptions =>
    {
        listenOptions.UseHttps(httpsOptions =>
        {
            var localhostCert = CertificateLoader.LoadFromStoreCert(
                "localhost", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var exampleCert = CertificateLoader.LoadFromStoreCert(
                "example.com", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var subExampleCert = CertificateLoader.LoadFromStoreCert(
                "sub.example.com", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var certs = new Dictionary<string, X509Certificate2>(
                StringComparer.OrdinalIgnoreCase);
            certs["localhost"] = localhostCert;
            certs["example.com"] = exampleCert;
            certs["sub.example.com"] = subExampleCert;

            httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
            {
                if (name != null && certs.TryGetValue(name, out var cert))
                {
                    return cert;
                }

                return exampleCert;
            };
        });
    });
});
```

### <a name="connection-logging"></a>Bağlantı günlüğü

Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın. Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur. `UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir. `UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a>TCP yuvasına bağlama

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=12-18)]

Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>olan bir uç nokta için HTTPS 'yi yapılandırır. Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>UNIX yuvasına bağlama

Aşağıdaki örnekte gösterildiği gibi NGINX ile daha iyi performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* NGINX confiuguration dosyasında, `server` > `location` > `proxy_pass` girişi `http://unix:/tmp/{KESTREL SOCKET}:/;`olarak ayarlayın. `{KESTREL SOCKET}`, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> için belirtilen yuvanın adıdır (örneğin, önceki örnekte `kestrel-test.sock`).
* Yuvanın NGINX tarafından yazılabilir olduğundan emin olun (örneğin, `chmod go+w /tmp/kestrel-test.sock`).

### <a name="port-0"></a>Bağlantı noktası 0

`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır. Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Sınırlamalar

Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* `--urls` komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* `ASPNETCORE_URLS` ortam değişkeni

Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır. Ancak, aşağıdaki sınırlamalara dikkat edin:

* HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.
* `Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.

### <a name="iis-endpoint-configuration"></a>IIS uç nokta yapılandırması

IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls`tarafından ayarlanır. Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.

### <a name="listenoptionsprotocols"></a>ListenOptions. Protocols

`Protocols` özelliği, bir bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) belirler. `HttpProtocols` numaralandırmasından `Protocols` özelliğine bir değer atayın.

| `HttpProtocols` Enum değeri | Bağlantı protokolü izin verildi |
| -------------------------- | ----------------------------- |
| `Http1`                    | Yalnızca HTTP/1.1. , TLS olmadan veya ile kullanılabilir. |
| `Http2`                    | Yalnızca HTTP/2. Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir. |
| `Http1AndHttp2`            | HTTP/1.1 ve HTTP/2. HTTP/2, istemcinin TLS [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) el sıkışmasında http/2 seçmesini gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir. |

Herhangi bir uç nokta için varsayılan `ListenOptions.Protocols` değeri `HttpProtocols.Http1AndHttp2`.

HTTP/2 için TLS kısıtlamaları:

* TLS sürüm 1,2 veya üzeri
* Yeniden anlaşma devre dışı
* Sıkıştırma devre dışı
* En az kısa ömürlü anahtar değişim boyutları:
  * Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum
  * Sınırlı alan Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit minimum
* Şifre paketi kara listede değil

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;&rbrack; `TLS-ECDHE`, P-256 eliptik eğrisi &lbrack;`FIPS186`&rbrack; varsayılan olarak desteklenir.

Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir. Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

Gerektiğinde belirli şifrelemeler için TLS el sıkışmaları için bağlantı temelinde filtre uygulamak üzere bağlantı ara yazılımı kullanın.

Aşağıdaki örnek, uygulamanın desteklemediği tüm şifre algoritmaların <xref:System.NotSupportedException> oluşturur. Alternatif olarak, [ılshandshakefeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) öğesini kabul edilebilir şifreleme paketleri listesi ile tanımlayın ve karşılaştırın.

[CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) şifre algoritması ile hiçbir şifreleme kullanılmaz.

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

Bağlantı filtrelemesi, bir <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda aracılığıyla da yapılandırılabilir:

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

Linux 'ta <xref:System.Net.Security.CipherSuitesPolicy>, TLS el sıkışmaları her bağlantı temelinde filtrelemek için kullanılabilir:

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

*Protokolü yapılandırmadan ayarla*

Kestrel yapılandırmasını yüklemek için varsayılan olarak `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.

Aşağıdaki *appSettings. JSON* örneği, tüm uç noktalar için varsayılan bağlantı protokolü olarak http/1.1 oluşturur:

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

Aşağıdaki *appSettings. JSON* örneği, belirli bir uç nokta için http/1.1 bağlantı protokolünü belirler:

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.

## <a name="transport-configuration"></a>Aktarım yapılandırması

Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>) kullanımını gerektiren projeler için:

* Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* `IWebHostBuilder`<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağırın:

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a>URL önekleri

`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.

Yalnızca HTTP URL ön ekleri geçerlidir. Kestrel, `UseUrls`kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.

* Bağlantı noktası numarası olan IPv4 adresi

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.

* Bağlantı noktası numarasına sahip IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.

* Bağlantı noktası numarası olan ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Ana bilgisayar adları, `*`ve `+`özel değildir. Geçerli bir IP adresi olarak tanınmayan bir şey veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanır. Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.

  > [!WARNING]
  > Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.

* Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  `localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır. İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz. Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.

## <a name="host-filtering"></a>Konak filtreleme

Kestrel, `http://example.com:5000`gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar. Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur. Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır. `Host` üstbilgileri doğrulanmadı.

Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın. Ana bilgisayar filtreleme ara yazılımı, ASP.NET Core uygulamaları için örtük olarak sunulan [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır. Ara yazılım, <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından eklenir:

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır. Ara yazılımı etkinleştirmek için *appSettings. json*/appSettings içinde bir `AllowedHosts` anahtarı tanımlayın *.\<environmentname >. JSON*. Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:

*appSettings. JSON*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği de vardır. İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir. Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur. Kestrel genel kullanıma açık bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara yazılımı ile `AllowedHosts` ayarlama uygundur.
>
> Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index). Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.

Kestrel aşağıdaki senaryoları destekler:

* HTTPS
* [WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme
* NGINX 'in arkasında yüksek performans için UNIX Yuvaları
* HTTP/2 (macOS&dagger;hariç)

&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.

Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="http2-support"></a>HTTP/2 desteği

Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:

* İşletim sistemi&dagger;
  * Windows Server 2016/Windows 10 veya üzeri&Dagger;
  * OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)
* Hedef Framework: .NET Core 2,2 veya üzeri
* [Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı
* TLS 1.2 veya sonraki bir bağlantı

&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.
&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir. Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır. TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.

Bir HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.

HTTP/2 varsayılan olarak devre dışıdır. Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Ters ara sunucu ile Kestrel ne zaman kullanılır?

Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir. Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.

Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

Ters Proxy yapılandırmasında kullanılan Kestrel:

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.

Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez. Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel `Host` ' den bağımsız olarak bu bağlantı noktası için tüm trafiği işler. Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.

Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.

Ters proxy:

* , Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.
* Ek bir yapılandırma ve savunma katmanı sağlayın.
* , Mevcut altyapıyla daha iyi tümleşebilir.
* Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın. Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.

> [!WARNING]
> Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core uygulamalarında Kestrel kullanma

Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.

ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır. *Program.cs*' de, şablon kodu, arka planda <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>çağırır.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

`CreateDefaultBuilder` ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host>*bir konak ayarlama* bölümüne bakın.

`CreateDefaultBuilder`çağrıldıktan sonra ek yapılandırma sağlamak için `ConfigureKestrel`kullanın:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

Uygulama, Konağı kurmak için `CreateDefaultBuilder` çağırmazsa, `ConfigureKestrel`çağrılmadan **önce** <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağırın:

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a>Kestrel seçenekleri

Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın. `Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.

Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir. Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

Aşağıdaki yaklaşımlardan **birini** kullanın:

* `Startup.ConfigureServices`'de Kestrel yapılandırma:

  1. `Startup` sınıfına bir `IConfiguration` örneği ekleyin. Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.
  2. `Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin:

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* Ana bilgisayarı oluştururken Kestrel yapılandırma:

  *Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.

### <a name="keep-alive-timeout"></a>Etkin tut zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar. Varsayılan olarak 2 dakikadır.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a>İstemci bağlantıları üst sınırı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır. Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).

### <a name="maximum-request-body-size"></a>En büyük istek gövdesi boyutu

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.

ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur. `MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.

Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.

### <a name="minimum-request-body-data-rate"></a>En az istek gövdesi veri hızı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler. Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez. Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.

Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.

Yanıt için bir minimum oran da geçerlidir. İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında `RequestBody` veya `Response` sahip olma dışında aynıdır.

*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

İstek çoğullama için protokol desteğinin desteklenmediği için, önceki örnekte başvurulan oran özelliği http/2 istekleri için `HttpContext.Features`, HTTP/2 için de hız sınırlarını değiştirmek için desteklenmez. `KestrelServerOptions.Limits` aracılığıyla yapılandırılan sunucu genelindeki hız sınırları hem HTTP/1. x hem de HTTP/2 bağlantıları için geçerlidir.

### <a name="request-headers-timeout"></a>İstek üst bilgileri zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar. Varsayılan değer 30 saniyedir.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a>Bağlantı başına en fazla akış

`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar. Fazlalık akışlar reddedildi.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

Varsayılan değer 100’dür.

### <a name="header-table-size"></a>Üst bilgi tablosu boyutu

HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar. `Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar. Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

Varsayılan değer 4096 ' dir.

### <a name="maximum-frame-size"></a>En büyük çerçeve boyutu

`Http2.MaxFrameSize`, alacak HTTP/2 bağlantı çerçevesi yükünün en büyük boyutunu belirtir. Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

Varsayılan değer 2 ^ 14 ' dir (16.384).

### <a name="maximum-request-header-size"></a>En fazla istek üst bilgi boyutu

`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir. Bu sınır, hem sıkıştırılmış hem de sıkıştırılmamış temsillerinde birlikte hem ad hem de değer için geçerlidir. Değer sıfırdan büyük (0) olmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

Varsayılan değer 8.192 ' dir.

### <a name="initial-connection-window-size"></a>İlk bağlantı pencere boyutu

`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir. İstekler aynı zamanda `Http2.InitialStreamWindowSize`ile sınırlıdır. Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

Varsayılan değer 128 KB 'tır (131.072).

### <a name="initial-stream-window-size"></a>İlk akış pencere boyutu

`Http2.InitialStreamWindowSize`, istek (Stream) başına bir seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir. İstekler aynı zamanda `Http2.InitialStreamWindowSize`ile sınırlıdır. Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

Varsayılan değer 96 KB 'tır (98.304).

### <a name="synchronous-io"></a>Zaman uyumlu GÇ

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler. Varsayılan değer `true`.

> [!WARNING]
> Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur. Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.

Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a>Uç nokta yapılandırması

Varsayılan olarak, ASP.NET Core bağlar:

* `http://localhost:5000`
* `https://localhost:5001` (yerel bir geliştirme sertifikası mevcut olduğunda)

Kullanarak URL 'Leri belirtin:

* ortam değişkeni `ASPNETCORE_URLS`.
* `--urls` komut satırı bağımsız değişkeni.
* `urls` ana bilgisayar yapılandırma anahtarı.
* `UseUrls` genişletme yöntemi.

Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS). Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).

Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.

Geliştirme sertifikası oluşturuldu:

* [.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.
* [Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.

Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.

Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.

Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerini çağırın.

`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).

`KestrelServerOptions` yapılandırması:

### <a name="configureendpointdefaultsactionlistenoptions"></a>ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)

Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir. `ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a>ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)

Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir. `ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.


### <a name="configureiconfiguration"></a>Yapılandırma (Iconation)

Giriş olarak bir <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur. Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.

### <a name="listenoptionsusehttps"></a>ListenOptions. UseHttps

Kestrel 'i HTTPS kullanacak şekilde yapılandırın.

`ListenOptions.UseHttps` uzantıları:

* `UseHttps` &ndash; varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın. Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` parametreleri:

* `filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.
* X. 509.440 sertifika verilerine erişmek için gereken parola `password`.
* `configureOptions`, `HttpsConnectionAdapterOptions`yapılandırmak için `Action`. `ListenOptions`döndürür.
* `storeName`, sertifikanın yükleneceği sertifika deposudur.
* `subject`, sertifikanın konu adıdır.
* `allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.
* `location`, sertifikanın yükleneceği mağaza konumudur.
* `serverCertificate` X. 509.440 sertifikasıdır.

Üretimde HTTPS 'nin açıkça yapılandırılması gerekir. En azından, varsayılan bir sertifika sağlanmalıdır.

Daha sonra açıklanan desteklenen yapılandırma:

* Yapılandırma yok
* Varsayılan sertifikayı yapılandırmadan Değiştir
* Koddaki varsayılanları değiştirme

*Yapılandırma yok*

Kestrel `http://localhost:5000` ve `https://localhost:5001` dinler (varsayılan bir sertifika varsa).

<a name="configuration"></a>

*Varsayılan sertifikayı yapılandırmadan Değiştir*

Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır. Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir. Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.

Aşağıdaki *appSettings. JSON* örneğinde:

* Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.
* Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **varsayılan** veya geliştirme sertifikası > **Sertifikalar** altında tanımlanan sertifikaya geri döner.

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir. Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirlenebilir:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Şema notları:

* Uç nokta adları büyük/küçük harfe duyarlıdır. Örneğin, `HTTPS` ve `Https` geçerlidir.
* Her uç nokta için `Url` parametresi gereklidir. Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.
* Bu uç noktalar, üst düzey `Urls` yapılandırmasında tanımlananlara eklemek yerine bunların yerini alır. `Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.
* `Certificate` bölümü isteğe bağlıdır. `Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır. Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.
* `Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.
* Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.
* `options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemi ile bir `KestrelConfigurationLoader` döndürür:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

`KestrelServerOptions.ConfigurationLoader`, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından sağlana gibi var olan yükleyicisindeki yinelenmeye devam etmek için doğrudan erişilebilir.

* Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.
* `options.Configure(context.Configuration.GetSection("{SECTION}"))` başka bir bölümle yeniden çağırarak birden çok yapılandırma yüklenebilir. `Load` önceki örneklerde açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır. Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` çağırmaz.
* `KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir. Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.

*Koddaki varsayılanları değiştirme*

`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions`için varsayılan ayarları değiştirmek üzere kullanılabilir. `ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

*SNı için Kestrel desteği*

[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir. SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir. İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.

Kestrel `ServerCertificateSelector` geri çağırma aracılığıyla SNı destekler. Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.

SNı desteği şunları gerektirir:

* Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor. `net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`. Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.
* Tüm Web siteleri aynı Kestrel örneğinde çalışır. Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

### <a name="connection-logging"></a>Bağlantı günlüğü

Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın. Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur. `UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir. `UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a>TCP yuvasına bağlama

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>olan bir uç nokta için HTTPS 'yi yapılandırır. Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>UNIX yuvasına bağlama

Aşağıdaki örnekte gösterildiği gibi NGINX ile daha iyi performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* NGINX confiuguration dosyasında, `server` > `location` > `proxy_pass` girişi `http://unix:/tmp/{KESTREL SOCKET}:/;`olarak ayarlayın. `{KESTREL SOCKET}`, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> için belirtilen yuvanın adıdır (örneğin, önceki örnekte `kestrel-test.sock`).
* Yuvanın NGINX tarafından yazılabilir olduğundan emin olun (örneğin, `chmod go+w /tmp/kestrel-test.sock`). 

### <a name="port-0"></a>Bağlantı noktası 0

`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır. Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Sınırlamalar

Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* `--urls` komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* `ASPNETCORE_URLS` ortam değişkeni

Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır. Ancak, aşağıdaki sınırlamalara dikkat edin:

* HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.
* `Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.

### <a name="iis-endpoint-configuration"></a>IIS uç nokta yapılandırması

IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls`tarafından ayarlanır. Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.

### <a name="listenoptionsprotocols"></a>ListenOptions. Protocols

`Protocols` özelliği, bir bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) belirler. `HttpProtocols` numaralandırmasından `Protocols` özelliğine bir değer atayın.

| `HttpProtocols` Enum değeri | Bağlantı protokolü izin verildi |
| -------------------------- | ----------------------------- |
| `Http1`                    | Yalnızca HTTP/1.1. , TLS olmadan veya ile kullanılabilir. |
| `Http2`                    | Yalnızca HTTP/2. Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir. |
| `Http1AndHttp2`            | HTTP/1.1 ve HTTP/2. HTTP/2 bir TLS ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir. |

Varsayılan protokol HTTP/1.1 ' dir.

HTTP/2 için TLS kısıtlamaları:

* TLS sürüm 1,2 veya üzeri
* Yeniden anlaşma devre dışı
* Sıkıştırma devre dışı
* En az kısa ömürlü anahtar değişim boyutları:
  * Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum
  * Sınırlı alan Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit minimum
* Şifre paketi kara listede değil

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;&rbrack; `TLS-ECDHE`, P-256 eliptik eğrisi &lbrack;`FIPS186`&rbrack; varsayılan olarak desteklenir.

Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir. Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

İsteğe bağlı olarak, belirli şifrelemeler için bağlantı başına TLS el sıkışmaları filtrelemek üzere `IConnectionAdapter` bir uygulama oluşturun:

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
    });
});
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // No encryption is used with a CipherAlgorithmType.Null cipher algorithm.
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

*Protokolü yapılandırmadan ayarla*

Kestrel yapılandırmasını yüklemek için varsayılan olarak `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır.

Aşağıdaki *appSettings. JSON* örneğinde, tüm Kestrel uç noktaları için varsayılan bir bağlantı protokolü (http/1.1 ve http/2) oluşturulur:

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

Aşağıdaki yapılandırma dosyası örneği, belirli bir uç nokta için bağlantı protokolü oluşturur:

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.

## <a name="transport-configuration"></a>Aktarım yapılandırması

ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır. Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağıran ve aşağıdaki paketlerden birine bağlı olan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamaları için bir son değişiklik değildir:

* [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)
* [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Libuv kullanımını gerektiren projeler için:

* Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>çağrısı:

  ```csharp
  public class Program
  {
      public static void Main(string[] args)
      {
          CreateWebHostBuilder(args).Build().Run();
      }

      public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
          WebHost.CreateDefaultBuilder(args)
              .UseLibuv()
              .UseStartup<Startup>();
  }
  ```

### <a name="url-prefixes"></a>URL önekleri

`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.

Yalnızca HTTP URL ön ekleri geçerlidir. Kestrel, `UseUrls`kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.

* Bağlantı noktası numarası olan IPv4 adresi

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.

* Bağlantı noktası numarasına sahip IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.

* Bağlantı noktası numarası olan ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Ana bilgisayar adları, `*`ve `+`özel değildir. Geçerli bir IP adresi olarak tanınmayan bir şey veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanır. Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.

  > [!WARNING]
  > Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.

* Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  `localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır. İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz. Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.

## <a name="host-filtering"></a>Konak filtreleme

Kestrel, `http://example.com:5000`gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar. Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur. Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır. `Host` üstbilgileri doğrulanmadı.

Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın. Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır. Ara yazılım, <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından eklenir:

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır. Ara yazılımı etkinleştirmek için *appSettings. json*/appSettings içinde bir `AllowedHosts` anahtarı tanımlayın *.\<environmentname >. JSON*. Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:

*appSettings. JSON*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği de vardır. İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir. Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur. Kestrel genel kullanıma açık bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara yazılımı ile `AllowedHosts` ayarlama uygundur.
>
> Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index). Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.

Kestrel aşağıdaki senaryoları destekler:

* HTTPS
* [WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme
* NGINX 'in arkasında yüksek performans için UNIX Yuvaları

Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Ters ara sunucu ile Kestrel ne zaman kullanılır?

Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir. Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.

Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

Ters Proxy yapılandırmasında kullanılan Kestrel:

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.

Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez. Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel `Host` ' den bağımsız olarak bu bağlantı noktası için tüm trafiği işler. Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.

Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.

Ters proxy:

* , Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.
* Ek bir yapılandırma ve savunma katmanı sağlayın.
* , Mevcut altyapıyla daha iyi tümleşebilir.
* Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın. Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.

> [!WARNING]
> Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core uygulamalarında Kestrel kullanma

Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.

ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır. *Program.cs*' de, şablon kodu, arka planda <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>çağırır.

`CreateDefaultBuilder`çağrıldıktan sonra ek yapılandırma sağlamak için, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>çağırın:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

`CreateDefaultBuilder` ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host>*bir konak ayarlama* bölümüne bakın.

## <a name="kestrel-options"></a>Kestrel seçenekleri

Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın. `Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.

Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir. Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

Aşağıdaki yaklaşımlardan **birini** kullanın:

* `Startup.ConfigureServices`'de Kestrel yapılandırma:

  1. `Startup` sınıfına bir `IConfiguration` örneği ekleyin. Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.
  2. `Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin:

     ```csharp
     using Microsoft.Extensions.Configuration
     
     public class Startup
     {
         public Startup(IConfiguration configuration)
         {
             Configuration = configuration;
         }

         public IConfiguration Configuration { get; }

         public void ConfigureServices(IServiceCollection services)
         {
             services.Configure<KestrelServerOptions>(
                 Configuration.GetSection("Kestrel"));
         }

         public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
         {
             ...
         }
     }
     ```

* Ana bilgisayarı oluştururken Kestrel yapılandırma:

  *Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.

### <a name="keep-alive-timeout"></a>Etkin tut zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar. Varsayılan olarak 2 dakikadır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a>İstemci bağlantıları üst sınırı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır. Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).

### <a name="maximum-request-body-size"></a>En büyük istek gövdesi boyutu

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.

ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur. `MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.

Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.

### <a name="minimum-request-body-data-rate"></a>En az istek gövdesi veri hızı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler. Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez. Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.

Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.

Yanıt için bir minimum oran da geçerlidir. İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında `RequestBody` veya `Response` sahip olma dışında aynıdır.

*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a>İstek üst bilgileri zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar. Varsayılan değer 30 saniyedir.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a>Zaman uyumlu GÇ

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler. Varsayılan değer `true`.

> [!WARNING]
> Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur. Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.

Aşağıdaki örnek, zaman uyumlu GÇ 'yi devre dışı bırakır:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a>Uç nokta yapılandırması

Varsayılan olarak, ASP.NET Core bağlar:

* `http://localhost:5000`
* `https://localhost:5001` (yerel bir geliştirme sertifikası mevcut olduğunda)

Kullanarak URL 'Leri belirtin:

* ortam değişkeni `ASPNETCORE_URLS`.
* `--urls` komut satırı bağımsız değişkeni.
* `urls` ana bilgisayar yapılandırma anahtarı.
* `UseUrls` genişletme yöntemi.

Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS). Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).

Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.

Geliştirme sertifikası oluşturuldu:

* [.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.
* [Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.

Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.

Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.

Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerini çağırın.

`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).

`KestrelServerOptions` yapılandırması:

### <a name="configureendpointdefaultsactionlistenoptions"></a>ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)

Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir. `ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a>ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)

Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir. `ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.

### <a name="configureiconfiguration"></a>Yapılandırma (Iconation)

Giriş olarak bir <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur. Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.

### <a name="listenoptionsusehttps"></a>ListenOptions. UseHttps

Kestrel 'i HTTPS kullanacak şekilde yapılandırın.

`ListenOptions.UseHttps` uzantıları:

* `UseHttps` &ndash; varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın. Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` parametreleri:

* `filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.
* X. 509.440 sertifika verilerine erişmek için gereken parola `password`.
* `configureOptions`, `HttpsConnectionAdapterOptions`yapılandırmak için `Action`. `ListenOptions`döndürür.
* `storeName`, sertifikanın yükleneceği sertifika deposudur.
* `subject`, sertifikanın konu adıdır.
* `allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.
* `location`, sertifikanın yükleneceği mağaza konumudur.
* `serverCertificate` X. 509.440 sertifikasıdır.

Üretimde HTTPS 'nin açıkça yapılandırılması gerekir. En azından, varsayılan bir sertifika sağlanmalıdır.

Daha sonra açıklanan desteklenen yapılandırma:

* Yapılandırma yok
* Varsayılan sertifikayı yapılandırmadan Değiştir
* Koddaki varsayılanları değiştirme

*Yapılandırma yok*

Kestrel `http://localhost:5000` ve `https://localhost:5001` dinler (varsayılan bir sertifika varsa).

<a name="configuration"></a>

*Varsayılan sertifikayı yapılandırmadan Değiştir*

Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır. Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir. Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.

Aşağıdaki *appSettings. JSON* örneğinde:

* Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.
* Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **varsayılan** veya geliştirme sertifikası > **Sertifikalar** altında tanımlanan sertifikaya geri döner.

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir. Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirlenebilir:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Şema notları:

* Uç nokta adları büyük/küçük harfe duyarlıdır. Örneğin, `HTTPS` ve `Https` geçerlidir.
* Her uç nokta için `Url` parametresi gereklidir. Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.
* Bu uç noktalar, üst düzey `Urls` yapılandırmasında tanımlananlara eklemek yerine bunların yerini alır. `Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.
* `Certificate` bölümü isteğe bağlıdır. `Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır. Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.
* `Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.
* Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.
* `options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemi ile bir `KestrelConfigurationLoader` döndürür:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

`KestrelServerOptions.ConfigurationLoader`, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından sağlana gibi var olan yükleyicisindeki yinelenmeye devam etmek için doğrudan erişilebilir.

* Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.
* `options.Configure(context.Configuration.GetSection("{SECTION}"))` başka bir bölümle yeniden çağırarak birden çok yapılandırma yüklenebilir. `Load` önceki örneklerde açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır. Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` çağırmaz.
* `KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir. Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.

*Koddaki varsayılanları değiştirme*

`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions`için varsayılan ayarları değiştirmek üzere kullanılabilir. `ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

*SNı için Kestrel desteği*

[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir. SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir. İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.

Kestrel `ServerCertificateSelector` geri çağırma aracılığıyla SNı destekler. Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.

SNı desteği şunları gerektirir:

* Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor. `net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`. Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.
* Tüm Web siteleri aynı Kestrel örneğinde çalışır. Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

### <a name="connection-logging"></a>Bağlantı günlüğü

Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın. Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur. `UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir. `UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a>TCP yuvasına bağlama

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>olan bir uç nokta için HTTPS 'yi yapılandırır. Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>UNIX yuvasına bağlama

Aşağıdaki örnekte gösterildiği gibi NGINX ile daha iyi performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

* NGINX confiuguration dosyasında, `server` > `location` > `proxy_pass` girişi `http://unix:/tmp/{KESTREL SOCKET}:/;`olarak ayarlayın. `{KESTREL SOCKET}`, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> için belirtilen yuvanın adıdır (örneğin, önceki örnekte `kestrel-test.sock`).
* Yuvanın NGINX tarafından yazılabilir olduğundan emin olun (örneğin, `chmod go+w /tmp/kestrel-test.sock`). 

### <a name="port-0"></a>Bağlantı noktası 0

`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır. Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Sınırlamalar

Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* `--urls` komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* `ASPNETCORE_URLS` ortam değişkeni

Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır. Ancak, aşağıdaki sınırlamalara dikkat edin:

* HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.
* `Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.

### <a name="iis-endpoint-configuration"></a>IIS uç nokta yapılandırması

IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls`tarafından ayarlanır. Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.

## <a name="transport-configuration"></a>Aktarım yapılandırması

ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır. Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağıran ve aşağıdaki paketlerden birine bağlı olan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamaları için bir son değişiklik değildir:

* [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)
* [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Libuv kullanımını gerektiren projeler için:

* Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>çağrısı:

  ```csharp
  public class Program
  {
      public static void Main(string[] args)
      {
          CreateWebHostBuilder(args).Build().Run();
      }

      public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
          WebHost.CreateDefaultBuilder(args)
              .UseLibuv()
              .UseStartup<Startup>();
  }
  ```

### <a name="url-prefixes"></a>URL önekleri

`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.

Yalnızca HTTP URL ön ekleri geçerlidir. Kestrel, `UseUrls`kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.

* Bağlantı noktası numarası olan IPv4 adresi

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.

* Bağlantı noktası numarasına sahip IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.

* Bağlantı noktası numarası olan ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Ana bilgisayar adları, `*`ve `+`özel değildir. Geçerli bir IP adresi olarak tanınmayan bir şey veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanır. Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.

  > [!WARNING]
  > Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.

* Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  `localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır. İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz. Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.

## <a name="host-filtering"></a>Konak filtreleme

Kestrel, `http://example.com:5000`gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar. Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur. Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır. `Host` üstbilgileri doğrulanmadı.

Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın. Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır. Ara yazılım, <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından eklenir:

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır. Ara yazılımı etkinleştirmek için *appSettings. json*/appSettings içinde bir `AllowedHosts` anahtarı tanımlayın *.\<environmentname >. JSON*. Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:

*appSettings. JSON*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği de vardır. İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir. Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur. Kestrel genel kullanıma açık bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara yazılımı ile `AllowedHosts` ayarlama uygundur.
>
> Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* Linux üzerinde UNIX yuvaları kullanırken, yuva uygulama kapatılırken otomatik olarak silinmez. Daha fazla bilgi için [Bu GitHub sorununa](https://github.com/dotnet/aspnetcore/issues/14134)bakın.
* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [RFC 7230: Ileti sözdizimi ve yönlendirme (Bölüm 5,4: Ana bilgisayar)](https://tools.ietf.org/html/rfc7230#section-5.4)
