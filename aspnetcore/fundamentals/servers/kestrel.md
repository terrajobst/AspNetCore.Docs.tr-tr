---
title: ASP.NET core'da kestrel web sunucusu uygulaması
author: guardrex
description: Kestrel'i, ASP.NET Core için platformlar arası web sunucusu hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/18/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: fe74583cdc1c8fa40ab12b4b31dc47f5e0296a6c
ms.sourcegitcommit: 3eedd6180fbbdcb81a8e1ebdbeb035bf4f2feb92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284525"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET core'da kestrel web sunucusu uygulaması

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), ve [Stephen Halter](https://twitter.com/halter73)

Kestrel'i olduğu bir platformlar arası [ASP.NET Core web sunucusu](xref:fundamentals/servers/index). Kestrel'i ASP.NET Core proje şablonları, varsayılan olarak bulunan bir web sunucusudur.

Kestrel'i aşağıdaki senaryoları destekler:

::: moniker range=">= aspnetcore-2.2"

* HTTPS
* Donuk yükseltme etkinleştirmek için kullanılan [WebSockets](https://github.com/aspnet/websockets)
* Yüksek performans Ngınx arkasında UNIX yuva
* HTTP/2 (Macos'ta dışındaki&dagger;)

&dagger;HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* HTTPS
* Donuk yükseltme etkinleştirmek için kullanılan [WebSockets](https://github.com/aspnet/websockets)
* Yüksek performans Ngınx arkasında UNIX yuva

::: moniker-end

Kestrel'i tüm platformlarda ve .NET Core destekleyen sürümler desteklenir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a>HTTP/2 desteği

[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki gereksinimleri dayandırırsanız ASP.NET Core uygulamaları karşılanması için kullanılabilir:

* İşletim Sistemi&dagger;
  * Windows Server 2016/Windows 10 veya üzeri&Dagger;
  * Linux OpenSSL 1.0.2 veya daha sonra (örneğin, Ubuntu 16.04 veya üzeri)
* Hedef çerçeve: .NET Core 2.2 veya üzeri
* [Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı
* TLS 1.2 veya sonraki bir bağlantı

&dagger;HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.
&Dagger;Kestrel'i HTTP/2 Windows Server 2012 R2 ve Windows 8.1 için destek sınırlıdır. Bu işletim sistemlerinde desteklenen TLS şifre paketlerinin listesini sınırlı olduğundan destek sınırlıdır. Bir Eliptik Eğri Dijital imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika, TLS bağlantıları güvenli hale getirmek için gerekebilir.

Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.

HTTP/2 varsayılan olarak devre dışıdır. Yapılandırma hakkında daha fazla bilgi için bkz. [Kestrel seçenekleri](#kestrel-options) ve [ListenOptions.Protocols](#listenoptionsprotocols) bölümler.

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Ne zaman Kestrel ters Ara sunucu ile kullanılır.

Tek başına veya birlikte kestrel kullanılabilir bir *ters Ara sunucu*, gibi [Internet Information Services (IIS)](https://www.iis.net/), [Ngınx](http://nginx.org), veya [Apache](https://httpd.apache.org/). Ters Ara sunucu HTTP isteklerinin ağdan alır ve bunları Kestrel için iletir.

Bir edge (Internet'e yönelik) web sunucusu olarak kullanılan kestrel:

![Kestrel'i ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

Ters proxy yapılandırmasında kullanılan kestrel:

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir desteklenen barındırma ASP.NET Core 2.1 veya Internet'ten isteklerini alacak sonraki uygulamalar için bir yapılandırmadır.

Kestrel'i ters Ara sunucu olmadan bir uç sunucusu olarak kullanılan aynı IP adresini ve bağlantı noktası arasında birden çok işlem paylaşımı desteklemez. Kestrel'i bir bağlantı noktasında dinleyecek şekilde yapılandırıldığında, Kestrel tüm istekleri bağımsız olarak bu bağlantı noktası trafiğini işler `Host` üstbilgileri. Bağlantı noktalarını paylaşan bir ters proxy Kestrel benzersiz bir IP ve bağlantı noktası isteklerini iletmek için özelliğine sahiptir.

Ters proxy sunucusu gerekli olmasa bile bir ters proxy sunucusu kullanarak iyi bir seçim olabilir.

Ters proxy:

* Barındırdığı uygulamaları kullanıma sunulan ortak yüzey alanını sınırlayabilirsiniz.
* Ek bir yapılandırma ve koruma katmanı sağlar.
* Var olan altyapınızla tümleştirebilirsiniz daha iyi.
* Yük Dengeleme ve güvenli (HTTPS) yapılandırma basitleştirin. Ters proxy sunucusu yalnızca bir X.509 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak iç ağ üzerinde uygulama sunucularınız ile iletişim kurabilir.

> [!WARNING]
> Ters proxy yapılandırmasında barındırma gerektirir [filtreleme konak](#host-filtering).

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core uygulamalarında Kestrel kullanma

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).

ASP.NET Core proje şablonları, varsayılan olarak Kestrel kullanın. İçinde *Program.cs*, şablon kod çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, çağıran <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> arka planda.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

Arama sonra ek bir yapılandırma sağlamak üzere `CreateDefaultBuilder`, kullanın `ConfigureKestrel`:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

Uygulama çağırırsanız değil `CreateDefaultBuilder` ana bilgisayar'kurmak için çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **önce** çağırma `ConfigureKestrel`:

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        })
        .Build();

    host.Run();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Arama sonra ek bir yapılandırma sağlamak üzere `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a>Kestrel'i seçenekleri

Kestrel'i web sunucusu Internet'e yönelik dağıtımlarda özellikle yararlı olan kısıtlaması yapılandırma seçenekleri vardır.

Kısıtlamaları ayarlama <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfı. `Limits` Özelliği bir örneğini tutan <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfı.

Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanı:

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a>Tutma zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

Alır veya ayarlar [tutma zaman aşımı](https://tools.ietf.org/html/rfc7230#section-6.5). Varsayılan 2 dakika olarak.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

::: moniker-end

### <a name="maximum-client-connections"></a>En fazla istemci bağlantısı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

Aşağıdaki kod ile tüm uygulama için eşzamanlı açık TCP bağlantıları sayısı ayarlanabilir:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

HTTP veya HTTPS, başka bir protokol (örneğin, WebSockets istek üzerine) a yükseltilmiştir bağlantıları için ayrı bir sınır yoktur. Bir bağlantı yükseltildikten sonra karşı sayılır değil `MaxConcurrentConnections` sınırı.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

Bağlantı sayısı, varsayılan olarak sınırsız (null) olur.

### <a name="maximum-request-body-size"></a>En fazla istek gövdesi boyutu

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

Varsayılan en büyük istek gövdesi boyutu 30.000.000, yaklaşık 28.6 MB bayttır.

Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşımdır <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliği bir eylem yöntemi:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Her istek için uygulama kısıtlama yapılandırma gösteren bir örnek aşağıda verilmiştir:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

Belirli bir istekte Ara yazılımında ayarı geçersiz kılabilirsiniz:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

Uygulama isteği okumak başlatıldıktan sonra bir istekte sınırını yapılandırmak çalışırsanız, bir özel durum oluşturulur. Var. bir `IsReadOnly` gösterir özelliği `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

Bir uygulamayı çalıştırdığınızda [işlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model) arkasında [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module), IIS sınırı IGNORE_DUP_KEY Kestrel'ın istek gövdesi boyutu sınırını devre dışı.

### <a name="minimum-request-body-data-rate"></a>En az bir istek gövdesi veri hızı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

Veri içinde belirtilen hızda bayt/saniye gelen, saniyede kestrel denetler. Hızı en düşerse, bağlantının zaman aşımına uğradı. Yetkisiz kullanım süresi Kestrel en düşük kadar gönderme hızını artırmak için istemci verdiğini süreyi belirtir; Bu süre boyunca oranı işaretlenmemiş. Yetkisiz kullanım süresi, başlangıçta TCP yavaş başlatma nedeniyle yavaş bir hızda veri gönderen bağlantıları verilmemesini yardımcı olur.

Varsayılan en az 5 saniye yetkisiz kullanım süresi ile 240 bayt/saniye oranıdır.

En düşük bir ücretle yanıt için de geçerlidir. İstek sınırı ve yanıt sınırı ayarlamak için kod olması dışında aynıdır `RequestBody` veya `Response` özelliği ve arabirimi adları.

En az veriyi hızları yapılandırma gösteren bir örnek aşağıdadır *Program.cs*:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

Ara yazılım istek başına en düşük oran sınırları geçersiz kılabilirsiniz:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

Önceki örnekte başvurulan hiçbiri oranı özelliği mevcut `HttpContext.Features` istek başına temelinde oran sınırlarını değiştirme HTTP/2 için istek çoğullama protokolün desteği nedeniyle desteklenmediğinden, HTTP/2 istekleri için. Sunucu çapında hız sınırları ile yapılandırılmış `KestrelServerOptions.Limits` HTTP/1.x hem de HTTP/2 bağlantıları için hala geçerlidir.

::: moniker-end

### <a name="request-headers-timeout"></a>İstek üstbilgileri zaman aşımı

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

Alır veya sunucu alma isteği üstbilgileri harcadığı en uzun süreyi ayarlar. Varsayılan olarak 30 saniye.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a>Bağlantı başına en fazla akış

`Http2.MaxStreamsPerConnection` HTTP/2 bağlantı başına akış eş zamanlı istek sayısını sınırlar. Aşırı akışları çevrilir.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

Varsayılan değer 100’dür.

### <a name="header-table-size"></a>Üst bilgi tablosu boyutu

HPACK kod çözücü, HTTP/2 bağlantılar için HTTP üstbilgileri açar. `Http2.HeaderTableSize` HPACK kod çözücü kullanan üst bilgi sıkıştırma tablonun boyutunu sınırlar. Değer, sekizlik tabanda sağlanır ve sıfır (0) büyük olmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

Varsayılan değer 4096'dır.

### <a name="maximum-frame-size"></a>En büyük çerçeve boyutu

`Http2.MaxFrameSize` en büyük boyutunu almak için HTTP/2 bağlantı çerçeve yükü gösterir. Değer sekizlik tabanda sağlanır ve 2 arasında olmalıdır ^ (16,384) 14. ve 2 ^ 24-1 (16.777.215).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

Varsayılan değer olan 2 ^ 14 (16,384).

### <a name="maximum-request-header-size"></a>En büyük istek üst bilgi boyutu

`Http2.MaxRequestHeaderFieldSize` izin verilen maksimum boyut sekizli istek üstbilgi değerleri gösterir. Bu sınır, hem adı hem de birlikte bunların sıkıştırılmış ve sıkıştırılmamış gösterimleri değeri için geçerlidir. Değer (0) sıfırdan büyük olmalıdır.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

Olmak üzere 8.192 varsayılan değerdir.

### <a name="initial-connection-window-size"></a>İlk bağlantı pencere boyutu

`Http2.InitialConnectionWindowSize` Sunucu arabellekleri üzerinden tüm istekler (akışları) bağlantı başına tek seferde toplu bayt cinsinden en büyük istek gövdesi veriler gösterir. İstek tarafından da sınırlıdır `Http2.InitialStreamWindowSize`. Değer büyüktür veya eşittir 65.535 ve 2'den az olmalıdır. ^ 31 (2,147,483,648).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

128 KB (131,072) varsayılan değerdir.

### <a name="initial-stream-window-size"></a>Akış başlangıç boyutu penceresi

`Http2.InitialStreamWindowSize` İstek (akışı) başına tek seferde sunucu arabelleğinin bayt cinsinden en büyük istek gövde verilerini gösterir. İstek tarafından da sınırlıdır `Http2.InitialStreamWindowSize`. Değer büyüktür veya eşittir 65.535 ve 2'den az olmalıdır. ^ 31 (2,147,483,648).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

96 KB'lık (98,304) varsayılan değerdir.

::: moniker-end

Diğer seçenekleri Kestrel ve sınırları hakkında daha fazla bilgi için bkz:

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a>Uç nokta yapılandırması

Varsayılan olarak, ASP.NET Core bağlar:

* `http://localhost:5000`
* `https://localhost:5001` (yerel geliştirme sertifikası mevcut olduğunda)

Kullanarak URL'leri belirtin:

* `ASPNETCORE_URLS` ortam değişkeni.
* `--urls` komut satırı bağımsız değişkeni.
* `urls` ana bilgisayar yapılandırma anahtarı.
* `UseUrls` genişletme yöntemi.

Bu yaklaşımları kullanarak sağlanan değer, bir veya daha fazla HTTP ve HTTPS uç noktası (varsayılan sertifika varsa HTTPS) olabilir. Değer noktalı virgülle ayrılmış listesini yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).

Bu yaklaşımlar hakkında daha fazla bilgi için bkz. [sunucu URL'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırmasını](xref:fundamentals/host/web-host#override-configuration).

Bir geliştirme sertifikası oluşturulur:

* Zaman [.NET Core SDK'sı](/dotnet/core/sdk) yüklenir.
* [Dev-certs aracını](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.

Bazı tarayıcılar, tarayıcının yerel geliştirme sertifikasına güvenmek açık izin vermenizi gerektirir.

ASP.NET Core 2.1 ve üzeri proje şablonları varsayılan olarak HTTPS üzerinde çalışır ve dahil etmek için uygulamaları yapılandırma [HTTPS yeniden yönlendirmesi ve HSTS desteği](xref:security/enforcing-ssl).

Çağrı <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> Kestrel için URL ön ekleri ve bağlantı noktalarını yapılandırmak için.

`UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır ancak daha sonra (bir varsayılan sertifika için HTTPS uç noktasının kullanılabilir olmalıdır, bu bölümde belirtilen kısıtlamalara sahip Yapılandırma).

ASP.NET Core 2.1 veya üzeri `KestrelServerOptions` yapılandırma:

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a>ConfigureEndpointDefaults (Eylem&lt;ListenOptions&gt;)

Bir yapılandırma belirtir `Action` belirtilen her uç nokta için çalıştırılacak. Çağırma `ConfigureEndpointDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.

::: moniker range=">= aspnetcore-3.0"

```csharp
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.ConfigureKestrel(serverOptions =>
        {
            serverOptions.ConfigureEndpointDefaults(configureOptions =>
            {
                configureOptions.NoDelay = true;
            });
        });
        webBuilder.UseStartup<Startup>();
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ConfigureEndpointDefaults(configureOptions =>
            {
                configureOptions.NoDelay = true;
            });
        });
```

::: moniker-end

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a>ConfigureHttpsDefaults (Eylem&lt;HttpsConnectionAdapterOptions&gt;)

Bir yapılandırma belirtir `Action` her HTTPS uç noktası için çalıştırılacak. Çağırma `ConfigureHttpsDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.

::: moniker range=">= aspnetcore-3.0"

```csharp
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.ConfigureKestrel(serverOptions =>
        {
            serverOptions.ConfigureHttpsDefaults(options =>
            {
                // certificate is an X509Certificate2
                options.ServerCertificate = certificate;
            });
        });
        webBuilder.UseStartup<Startup>();
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ConfigureHttpsDefaults(httpsOptions =>
            {
                // certificate is an X509Certificate2
                httpsOptions.ServerCertificate = certificate;
            });
        });
```

::: moniker-end

### <a name="configureiconfiguration"></a>Configure(IConfiguration)

Alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur bir <xref:Microsoft.Extensions.Configuration.IConfiguration> giriş olarak. Yapılandırma için yapılandırma bölümü için Kestrel kapsamlandırılmalıdır.

### <a name="listenoptionsusehttps"></a>ListenOptions.UseHttps

Kestrel'i HTTPS kullanacak şekilde yapılandırın.

`ListenOptions.UseHttps` uzantılar:

* `UseHttps` &ndash; Kestrel'i varsayılan sertifika ile HTTPS kullanacak şekilde yapılandırın. Varsayılan Sertifika yapılandırılmışsa, bir özel durum oluşturur.
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

`ListenOptions.UseHttps` Parametreler:

* `filename` uygulamanın içerik dosyaları içeren dizine göre bir sertifika dosyası yolu ve dosya adını ' dir.
* `password` X.509 Sertifika verilere erişmek için gerekli paroladır.
* `configureOptions` olan bir `Action` yapılandırmak için `HttpsConnectionAdapterOptions`. Döndürür `ListenOptions`.
* `storeName` sertifika deposundan sertifikayı yüklemek için ' dir.
* `subject` sertifika için konu adı var.
* `allowInvalid` Geçersiz sertifikaları, otomatik olarak imzalanan sertifikaları gibi düşünülmesi gereken olmadığını gösterir.
* `location` sertifikası yüklemek için depo konumudur.
* `serverCertificate` X.509 sertifikasıdır.

Üretim ortamında, HTTPS açıkça yapılandırılmış olması gerekir. En az bir varsayılan sertifika sağlanmalıdır.

Sonraki bölümde açıklandığı desteklenen yapılandırmalar:

* Yapılandırma yok
* Varsayılan Sertifika yapılandırmasından değiştirin
* Kodda Varsayılanları Değiştir

*Yapılandırma yok*

Kestrel'i dinlediği `http://localhost:5000` ve `https://localhost:5001` (varsayılan sertifika varsa).

<a name="configuration"></a>

*Varsayılan Sertifika yapılandırmasından değiştirin*

<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Kestrel yapılandırmasını varsayılan olarak. Bir varsayılan HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir. Disk üzerindeki bir dosyadan veya bir sertifika deposu kullanmak için URL'leri ve sertifikalar dahil olmak üzere birden fazla uç noktası yapılandırın.

Aşağıdaki *appsettings.json* örneği:

* Ayarlama **AllowInvalid** için `true` geçersiz sertifikaları (örneğin, otomatik olarak imzalanan sertifikalar) izin verilir.
* Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (**HttpsDefaultCert** örnekte) bölümünde tanımlanan sertifika için geri döner **sertifikaları** > **varsayılan** veya geliştirme sertifikası.

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

Kullanmaya alternatif **yolu** ve **parola** herhangi bir sertifikayı sertifika deposuna alanlarını kullanarak sertifikasını belirtmek için düğümüdür. Örneğin, **sertifikaları** > **varsayılan** olarak sertifika belirtilebilir:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Şema Notlar:

* Uç noktaları adları büyük/küçük harfe duyarsızdır. Örneğin, `HTTPS` ve `Https` geçerlidir.
* `Url` Parametresi her uç nokta için gereklidir. Bu parametre için aynı üst düzey biçimdir `Urls` yapılandırma parametresi dışında olan sınırlı için tek bir değer.
* Bu uç noktaları üst düzey tanımlanan değiştirin `Urls` ekleyerek bunları yerine yapılandırma. Uç noktaları aracılığıyla kod içinde tanımlanan `Listen` yapılandırma bölümünde tanımlanan uç noktaları ile bunların toplamı olur.
* `Certificate` Bölümüne, isteğe bağlıdır. Varsa `Certificate` bölüm belirtilmediyse, önceki senaryoda tanımlanan varsayılan değerler kullanılır. Varsayılan değer mevcutsa, sunucunun bir özel durum oluşturur ve başlatmak başarısız olur.
* `Certificate` Bölümü destekler **yolu**&ndash;**parola** ve **konu**&ndash;**Store** sertifikalar.
* Bağlantı noktası çakışmalara neden olmayan sürece herhangi bir sayıda uç noktaları bu şekilde tanımlanabilir.
* `options.Configure(context.Configuration.GetSection("Kestrel"))` döndürür bir `KestrelConfigurationLoader` ile bir `.Endpoint(string name, options => { })` yapılandırılmış bir uç noktanın ayarları desteklemek için kullanılan yöntemi:

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Ayrıca doğrudan erişebilirsiniz `KestrelServerOptions.ConfigurationLoader` tarafından sağlanan gibi mevcut yükleyiciyi yineleme tutmak için <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.

* Her uç nokta yapılandırma bölümü bir seçenekler kullanılabilir `Endpoint` yöntemi böylece özel ayarlarını okuyun.
* Birden fazla yapılandırması çağırarak yüklenmemiş olabilir `options.Configure(context.Configuration.GetSection("Kestrel"))` yeniden başka bir bölüme sahip. Sürece yalnızca son yapılandırma kullanıldığını `Load` önceki örnekleri üzerinde açıkça çağrılır. Metapackage çağrı değil `Load` böylece kendi varsayılan yapılandırma bölümü değiştirilebilir.
* `KestrelConfigurationLoader` yansıtmalar `Listen` API'lerinden ailesi `KestrelServerOptions` olarak `Endpoint` yüklemeleri için aynı yerde kodda ve yapılandırma uç noktaları yapılandırılabilir. Bu aşırı yüklemeler olmayan adlar kullanın ve yalnızca varsayılan yapılandırma ayarlarından kullanma.

*Kodda Varsayılanları Değiştir*

`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` varsayılan ayarlarını değiştirmek için kullanılan `ListenOptions` ve `HttpsConnectionAdapterOptions`, önceki senaryoda belirtilen varsayılan sertifika geçersiz kılma dahil. `ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` tüm uç noktaları yapılandırılmış önce çağrılmalıdır.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*SNI kestrel desteği*

[Sunucu adı belirtme (SNI)](https://tools.ietf.org/html/rfc6066#section-3) birden çok etki alanı aynı IP adresini ve bağlantı noktası üzerinde barındırmak için kullanılabilir. Sunucunun doğru sertifikayı sağlayabilmesi işlevine SNI için TLS anlaşması sırasında sunucusuna güvenli oturum için konak adı istemciye gönderir. İstemci, TLS el sıkışma izleyen güvenli oturum sırasında sunucu ile şifreli iletişim için furnished sertifikayı kullanır.

SNI aracılığıyla kestrel destekler `ServerCertificateSelector` geri çağırma. Geri çağırma ana bilgisayar adını denetleyin ve uygun sertifikayı seçmek izin vermek için bağlantı bir kez çağrılır.

SNI desteği gerektirir:

* Hedef framework üzerinde çalışan `netcoreapp2.1`. Üzerinde `netcoreapp2.0` ve `net461`, geri çağırma çağrılır ancak `name` her zaman `null`. `name` De `null` istemci TLS anlaşması name parametresinde konak sağlamıyorsa.
* Tüm Web sitelerinin aynı Kestrel örneğinde çalıştırın. Kestrel'i ters Ara sunucu olmadan birden çok örneğinde bir IP adresi ve bağlantı noktası paylaşımı desteklemez.

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
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

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
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

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a>Bir TCP yuva için bağlama

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Yöntemi için bir TCP yuva bağlar ve X.509 Sertifika yapılandırma seçenekleri lambda verir:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
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
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

Örnek, bir uç nokta için HTTPS yapılandırır <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>. Özel uç noktaları diğer Kestrel ayarlarını yapılandırmak için aynı API kullanın.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>Bir UNIX yuvası için bağlama

Bir UNIX yuvasıyla dinleyecek <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Bu örnekte gösterildiği gibi Ngınx ile Gelişmiş performans için:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a>Bağlantı noktası 0

Zaman bağlantı noktası numarasını `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar. Aşağıdaki örnek, Kestrel çalışma zamanında gerçekten bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı dinamik bağlantı noktası uygulama nereden ulaşabileceğinizi gösterir:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Sınırlamalar

Uç noktaları ile aşağıdaki yaklaşımlardan yapılandırın:

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* `--urls` komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* `ASPNETCORE_URLS` ortam değişkeni

Bu yöntemler, kod Kestrel dışında sunucuları ile iş yapmak için kullanışlıdır. Ancak, aşağıdaki sınırlamaları unutmayın:

* HTTPS varsayılan sertifikayı HTTPS uç nokta yapılandırmasında sağlanmadığı sürece bu yaklaşımların ile kullanılamaz (örneğin, kullanarak `KestrelServerOptions` yapılandırma veya bu konuda daha önce gösterildiği gibi bir yapılandırma dosyası).
* Hem `Listen` ve `UseUrls` yaklaşımları eşzamanlı olarak kullanılan `Listen` uç noktaları geçersiz kılma `UseUrls` uç noktaları.

### <a name="iis-endpoint-configuration"></a>IIS bitiş noktası yapılandırması

IIS geçersiz kılmak için IIS, URL bağlamaları kullanırken bağlamalar tarafından ayarlanan `Listen` veya `UseUrls`. Daha fazla bilgi için [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konu.

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a>ListenOptions.Protocols

`Protocols` Özelliği kurar HTTP protokollerini (`HttpProtocols`) bir bağlantı uç noktası veya sunucu için etkin. Bir değer atayın `Protocols` özelliğinden `HttpProtocols` sabit listesi.

| `HttpProtocols` Sabit listesi değeri | İzin verilen bağlantı protokolü |
| -------------------------- | ----------------------------- |
| `Http1`                    | HTTP/1.1 yalnızca. İle veya olmadan TLS kullanılabilir. |
| `Http2`                    | HTTP/2 yalnızca. TLS ile kullanılır. Yalnızca istemci uygulamalarını destekliyorsa, TLS kullanılabilir bir [bilgisi modu](https://tools.ietf.org/html/rfc7540#section-3.4). |
| `Http1AndHttp2`            | HTTP/1.1 ve HTTP/2. Bir TLS gerektirir ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı HTTP/2; anlaşmak üzere bağlantı, HTTP/1.1 Aksi takdirde, varsayılan olarak. |

Varsayılan, HTTP/1.1 protokolüdür.

HTTP/2 için TLS kısıtlamaları:

* TLS 1.2 veya sonraki bir sürümü
* Devre dışı yeniden anlaşma
* Sıkıştırma devre dışı
* En düşük kısa ömürlü anahtar değişimi boyutları:
  * Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 BITS en düşük
  * Sınırlı alanda Diffie-Hellman (DHE) &lbrack; `TLS12` &rbrack; &ndash; en az 2048 bit
* Şifre paketini değil kara listede

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; p-256 Eliptik Eğri ile &lbrack; `FIPS186` &rbrack; varsayılan olarak desteklenir.

Aşağıdaki örnek, HTTP/1.1 ve 8000 numaralı bağlantı noktasındaki HTTP/2 bağlantılarına izin verir. Bağlantılar TLS tarafından sağlanan bir sertifika ile güvenli hale getirilir:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

İsteğe bağlı olarak bir `IConnectionAdapter` TLS el sıkışma belirli şifrelemeleri için bağlantı başına temelinde filtre uygulamak için uygulama:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
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

        // Throw NotSupportedException for any cipher algorithm that you don't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null
        // indicates that no cipher algorithm supported by Kestrel matches the
        // requested algorithm(s).
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

*Protokol yapılandırmasını ayarlayın*

<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Kestrel yapılandırmasını varsayılan olarak.

Aşağıdaki *appsettings.json* örnek, bir varsayılan bağlantı protokol (HTTP/1.1 ve HTTP/2) kurulmuş tüm Kestrel'ın uç noktaları için:

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

Aşağıdaki yapılandırma dosyası örneği, belirli bir uç noktası için bir bağlantı protokol oluşturur:

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

Kodda belirtilen protokoller, yapılandırma tarafından ayarlanan değerleri geçersiz.

::: moniker-end

## <a name="transport-configuration"></a>Aktarım yapılandırma

ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda göre. Bu, bir ASP.NET Core 2.0 uygulamaları çağıran 2.1 yükseltme için değişiklik <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ve aşağıdaki paketlerden birini bağlıdır:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket Başvurusu)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

ASP.NET Core 2.1 veya üzerini kullanan projeleri [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ve Libuv kullanılmasını gerektirir:

* İçin bağımlılık ekleme [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) uygulamanın proje dosyasına paket:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                      Version="<LATEST_VERSION>" />
    ```

* Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:

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

### <a name="url-prefixes"></a>URL ön ekleri

Kullanırken `UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı veya `ASPNETCORE_URLS` ortam değişkeni URL ön ekleri olabilir aşağıdaki biçimlerden birini.

Yalnızca HTTP URL ön ekleri geçerlidir. Kullanarak URL bağlamaları yapılandırma sırasında kestrel HTTPS desteklemiyor `UseUrls`.

* Bağlantı noktası numarası ile IPv4 adresi

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` tüm IPv4 adreslerine bağlayan bir özel durumdur.

* Bağlantı noktası numarası ile IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` IPv4 IPv6 eşdeğerdir `0.0.0.0`.

* Bağlantı noktası numarası ile ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Ana makine adları, `*`, ve `+`, özel değildir. Herhangi bir şey geçerli bir IP adresi tanınmıyor veya `localhost` tüm IPv4 ve IPv6 IP bağlar. Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak için kullanın [HTTP.sys](xref:fundamentals/servers/httpsys) veya IIS, Ngınx veya Apache gibi bir ters proxy sunucusu.

  > [!WARNING]
  > Ters proxy yapılandırmasında barındırma gerektirir [filtreleme konak](#host-filtering).

* Konak `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır. İstenen bağlantı noktası başka bir hizmette ya da geri döngü arabirimine tarafından kullanılıyor Kestrel başlatmak başarısız olur. Ya da geri döngü arabirimine başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı günlüğe kaydeder.

## <a name="host-filtering"></a>Konak filtreleme

Kestrel'i yapılandırma gibi önekleri temel alarak desteklerken `http://example.com:5000`, Kestrel büyük ölçüde ana bilgisayar adı yok sayar. Konak `localhost` bağlama geri döngü adresi için kullanılan özel bir durum. Daha açık bir IP adresi tüm genel IP adresine bağlar herhangi diğer barındırın. `Host` üst bilgileri doğrulanmış değil.

Geçici çözüm olarak, konak filtreleme ara yazılımı kullanın. Konak filtreleme ara yazılımı tarafından sağlanan [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) dahil edilen paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri). Ara yazılım tarafından eklenen <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, çağıran <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Konak filtreleme ara yazılım, varsayılan olarak devre dışıdır. Ara yazılım etkinleştirmek için tanımladığınız bir `AllowedHosts` anahtarını *appsettings.json*/*appsettings.\< EnvironmentName > .json*. Bağlantı noktası numaraları olmadan konak adları, noktalı virgülle ayrılmış listesini değerdir:

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Üst bilgileri ara yazılım iletilen](xref:host-and-deploy/proxy-load-balancer) de sahip bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği. İletilen üstbilgileri ara yazılım ve konak filtreleme ara yazılım farklı senaryolar için benzer bir işlevsellik vardır. Ayarı `AllowedHosts` iletilen üstbilgileri Ara yazılımla uygundur `Host` üstbilgi ters Ara sunucu veya yük dengeleyici isteklerle iletirken korunur değil. Ayarı `AllowedHosts` konak filtreleme Ara yazılımla uygundur Kestrel genel kullanıma yönelik bir uç sunucusu kullanıldığında veya zaman `Host` başlığı doğrudan iletilir.
>
> İletilen üstbilgileri ara yazılım hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [Kestrel'i kaynak kodu](https://github.com/aspnet/AspNetCore/tree/master/src/Servers/Kestrel)
* [RFC 7230: İleti söz dizimi ve yönlendirme (Bölüm 5.4: Ana bilgisayarı)](https://tools.ietf.org/html/rfc7230#section-5.4)
