---
title: ASP.NET core'da kestrel web sunucusu uygulaması
author: rick-anderson
description: Kestrel'i, ASP.NET Core için platformlar arası web sunucusu hakkında bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 62649351271deebcf1ed9d2f8b2258bed3478989
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126949"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET core'da kestrel web sunucusu uygulaması

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), ve [Stephen Halter](https://twitter.com/halter73)

Kestrel'i olduğu bir platformlar arası [ASP.NET Core web sunucusu](xref:fundamentals/servers/index). Kestrel'i ASP.NET Core proje şablonları, varsayılan olarak bulunan bir web sunucusudur.

Kestrel'i aşağıdaki özellikleri destekler:

* HTTPS
* Donuk yükseltme etkinleştirmek için kullanılan [WebSockets](https://github.com/aspnet/websockets)
* Yüksek performans Ngınx arkasında UNIX yuva

Kestrel'i tüm platformlarda ve .NET Core destekleyen sürümler desteklenir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Ne zaman Kestrel ters Ara sunucu ile kullanılır.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters Ara sunucu*IIS, Ngınx veya Apache gibi. Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel'i ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Bir uygulama yalnızca bir iç ağ gelen istekleri kabul ederse Kestrel doğrudan uygulamanın sunucusu olarak kullanılabilir.

![Kestrel'i doğrudan iç ağınıza ile iletişim kurar.](kestrel/_static/kestrel-to-internal.png)

Uygulamanıza Internet kullanıma sunma, IIS, Ngınx veya Apache olarak kullanın. bir *ters Ara sunucu*. Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Güvenlik nedenleriyle edge dağıtımları (trafiği Internet'ten kullanıma sunulur) için ters Ara sunucu gereklidir. Kestrel'i 1.x sürümlerini tamamlayıcı uygun zaman aşımları, boyut sınırları ve eş zamanlı bağlantı sınırları gibi saldırılara karşı savunma yoktur.

---

Aynı IP adresini ve bağlantı noktası tek bir sunucu üzerinde çalışan paylaşan birden çok uygulama olduğunda bir ters proxy senaryosu bulunmaktadır. Aynı IP adresini ve bağlantı noktası arasında birden çok işlem paylaşımı Kestrel desteklemediğinden bu senaryo kestrel desteklemiyor. Kestrel'i bir bağlantı noktasında dinleyecek şekilde yapılandırıldığında, Kestrel tüm istekleri ana bilgisayar üstbilgisi bağımsız olarak bu bağlantı noktası trafiğini işler. Bağlantı noktalarını paylaşan bir ters proxy Kestrel benzersiz bir IP ve bağlantı noktası isteklerini iletmek için özelliğine sahiptir.

Ters proxy sunucusu gerekli olmasa bile bir ters proxy sunucusu kullanarak iyi bir seçim olabilir:

* Bu, barındırdığı uygulamaları kullanıma sunulan ortak yüzey alanını sınırlayabilirsiniz.
* Bu, ek bir yapılandırma ve koruma katmanı sağlar.
* Daha iyi var olan altyapınızla tümleştirebilirsiniz.
* Yük Dengeleme ve SSL yapılandırmasını kolaylaştırır. Ters proxy sunucusu yalnızca bir SSL sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak iç ağ üzerinde uygulama sunucularınız ile iletişim kurabilir.

> [!WARNING]
> Etkin bir ters proxy filtreleme konakla kullanılmıyorsa [filtreleme konak](#host-filtering) etkinleştirilmesi gerekir.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core uygulamalarında Kestrel kullanma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketinde [Microsoft.AspNetCore.App metapackage] (xref:fundamentals / metapackage uygulama) (ASP.NET Core 2.1 veya üzeri).

ASP.NET Core proje şablonları, varsayılan olarak Kestrel kullanın. İçinde *Program.cs*, şablon kod çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), çağıran [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) arka planda.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Yükleme [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet paketi.

Çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) genişletme yöntemini [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) içinde `Main` herhangi belirtme yöntemi [Kestrel seçenekleri](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) , sonraki bölümde gösterildiği gibi gerekli.

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel'i seçenekleri

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Kestrel'i web sunucusu Internet'e yönelik dağıtımlarda özellikle yararlı olan kısıtlaması yapılandırma seçenekleri vardır. Özelleştirilebilir birkaç önemli sınırları:

* En fazla istemci bağlantısı
* En fazla istek gövdesi boyutu
* En az bir istek gövdesi veri hızı

Bunlar ve diğer kısıtlamaları ayarlamak [sınırları](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) özelliği [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) sınıfı. `Limits` Özelliği bir örneğini tutan [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) sınıfı.

**En fazla istemci bağlantısı**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

Aşağıdaki kod ile tüm uygulama için eşzamanlı açık TCP bağlantıları sayısı ayarlanabilir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

HTTP veya HTTPS, başka bir protokol (örneğin, WebSockets istek üzerine) a yükseltilmiştir bağlantıları için ayrı bir sınır yoktur. Bir bağlantı yükseltildikten sonra karşı sayılır değil `MaxConcurrentConnections` sınırı.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

Bağlantı sayısı, varsayılan olarak sınırsız (null) olur.

**En fazla istek gövdesi boyutu**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

Varsayılan en büyük istek gövdesi boyutu 30.000.000, yaklaşık 28.6 MB bayttır.

Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşımdır [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) özniteliği bir eylem yöntemi:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Her istek için uygulama kısıtlama yapılandırma gösteren bir örnek aşağıda verilmiştir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

Belirli bir istekte Ara yazılımında ayarı geçersiz kılabilirsiniz:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

Uygulama isteği okumak başlatıldıktan sonra bir istekte sınırını yapılandırmak çalışırsanız, bir özel durum oluşturulur. Var. bir `IsReadOnly` gösterir özelliği `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

**En az bir istek gövdesi veri hızı**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Veri içinde belirtilen hızda bayt/saniye gelen, saniyede kestrel denetler. Hızı en düşerse, bağlantının zaman aşımına uğradı. Yetkisiz kullanım süresi Kestrel en düşük kadar gönderme hızını artırmak için istemci verdiğini süreyi belirtir; Bu süre boyunca oranı işaretlenmemiş. Yetkisiz kullanım süresi, başlangıçta TCP yavaş başlatma nedeniyle yavaş bir hızda veri gönderen bağlantıları verilmemesini yardımcı olur.

Varsayılan en az 5 saniye yetkisiz kullanım süresi ile 240 bayt/saniye oranıdır.

En düşük bir ücretle yanıt için de geçerlidir. İstek sınırı ve yanıt sınırı ayarlamak için kod olması dışında aynıdır `RequestBody` veya `Response` özelliği ve arabirimi adları.

En az veriyi hızları yapılandırma gösteren bir örnek aşağıdadır *Program.cs*:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

İstek başına ücretler Ara yazılımında yapılandırabilirsiniz:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

Diğer seçenekleri Kestrel ve sınırları hakkında daha fazla bilgi için bkz:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Kestrel'i seçenekleri ve sınırları hakkında daha fazla bilgi için bkz:

* [KestrelServerOptions sınıfı](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a>Uç nokta yapılandırması

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. Çağrı [dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) veya [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) yöntemlerde [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) Kestrel için URL ön ekleri ve bağlantı noktalarını yapılandırmak için. `UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak daha sonra bu bölümde belirtilen kısıtlamalara sahip.

`urls` Ana bilgisayar yapılandırma anahtarı konak yapılandırması, uygulama yapılandırmasını gelmelidir. Ekleme bir `urls` anahtarı ve değeri *appsettings.json* konak yapılandırması, yapılandırma dosyasından okunur zamanı tarafından tamamen başlatılmış olduğundan ana bilgisayar yapılandırması etkilemez. Ancak, bir `urls` anahtarını *appsettings.json* kullanılabilir [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) konağı yapılandırmak için konak oluşturucu üzerinde:

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

Varsayılan olarak, ASP.NET Core bağlar:

* `http://localhost:5000`
* `https://localhost:5001` (yerel geliştirme sertifikası mevcut olduğunda)

Bir geliştirme sertifikası oluşturulur:

* Zaman [.NET Core SDK'sı](/dotnet/core/sdk) yüklenir.
* [Dev-certs aracını](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.

Bazı tarayıcılar, tarayıcının yerel geliştirme sertifikasına güvenmek açık izin vermenizi gerektirir.

ASP.NET Core 2.1 ve üzeri proje şablonları varsayılan olarak HTTPS üzerinde çalışır ve dahil etmek için uygulamaları yapılandırma [HTTPS yeniden yönlendirmesi ve HSTS desteği](xref:security/enforcing-ssl).

Çağrı [dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) veya [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) yöntemlerde [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) Kestrel için URL ön ekleri ve bağlantı noktalarını yapılandırmak için.

`UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır ancak daha sonra (bir varsayılan sertifika için HTTPS uç noktasının kullanılabilir olmalıdır, bu bölümde belirtilen kısıtlamalara sahip Yapılandırma).

ASP.NET Core 2.1 `KestrelServerOptions` yapılandırma:

**ConfigureEndpointDefaults (Eylem&lt;ListenOptions&gt;)**  
Bir yapılandırma belirtir `Action` belirtilen her uç nokta için çalıştırılacak. Çağırma `ConfigureEndpointDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.

**ConfigureHttpsDefaults (Eylem&lt;HttpsConnectionAdapterOptions&gt;)**  
Bir yapılandırma belirtir `Action` her HTTPS uç noktası için çalıştırılacak. Çağırma `ConfigureHttpsDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.

**Configure(IConfiguration)**  
Alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) giriş olarak. Yapılandırma için yapılandırma bölümü için Kestrel kapsamlandırılmalıdır.

**ListenOptions.UseHttps**  
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

Kullanarak URL'leri belirtin:

* `ASPNETCORE_URLS` ortam değişkeni.
* `--urls` komut satırı bağımsız değişkeni.
* `urls` ana bilgisayar yapılandırma anahtarı.
* `UseUrls` genişletme yöntemi.

Daha fazla bilgi için [sunucu URL'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırmasını](xref:fundamentals/host/web-host#override-configuration).

Bu yaklaşımları kullanarak sağlanan değer, bir veya daha fazla HTTP ve HTTPS uç noktası (varsayılan sertifika varsa HTTPS) olabilir. Değer noktalı virgülle ayrılmış listesini yapılandırın (örneğin, `"Urls": "http://localhost:8000;http://localhost:8001"`).

*Varsayılan Sertifika yapılandırmasından değiştirin*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Kestrel yapılandırmasını varsayılan olarak. Bir varsayılan HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir. Disk üzerindeki bir dosyadan veya bir sertifika deposu kullanmak için URL'leri ve sertifikalar dahil olmak üzere birden fazla uç noktası yapılandırın.

Aşağıdaki *appsettings.json* örneği:

* Ayarlama **AllowInvalid** için `true` geçersiz sertifikaları (örneğin, otomatik olarak imzalanan sertifikalar) izin verilir.
* Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (**HttpsDefaultCert** örnekte) bölümünde tanımlanan sertifika için geri döner **sertifikaları** > **varsayılan**  veya geliştirme sertifikası.

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

Kullanmaya alternatif **yolu** ve **parola** herhangi bir sertifikayı sertifika deposuna alanlarını kullanarak sertifikasını belirtmek için düğümüdür. Örneğin, **sertifikaları** > **varsayılan** olarak sertifika belirtilebilir:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
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
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` döndürür bir `KestrelConfigurationLoader` ile bir `.Endpoint(string name, options => { })` yapılandırılmış bir uç noktanın ayarları desteklemek için kullanılan yöntemi:

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Ayrıca doğrudan erişebilirsiniz `KestrelServerOptions.ConfigurationLoader` tarafından sağlanan gibi mevcut yükleyiciyi yineleme tutmak için [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

* Her uç nokta yapılandırma bölümü bir seçenekler kullanılabilir `Endpoint` yöntemi böylece özel ayarlarını okuyun.
* Birden fazla yapılandırması çağırarak yüklenmemiş olabilir `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` yeniden başka bir bölüme sahip. Sürece yalnızca son yapılandırma kullanıldığını `Load` önceki örnekleri üzerinde açıkça çağrılır. Metapackage çağrı değil `Load` böylece kendi varsayılan yapılandırma bölümü değiştirilebilir.
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

**Bir TCP yuva için bağlama**

[Dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) yöntemi için bir TCP yuva bağlar ve SSL sertifika yapılandırma seçenekleri lambda verir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

Örnek, bir uç nokta için SSL yapılandırır [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Özel uç noktaları diğer Kestrel ayarlarını yapılandırmak için aynı API kullanın.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**Bir UNIX yuvası için bağlama**

Bir UNIX yuvasıyla dinleyecek [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) Bu örnekte gösterildiği gibi Ngınx ile Gelişmiş performans için:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

**Bağlantı noktası 0**

Zaman bağlantı noktası numarasını `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar. Aşağıdaki örnek, Kestrel çalışma zamanında gerçekten bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı dinamik bağlantı noktası uygulama nereden ulaşabileceğinizi gösterir:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

**UseUrls,--URL'leri komut satırı bağımsız değişkeni, URL ana bilgisayar yapılandırma anahtarı ve ASPNETCORE_URLS ortam değişkeni sınırlamaları**

Uç noktaları ile aşağıdaki yaklaşımlardan yapılandırın:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* `--urls` komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* `ASPNETCORE_URLS` ortam değişkeni

Bu yöntemler, kod Kestrel dışında sunucuları ile iş yapmak için kullanışlıdır. Ancak, bu sınırlamaları unutmayın:

* SSL HTTPS uç nokta yapılandırmasında bir varsayılan sertifika sağlanmadığı sürece bu yaklaşımların ile kullanılamaz (örneğin, kullanarak `KestrelServerOptions` yapılandırma veya bu konuda daha önce gösterildiği gibi bir yapılandırma dosyası).
* Hem `Listen` ve `UseUrls` yaklaşımları eşzamanlı olarak kullanılan `Listen` uç noktaları geçersiz kılma `UseUrls` uç noktaları.

**IIS bitiş noktası yapılandırması**

IIS geçersiz kılmak için IIS, URL bağlamaları kullanırken bağlamalar tarafından ayarlanan `Listen` veya `UseUrls`. Daha fazla bilgi için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konu.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. URL ön ekleri ve Kestrel kullanarak bağlantı noktalarını yapılandırın:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) genişletme yöntemi
* `--urls` komut satırı bağımsız değişkeni
* `urls` ana bilgisayar yapılandırma anahtarı
* ASP.NET Core yapılandırma sistemi dahil olmak üzere `ASPNETCORE_URLS` ortam değişkeni

Bu yöntemler hakkında daha fazla bilgi için bkz. [barındırma](xref:fundamentals/host/index).

**IIS bitiş noktası yapılandırması**

IIS kullanırken, IIS için URL bağlamaları bağlamaları belirlediği geçersiz kılma `UseUrls`. Daha fazla bilgi için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konu.

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Aktarım yapılandırma

ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda göre. Bu, bir ASP.NET Core 2.0 uygulamaları çağıran 2.1 yükseltme için değişiklik [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) ve aşağıdaki paketlerden birini bağlıdır:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket Başvurusu)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

ASP.NET Core 2.1 veya üzerini kullanan projeleri [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ve Libuv kullanılmasını gerektirir:

* İçin bağımlılık ekleme [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) uygulamanın proje dosyasına paket:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* Çağrı [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):

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

::: moniker-end

### <a name="url-prefixes"></a>URL ön ekleri

Kullanırken `UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı veya `ASPNETCORE_URLS` ortam değişkeni URL ön ekleri olabilir aşağıdaki biçimlerden birini.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Yalnızca HTTP URL ön ekleri geçerlidir. Kullanarak URL bağlamaları yapılandırma sırasında kestrel SSL'yi desteklemez `UseUrls`.

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
  > Etkin bir ters proxy filtreleme konakla kullanmayan etkinleştirirseniz [filtreleme konak](#host-filtering).

* Konak `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır. İstenen bağlantı noktası başka bir hizmette ya da geri döngü arabirimine tarafından kullanılıyor Kestrel başlatmak başarısız olur. Ya da geri döngü arabirimine başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı günlüğe kaydeder.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Bağlantı noktası numarası ile IPv4 adresi

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` tüm IPv4 adreslerine bağlayan bir özel durumdur.

* Bağlantı noktası numarası ile IPv6 adresi

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

  Ana makine adları, `*`, ve `+` özel değildir. Tanınan bir IP adresi olmayan herhangi bir şey veya `localhost` tüm IPv4 ve IPv6 IP bağlar. Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak için kullanın [WebListener](xref:fundamentals/servers/weblistener) veya IIS, Ngınx veya Apache gibi bir ters proxy sunucusu.

* Konak `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır. İstenen bağlantı noktası başka bir hizmette ya da geri döngü arabirimine tarafından kullanılıyor Kestrel başlatmak başarısız olur. Ya da geri döngü arabirimine başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı günlüğe kaydeder.

* UNIX yuva

  ```
  http://unix:/run/dan-live.sock
  ```

**Bağlantı noktası 0**

Bağlantı noktası numarasını olduğunda `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar. Bağlantı noktası bağlama `0` herhangi bir ana bilgisayar adı veya IP dışında için izin verilen `localhost`.

Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı dinamik bağlantı noktası uygulama nereden ulaşabileceğinizi gösterir:

```console
Now listening on: http://127.0.0.1:48508
```

**SSL için URL ön ekleri**

Çağırma varsa `UseHttps` genişletme yöntemi ile URL ön ekleri eklediğinizden emin olun `https:`:

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

---

## <a name="host-filtering"></a>Konak filtreleme

Kestrel'i yapılandırma gibi önekleri temel alarak desteklerken `http://example.com:5000`, Kestrel büyük ölçüde ana bilgisayar adı yok sayar. Konak `localhost` bağlama geri döngü adresi için kullanılan özel bir durum. Daha açık bir IP adresi tüm genel IP adresine bağlar herhangi diğer barındırın. Bu bilgilerin hiçbiri isteği doğrulamak için kullanılan `Host` üstbilgileri.

::: moniker range="< aspnetcore-2.0"

Geçici bir çözüm olarak, ana bilgisayar üst bilgisi filtrelemeyle ters Ara sunucu arkasındaki barındırın. ASP.NET core'da Kestrel için desteklenen tek senaryo budur 1.x.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Geçici çözüm olarak, istekleri filtrelemek için Ara yazılımları kullanmayı `Host` üst bilgi:

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

Önceki kaydetme `HostFilteringMiddleware` içinde `Startup.Configure`. Unutmayın [ara yazılımı kaydı sıralama](xref:fundamentals/middleware/index#ordering) önemlidir. Kayıt hemen tanılama ara yazılım kayıttan sonra gerçekleşmesi (örneğin, `app.UseExceptionHandler`).

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

Ara yazılım bekliyor bir `AllowedHosts` anahtarını *appsettings.json*/*appsettings.\< EnvironmentName > .json*. Bağlantı noktası numaraları olmadan konak adları, noktalı virgülle ayrılmış listesini değerdir:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Geçici çözüm olarak, konak filtreleme ara yazılımı kullanın. Konak filtreleme ara yazılımı tarafından sağlanan [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) dahil edilen paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri). Ara yazılım tarafından eklenen [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), çağıran [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Konak filtreleme ara yazılım, varsayılan olarak devre dışıdır. Ara yazılım etkinleştirmek için tanımladığınız bir `AllowedHosts` anahtarını *appsettings.json*/*appsettings.\< EnvironmentName > .json*. Bağlantı noktası numaraları olmadan konak adları, noktalı virgülle ayrılmış listesini değerdir:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appSettings.JSON*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Üst bilgileri ara yazılım iletilen](xref:host-and-deploy/proxy-load-balancer) de sahip bir [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) seçeneği. İletilen üstbilgileri ara yazılım ve konak filtreleme ara yazılım farklı senaryolar için benzer bir işlevsellik vardır. Ayar `AllowedHosts` iletilen üstbilgileri Ara yazılımla barındırma üst bilgisi bir tersine Ara sunucunun istekleri iletirken korunmaz zaman uygun yük dengeleyici mı. Ayarı `AllowedHosts` Kestrel bir uç sunucusu kullanıldığında veya ne zaman ana bilgisayar üst bilgisini doğrudan iletilen konak filtreleme Ara yazılımla uygundur.
>
> İletilen üstbilgileri ara yazılım hakkında daha fazla bilgi için bkz. [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTPS'yi Zorunlu Kılma](xref:security/enforcing-ssl)
* [Kestrel'i kaynak kodu](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: İleti söz dizimi ve yönlendirme (Bölüm 5.4: ana bilgisayar)](https://tools.ietf.org/html/rfc7230#section-5.4)
* [ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma](xref:host-and-deploy/proxy-load-balancer)
