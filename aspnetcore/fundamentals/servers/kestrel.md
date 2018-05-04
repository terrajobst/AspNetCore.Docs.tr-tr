---
title: ASP.NET Core kestrel web sunucusu uygulaması
author: tdykstra
description: ASP.NET Core üzerinde libuv tabanlı için platformlar arası web sunucusu Kestrel hakkında bilgi edinin.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 1709d26f5dfe40d178da70c286d328982f2c39a0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core kestrel web sunucusu uygulaması

Tarafından [zel Dykstra](https://github.com/tdykstra), [Chris fillerin](https://github.com/Tratcher), ve [Stephen Halter](https://twitter.com/halter73)

Kestrel olan bir platformlar arası [ASP.NET Core için web sunucusu](xref:fundamentals/servers/index) göre [libuv](https://github.com/libuv/libuv), platformlar arası zaman uyumsuz g/ç kitaplığı. Kestrel varsayılan ASP.NET Core proje şablonları olarak dahil edilen web sunucusudur.

Kestrel aşağıdaki özellikleri destekler:

* HTTPS
* Etkinleştirmek için kullanılan opak yükseltme [WebSockets](https://github.com/aspnet/websockets)
* Yüksek performanslı Nginx arkasında UNIX yuvaları

Kestrel tüm platformlar ve .NET Core destekleyen sürümler desteklenir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Ters proxy ile Kestrel kullanma zamanı

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters proxy sunucusu*, IIS, Nginx ya da Apache gibi. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel bir ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Kestrel yalnızca bir iç ağ eline sürece Kestrel ters proxy sunucusu ile kullanmanızı öneririz.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bir uygulamayı yalnızca bir iç ağ gelen istekleri kabul ederse, Kestrel doğrudan uygulamanın sunucusu olarak kullanılabilir.

![Kestrel, iç ağınız ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internal.png)

Uygulamanızı internet kullanıma sunma, IIS, Nginx ya da Apache olarak kullanırsanız bir *ters proxy sunucu*. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Güvenlik nedenleriyle (trafiğini Internet'ten gösterilen) kenar dağıtımlar için ters Ara sunucu gereklidir. Kestrel 1.x sürümleri tamamlayıcı uygun zaman aşımları, boyut sınırları ve eş zamanlı bağlantı sınırlarını gibi saldırılarına karşı savunma yoktur.

* * *

Aynı IP adresini ve bağlantı noktası tek bir sunucu üzerinde çalışan paylaşan birden çok uygulama olduğunda bir ters proxy senaryosu bulunmaktadır. Aynı IP adresini ve birden çok işlemler arasında bağlantı noktası paylaşımı Kestrel desteklemediğinden kestrel bu senaryo desteklemiyor. Kestrel Kestrel bir bağlantı noktasında dinleyecek şekilde yapılandırıldığında, bu bağlantı noktası isteklerini ana bilgisayar üstbilgisi bağımsız olarak tüm trafiği işler. Bağlantı noktalarını paylaşabilirsiniz ters bir proxy Kestrel, benzersiz bir IP ve bağlantı noktası isteklerini iletmek için özelliğine sahiptir.

Ters proxy sunucusu gerekli olmasa bile bir ters proxy sunucusu kullanmak iyi bir seçimdir olabilir:

* Barındırdığı uygulamaları sunulan genel yüzey alanını sınırlayabilirsiniz.
* Ek bir yapılandırma ve koruma katmanı sağlar.
* Mevcut altyapısına daha iyi tümleştirme.
* Yük Dengeleme ve SSL yapılandırmasını basitleştirir. Yalnızca ters proxy sunucuyu bir SSL sertifikası gerektirir ve bu sunucu uygulama sunucularınızda düz HTTP kullanarak iç ağ ile iletişim kurabilir.

> [!WARNING]
> Etkin bir ters proxy filtreleme ana bilgisayarla kullanılmıyorsa [ana bilgisayar filtre](#host-filtering) etkinleştirilmesi gerekir.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core uygulamaları Kestrel kullanma

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paket dahil [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

ASP.NET Core proje şablonlarını Kestrel varsayılan olarak kullanın. İçinde *Program.cs*, şablonu kodu çağrılarının [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), çağıran [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) arka planda.

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Yükleme [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet paketi.

Çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) genişletme yöntemi [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) içinde `Main` herhangi belirtme yöntemi [Kestrel seçenekleri](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) , sonraki bölümde gösterildiği gibi gerekli.

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

* * *

### <a name="kestrel-options"></a>Kestrel seçenekleri

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Kestrel web sunucusu Internet'e yönelik dağıtımlarda özellikle yararlı kısıtlaması yapılandırma seçenekleri vardır. Özelleştirilebilir bir birkaç önemli sınırları:

* En fazla istemci bağlantıları
* En büyük istek gövdesi boyutu
* En düşük istek gövdesi veri oranı

Bunlar ve diğer kısıtlamaları ayarlamak [sınırları](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) özelliği [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) sınıfı. `Limits` Özelliği bir örneğini tutan [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) sınıfı.

**En fazla istemci bağlantıları**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

Eşzamanlı açık TCP bağlantısı sayısı tüm uygulama aşağıdaki kod ile ayarlayabilirsiniz:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3-4)]

Başka bir protokol (örneğin, WebSockets istek üzerine) için HTTP veya HTTPS yükseltildi bağlantıları için ayrı bir sınır yoktur. Bir bağlantı yükseltildikten sonra onu karşı sayılan değil `MaxConcurrentConnections` sınırı.

En fazla bağlantı sayısı, varsayılan olarak sınırsız (null) olur.

**En büyük istek gövdesi boyutu**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

Varsayılan en büyük istek gövdesi boyutu 30.000.000, hangi 28.6 MB'dir bayttır.

Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım kullanmaktır [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) bir eylem yönteminin özniteliği:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Aşağıda, her istek için uygulama kısıtlama yapılandırmak nasıl oluşturulduğunu gösteren bir örnek verilmiştir:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

Belirli bir istek Ara yazılımında ayarı geçersiz kılabilirsiniz:

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

Uygulama isteği okumak başlatıldıktan sonra bir istekte sınırını yapılandırmak çalışırsanız özel durum oluşur. Var. bir `IsReadOnly` gösterir özelliği `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

**En düşük istek gövdesi veri oranı**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Kestrel veri belirtilen hızını saniye başına baytların geliş saniyede denetler. Hızı en düşerse, bağlantı zaman aşımına. Yetkisiz kullanım süresi Kestrel en düşük kadar gönderme oranını artırmak için istemcinin verir süreyi belirtir; oranı, bu süre boyunca işaretli değil. Yetkisiz kullanım süresi başlangıçta TCP yavaş başlatma nedeniyle yavaş bir hızda veri gönderme bağlantıları bırakarak önlemeye yardımcı olur.

Varsayılan en az 5 saniye yetkisiz kullanım süresi ile 240 bayt/saniye hızıdır.

Minimum oran yanıt için de geçerlidir. İstek sınırı ve yanıt sınırı ayarlamak için kod olması dışında aynı olduğundan `RequestBody` veya `Response` özelliği ve arabirim adlarını.

En az veri hızları yapılandırmak nasıl oluşturulduğunu gösteren bir örnek şudur *Program.cs*:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-9)]

İstek başına oranları Ara yazılımında yapılandırabilirsiniz:

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

Diğer Kestrel seçenekleri ve sınırları hakkında daha fazla bilgi için bkz:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Kestrel seçenekleri ve sınırları hakkında daha fazla bilgi için bkz:

* [KestrelServerOptions sınıfı](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

* * *

### <a name="endpoint-configuration"></a>Uç nokta yapılandırması

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. Çağrı [dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) veya [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) yöntemlere [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak için. `UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni ayrıca iş ancak daha sonra bu bölümde belirtildiği sınırlamalar vardır.

`urls` Ana bilgisayar yapılandırma anahtarı konak yapılandırması, uygulama yapılandırması gelmesi gerekir. Ekleme bir `urls` anahtarı ve değeri *appsettings.json* ana bilgisayar yapılandırma yapılandırma dosyasından okunur zamana göre tamamen başlatılmış olduğundan ana bilgisayar yapılandırması etkilemez. Ancak, bir `urls` anahtarını *appsettings.json* ile kullanılan [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ana bilgisayarı yapılandırmak için konak oluşturucu üzerinde:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]

Varsayılan olarak, ASP.NET Core bağlar:

* `http://localhost:5000`
* `https://localhost:5001` (bir yerel geliştirme sertifikası var olduğunda)

Geliştirme sertifikası oluşturulur:

* Zaman [.NET Core SDK](/dotnet/core/sdk) yüklenir.
* [Geliştirme sertifikaları aracı](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) bir sertifika oluşturmak için kullanılır.

Bazı tarayıcılar, yerel geliştirme sertifika güven açık izni tarayıcıya vermenizi gerektirir.

ASP.NET Core 2.1 ve üzeri proje şablonları HTTPS üzerinde varsayılan olarak çalışır ve dahil etmek için uygulamaları yapılandırma [HTTPS yeniden yönlendirmesi ve HSTS Destek](xref:security/enforcing-ssl).

Çağrı [dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) veya [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) yöntemlere [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak için.

`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni ayrıca iş ancak (varsayılan sertifikayı HTTPS uç noktası için kullanılabilir olmalıdır Bu bölümde daha sonra belirtilen sınırlamalara sahip Yapılandırma).

ASP.NET Core 2.1 `KestrelServerOptions` yapılandırma:

**ConfigureEndpointDefaults (Eylem<ListenOptions>)**  
Bir yapılandırma belirtir `Action` belirtilen her uç nokta için çalıştırılacak. Çağırma `ConfigureEndpointDefaults` öncesinde birden çok kez değiştirir `Action`son s `Action` belirtilen.

**ConfigureHttpsDefaults (Eylem<HttpsConnectionAdapterOptions>)**  
Bir yapılandırma belirtir `Action` her HTTPS uç noktası için çalıştırılacak. Çağırma `ConfigureHttpsDefaults` öncesinde birden çok kez değiştirir `Action`son s `Action` belirtilen.

**Configure(IConfiguration)**  
Alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) giriş olarak. Yapılandırma yapılandırma bölümü için Kestrel kapsamlı gerekir.

**ListenOptions.UseHttps**  
Kestrel HTTPS kullanmak üzere yapılandırın.

`ListenOptions.UseHttps` uzantılar:

* `UseHttps` &ndash; Kestrel varsayılan sertifika ile HTTPS kullanmak üzere yapılandırın. Varsayılan Sertifika yapılandırılmışsa, bir özel durum oluşturur.
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

* `filename` uygulamanın içerik dosyalarını içeren dizine göreli bir sertifika dosyası yolu ve dosya adı değil.
* `password` X.509 sertifikası verilere erişmek için gerekli paroladır.
* `configureOptions` olan bir `Action` yapılandırmak için `HttpsConnectionAdapterOptions`. Döndürür `ListenOptions`.
* `storeName` sertifika deposundan sertifika yükleneceği olur.
* `subject` Sertifikanın konu adı var.
* `allowInvalid` Geçersiz sertifikalar, otomatik olarak imzalanan sertifikalar gibi düşünülmesi gereken olmadığını gösterir.
* `location` sertifika yüklemek için depolama konumudur.
* `serverCertificate` X.509 sertifika yok.

Üretimde HTTPS açıkça yapılandırılmalıdır. En az bir varsayılan sertifika sağlanmalıdır.

Sonraki bölümde açıklandığı desteklenen yapılandırmalar:

* Yapılandırma yok
* Yapılandırmasından varsayılan sertifikayı değiştirin
* Kodda varsayılan değerlerini değiştirme

*Yapılandırma yok*

Kestrel dinlediği `http://localhost:5000` ve `https://localhost:5001` (varsayılan sertifika varsa).

Kullanarak URL'leri belirtin:

* `ASPNETCORE_URLS` Ortam değişkeni.
* `--urls` komut satırı bağımsız değişkeni.
* `urls` ana bilgisayar yapılandırma anahtarı.
* `UseUrls` genişletme yöntemi.

Daha fazla bilgi için bkz: [sunucu URL'leri](xref:fundamentals/hosting#server-urls) ve [geçersiz kılınıyor yapılandırma](xref:fundamentals/hosting#overriding-configuration).

Bu yaklaşımı kullanarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktaları (varsayılan sertifika varsa HTTPS) olabilir. Noktalı virgülle ayrılmış liste olarak değerini yapılandırın (örneğin, `"Urls": "http://localhost:8000;http://localhost:8001"`).

*Yapılandırmasından varsayılan sertifikayı değiştirin*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` varsayılan olarak Kestrel yapılandırma yüklenemedi. Bir varsayılan HTTPS uygulama ayarlarını yapılandırma şeması Kestrel için kullanılabilir. Sertifikalar ve URL'leri, disk üzerindeki bir dosyadan veya bir sertifika deposundan kullanmak için de dahil olmak üzere birden fazla uç noktası yapılandırın.

Aşağıdaki *appsettings.json* örnek:

* Ayarlama **AllowInvalid** için `true` geçersiz sertifikaları (örneğin, otomatik olarak imzalanan sertifikalar) kullanımına izin vermek için.
* Bir sertifika belirtmeyen herhangi bir HTTPS uç nokta (**HttpsDefaultCert** aşağıdaki örnekte) altında tanımlanan sertifika için geri döner **sertifikaları** > **varsayılan**  veya geliştirme sertifikası.

```json
{
"Kestrel": {
  "EndPoints": {
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
        "Store": "<certificate store; defaults to My>",
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

Kullanmaya alternatif **yolu** ve **parola** herhangi bir sertifika için sertifika deposu alanlarını kullanarak sertifikasını belirtmek için düğümdür. Örneğin, **sertifikaları** > **varsayılan** sertifika olarak belirtilebilir:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Şema Notlar:

* Uç nokta adları büyük/küçük harfe duyarsızdır. Örneğin, `HTTPS` ve `Https` geçerlidir.
* `Url` Parametresi her uç noktası için gereklidir. Bu parametre için aynı üst düzey biçimidir `Urls` onun dışında yapılandırma parametresi sınırlı için tek bir değer.
* Bu uç noktalar üst düzey tanımlanan Değiştir `Urls` ekleyerek yerine yapılandırma. Uç noktaları kodda tanımlanan `Listen` yapılandırma bölümünde tanımlanan uç noktaları birbirinin yerine kullanılabilir.
* `Certificate` Bölümdür isteğe bağlıdır. Varsa `Certificate` değil bölümünde belirtilen, önceki senaryolarda tanımlanan varsayılan değerler kullanılır. Varsayılan değer varsa, sunucu bir özel durum oluşturur ve başlatılamıyor.
* `Certificate` Bölüm destekler **yolu**&ndash;**parola** ve **konu**&ndash;**deposu** sertifikalar.
* Bağlantı noktası çakışmaları neden olmayan sürece herhangi bir sayıda uç noktaları bu şekilde tanımlanabilir.
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` döndüren bir `KestrelConfigurationLoader` ile bir `.Endpoint(string name, options => { })` yapılandırılmış bir uç noktanın ayarlarını desteklemek için kullanılan yöntem:

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Ayrıca doğrudan erişebilirsiniz `KestrelServerOptions.ConfigurationLoader` tarafından sağlanan gibi varolan yükleyicisi yineleme tutmak için `WebHost.CreatedDeafaultBuilder`.

* Her uç nokta için yapılandırma bölümü bir seçeneklerinde kullanılabilir `Endpoint` yöntemi böylece özel ayarları okuyabilirsiniz.
* Birden çok yapılandırmayı çağırarak yüklenmemiş olabilir `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` yeniden başka bir bölüme sahip. Sürece yalnızca son yapılandırma kullanılır `Load` önceki örneklerinde açık olarak adlandırılır. Metapackage çağrısı değil `Load` böylece kendi varsayılan yapılandırma bölümü değiştirilebilir.
* `KestrelConfigurationLoader` yansıtmalar `Listen` API'lerden ailesi `KestrelServerOptions` olarak `Endpoint` overloads kod ve yapılandırma bitiş noktaları aynı yerde yapılandırılabilir şekilde. Bu aşırı yoksa adları kullanın ve yalnızca yapılandırma varsayılan ayarları kullanabilir.

*Kodda varsayılan değerlerini değiştirme*

`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` varsayılan ayarlarını değiştirmek için kullanılan `ListenOptions` ve `HttpsConnectionAdapterOptions`, önceki senaryoda belirtilen varsayılan sertifika geçersiz kılma dahil olmak üzere. `ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` uç nokta yapılandırılmadan önce çağrılmalıdır.

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

[Sunucu adı göstergesi (SNI)](https://tools.ietf.org/html/rfc6066#section-3) aynı IP adresini ve bağlantı noktası üzerinde birden çok etki alanı ana bilgisayar için kullanılabilir. Böylece sunucunun doğru sertifikayı sağlayabilirler işlevi SNI için TLS anlaşması sırasında sunucuya güvenli oturum için ana bilgisayar adı istemci gönderir. İstemci furnished sertifika TLS el sıkışma izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için kullanır.

Kestrel destekleyen aracılığıyla SNI `ServerCertificateSelector` geri çağırma. Geri arama, ana bilgisayar adı inceleyin ve uygun sertifikayı seçin yazmasına izin vermek için bağlantı bir kez çağrılır.

SNI destek gerektiren hedef framework üzerinde çalışan `netcoreapp2.1`. Üzerinde `netcoreapp2.0` ve `net461`, geri çağırma çağrılır ancak `name` her zaman `null`. `name` De `null` istemci TLS el sıkışma name parametresinde konak sağlamıyorsa.

```csharp
WebHost.CreateDefaultBuilder()
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
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
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

**Bir TCP yuvası bağlama**

[Dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) yöntemi bir TCP yuvasını bağlar ve SSL sertifikası yapılandırma seçenekleri lambda verir:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Nasıl Bu örnek SSL için özel bir uç nokta kullanarak yapılandırır fark [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Özel uç noktaları diğer Kestrel ayarlarını yapılandırmak için aynı API'yi kullanın.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**Bir UNIX yuvası bağlama**

Bir UNIX yuvası ile Dinlemenin [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) Bu örnekte gösterildiği gibi Nginx ile Gelişmiş performans için:

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

**Bağlantı noktası 0**

Bağlantı noktası numarasını `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar. Aşağıdaki örnek, Kestrel çalışma zamanında gerçekten bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı uygulama burada ulaşılabilen dinamik bağlantı noktası gösterir:

```console
Now listening on: http://127.0.0.1:48508
```

**UseUrls,--URL'leri komut satırı bağımsız değişkeni, URL'ler ana bilgisayar yapılandırma anahtarı ve ASPNETCORE_URLS ortam değişkeni sınırlamaları**

Uç noktaları ile aşağıdaki yaklaşımlardan yapılandırın:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* `--urls` Komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* `ASPNETCORE_URLS` Ortam değişkeni

Bu yöntemleri Kestrel dışında sunucularıyla iş kodu yapmak için yararlıdır. Ancak, bu sınırlamaları unutmayın:

* SSL HTTPS uç nokta yapılandırmasında bir varsayılan sertifika sağlanmadığı sürece bu yaklaşımlar ile kullanılamaz (örneğin, kullanarak `KestrelServerOptions` yapılandırma veya bu konuda daha önce gösterildiği gibi bir yapılandırma dosyası).
* Zaman hem `Listen` ve `UseUrls` yaklaşımlar eşzamanlı olarak kullanılan `Listen` uç noktaları geçersiz kılma `UseUrls` uç noktaları.

**IIS bitiş noktası yapılandırması**

IIS geçersiz kılmak için IIS, URL bağlamaları kullanılırken bağlamaları tarafından ayarlanan `Listen` veya `UseUrls`. Daha fazla bilgi için bkz: [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konu.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. URL öneklerini ve Kestrel kullanarak bağlantı noktalarını yapılandırın:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) genişletme yöntemi
* `--urls` Komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* ASP.NET Core yapılandırma sistemi de dahil olmak üzere `ASPNETCORE_URLS` ortam değişkeni

Bu yöntemleri hakkında daha fazla bilgi için bkz: [barındırma](xref:fundamentals/hosting).

**IIS bitiş noktası yapılandırması**

IIS kullanırken, IIS için URL bağlamaları tarafından belirlenen bağlamaları geçersiz kılma `UseUrls`. Daha fazla bilgi için bkz: [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konu.

* * *

### <a name="url-prefixes"></a>URL öneklerini

Kullanırken `UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı, veya `ASPNETCORE_URLS` ortam değişkeni URL öneklerini olabilir aşağıdaki biçimlerden birini.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Yalnızca HTTP URL öneklerini geçerlidir. Kestrel SSL'yi desteklemez kullanarak URL bağlamaları yapılandırılırken `UseUrls`.

* Bağlantı noktası numarası ile birlikte IPv4 adresi

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` tüm IPv4 adreslerine bağlar özel bir durumdur.


* Bağlantı noktası numarası ile birlikte IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` IPv4 IPv6 eşdeğerdir `0.0.0.0`.


* Bağlantı noktası numarası ile ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Ana makine adları, `*`, ve `+`, özel değildir. Herhangi bir şey geçerli bir IP adresi tanınmıyor veya `localhost` tüm IPv4 ve IPv6 IP bağlar. Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak için kullanın [HTTP.sys](xref:fundamentals/servers/httpsys) veya IIS, Nginx ya da Apache gibi bir ters proxy sunucusu.

  > [!WARNING]
  > Etkin bir ters proxy filtreleme ana bilgisayarla kullanmayan etkinleştirirseniz [ana bilgisayar filtre](#host-filtering).

* Ana bilgisayar `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır. İstenen bağlantı noktası başka bir hizmet ya da geri döngü arabirimde tarafından kullanılıyor Kestrel başlatmak başarısız olur. Herhangi bir nedenden dolayı ya da geri döngü arabirimi kullanılabilir olup olmadığını (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı kaydeder.

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Bağlantı noktası numarası ile birlikte IPv4 adresi

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` tüm IPv4 adreslerine bağlar özel bir durumdur.

* Bağlantı noktası numarası ile birlikte IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` IPv4 IPv6 eşdeğerdir `0.0.0.0`.

* Bağlantı noktası numarası ile ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Ana makine adları, `*`, ve `+` özel değildir. Tanınan bir IP adresi olmayan bir şey veya `localhost` tüm IPv4 ve IPv6 IP bağlar. Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak için kullanın [WebListener](xref:fundamentals/servers/weblistener) veya IIS, Nginx ya da Apache gibi bir ters proxy sunucusu.

* Ana bilgisayar `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır. İstenen bağlantı noktası başka bir hizmet ya da geri döngü arabirimde tarafından kullanılıyor Kestrel başlatmak başarısız olur. Herhangi bir nedenden dolayı ya da geri döngü arabirimi kullanılabilir olup olmadığını (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı kaydeder.

* UNIX yuva

  ```
  http://unix:/run/dan-live.sock
  ```

**Bağlantı noktası 0**

Bağlantı noktası numarasını olduğunda `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar. Bağlantı noktasına bağlama `0` herhangi bir ana bilgisayar adı veya IP dışında için izin verilen `localhost`.

Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı uygulama burada ulaşılabilen dinamik bağlantı noktası gösterir:

```console
Now listening on: http://127.0.0.1:48508
```

**SSL için URL öneklerini**

Çağırma varsa `UseHttps` genişletme yöntemi ile URL öneklerini eklediğinizden emin olun `https:`:

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> Aynı bağlantı noktasında HTTPS ve HTTP barındırılamaz.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

* * *

## <a name="host-filtering"></a>Ana bilgisayar filtre

Kestrel gibi ön eklerine göre yapılandırma desteklerken `http://example.com:5000`, Kestrel büyük ölçüde ana bilgisayar adı yok sayar. Ana bilgisayar `localhost` bağlama geri döngü adresleri için kullanılan özel bir durum. Açık bir IP adresi tüm ortak IP adresine bağlar daha herhangi diğer barındırır. Bu bilgilerin hiçbiri isteği doğrulamak için kullanılan `Host` üstbilgileri.

İki geçici çözüm vardır:

* Ana bilgisayar üstbilgisi filtreleme ile ters Ara sunucu arkasındaki ana bilgisayarı. ASP.NET Core Kestrel için desteklenen tek senaryo, bu 1.x.
* Filtre uygulamak için bir ara yazılım istekleri tarafından kullanım `Host` üstbilgi. Bir örnek ara yazılımı aşağıdaki gibidir:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Yukarıdaki kayıt `HostFilteringMiddleware` içinde `Startup.Configure`. Unutmayın [ara yazılım kaydı sıralama](xref:fundamentals/middleware/index#ordering) önemlidir. Kayıt hemen tanılama Ara kayıttan sonra gerçekleşmelidir (örneğin, `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

Önceki Ara bekliyor bir `AllowedHosts` anahtarını *appsettings.\< EnvironmentName > .json*. Bu anahtarın değeri, bağlantı noktası numaralarını olmadan ana bilgisayar adlarını noktalı virgülle ayrılmış bir listesidir. Dahil `AllowedHosts` anahtar-değer çifti *appsettings. Production.JSON*:

```json
{
  "AllowedHosts": "example.com"
}
```

*appSettings. Development.JSON* (localhost yapılandırma dosyası):

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTPS'yi Zorunlu Kılma](xref:security/enforcing-ssl)
* [Kestrel kaynak kodu](https://github.com/aspnet/KestrelHttpServer)
