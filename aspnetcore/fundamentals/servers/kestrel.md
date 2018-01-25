---
title: "ASP.NET Core kestrel web sunucusu uygulaması"
author: tdykstra
description: "ASP.NET Core üzerinde libuv tabanlı için Kestrel, platformlar arası web sunucusu tanıtır."
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e2b28f15e47789ac89213e57396060ee356ee33
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Kestrel web server ASP.NET Core uygulamasında giriş

Tarafından [zel Dykstra](https://github.com/tdykstra), [Chris fillerin](https://github.com/Tratcher), ve [Stephen Halter](https://twitter.com/halter73)

Kestrel olan bir platformlar arası [ASP.NET Core için web sunucusu](index.md) göre [libuv](https://github.com/libuv/libuv), platformlar arası zaman uyumsuz g/ç kitaplığı. Kestrel varsayılan ASP.NET Core proje şablonları olarak dahil edilen web sunucusudur. 

Kestrel aşağıdaki özellikleri destekler:

  * HTTPS
  * Etkinleştirmek için kullanılan opak yükseltme [WebSockets](https://github.com/aspnet/websockets)
  * Yüksek performanslı Nginx arkasında UNIX yuvaları 

Kestrel tüm platformlar ve .NET Core destekleyen sürümler desteklenir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Görüntülemek veya karşıdan 2.x için örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Görüntülemek veya karşıdan 1.x için örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Ters proxy ile Kestrel kullanma zamanı

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters proxy sunucusu*, IIS, Nginx ya da Apache gibi. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel bir ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Her iki yapılandırma &mdash; ile veya bir ters Ara sunucu olmadan &mdash; Kestrel yalnızca bir iç ağa açılırsa de kullanılabilir.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Uygulamanızı bir iç ağ yalnızca gelen istekleri kabul ederse, tek başına Kestrel kullanabilirsiniz.

![Kestrel, iç ağınız ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internal.png)

Uygulamanızı internet kullanıma sunma, IIS, Nginx ya da Apache olarak kullanmalısınız bir *ters proxy sunucu*. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

Güvenlik nedenleriyle (trafiğini Internet'ten gösterilen) kenar dağıtımlar için ters Ara sunucu gereklidir. Kestrel 1.x sürümleri tamamlayıcı saldırılarına karşı savunma yoktur. Bu içerir ancak uygun zaman aşımları, boyut sınırları ve eş zamanlı bağlantı sınırlarını sınırlı değildir.

---

Aynı IP adresini ve bağlantı noktası tek bir sunucu üzerinde çalışan paylaşan birden fazla uygulamanız varsa, ters proxy gerektiren bir senaryodur. Aynı IP adresini ve birden çok işlem arasında bağlantı noktası paylaşımı Kestrel desteklemediğinden, doğrudan Kestrel ile işe yaramaz. Bir bağlantı noktasında dinleyecek şekilde Kestrel yapılandırdığınızda, bu bağlantı noktası ana bilgisayar üstbilgisi bağımsız olarak tüm trafiği işler. Bağlantı noktalarını paylaşabilirsiniz ters bir proxy, ardından, benzersiz bir IP ve bağlantı noktası Kestrel iletmelidir.

Kullanarak bir ters proxy sunucusu gerekli olmasa bile, iyi bir seçimdir diğer nedenleri olabilir:

* Sunulan yüzey alanınızı sınırlayabilirsiniz.
* Yapılandırma ve savunma isteğe bağlı bir ek katmanı sağlar.
* Mevcut altyapısına daha iyi tümleştirme.
* Yük Dengeleme ve SSL kurulumu basitleştirir. Yalnızca ters Ara sunucunuz bir SSL sertifikası gerektirir ve bu sunucu uygulama sunucularınızda düz HTTP kullanarak iç ağ ile iletişim kurabilir.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core uygulamaları Kestrel kullanma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paket dahil [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

ASP.NET Core proje şablonlarını Kestrel varsayılan olarak kullanın. İçinde *Program.cs*, şablonu kodu çağrılarının `CreateDefaultBuilder`, çağıran [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) arka planda.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Kestrel seçeneklerini yapılandırmak gerekirse, çağrı `UseKestrel` içinde *Program.cs* aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Yükleme [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet paketi.

Çağrı [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) genişletme yöntemi `WebHostBuilder` içinde `Main` herhangi belirtme yöntemi [Kestrel seçenekleri](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) sonraki bölümde gösterildiği gibi gereken.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel seçenekleri

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel web sunucusu Internet'e yönelik dağıtımlarda özellikle yararlı kısıtlaması yapılandırma seçenekleri vardır. Ayarlayabileceğiniz sınırları bazıları şunlardır:

- En fazla istemci bağlantıları
- En büyük istek gövdesi boyutu
- En düşük istek gövdesi veri oranı

Bu kısıtlamaların ve diğerleri ayarlamak `Limits` özelliği [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) sınıfı. `Limits` Özelliği bir örneğini tutan [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) sınıfı. 

**En fazla istemci bağlantıları**

Aşağıdaki kod ile tüm uygulama için en fazla eş zamanlı açık TCP bağlantı sayısını ayarlanabilir:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Başka bir protokol (örneğin, WebSockets istek üzerine) için HTTP veya HTTPS yükseltildi bağlantıları için ayrı bir sınır yoktur. Bir bağlantı yükseltildikten sonra onu karşı sayılan değil `MaxConcurrentConnections` sınırı. 

En fazla bağlantı sayısı, varsayılan olarak sınırsız (null) olur.

**En büyük istek gövdesi boyutu**

Varsayılan en büyük istek gövdesi boyutu 30.000.000, hangi 28.6 MB'dir bayttır. 

Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem kullanmaktır [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) bir eylem yönteminin özniteliği:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Aşağıda, tüm uygulama, her istek için kısıtlamayı yapılandırmak nasıl oluşturulduğunu gösteren bir örnek verilmiştir:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Belirli bir istek Ara yazılımında ayarı geçersiz kılabilirsiniz:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırı yapılandırmayı denerseniz özel durum oluşur. Var. bir `IsReadOnly` belirten özelliği, `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

**En düşük istek gövdesi veri oranı**

Veri içinde belirtilen hızda bayt/saniye geliyorsa kestrel saniyede denetler. Hızı en düşerse, bağlantı zaman aşımına. Yetkisiz kullanım süresi Kestrel en düşük kadar gönderme oranını artırmak için istemcinin verir süreyi belirtir; oranı, bu süre boyunca işaretli değil. Yetkisiz kullanım süresi başlangıçta TCP yavaş başlatma nedeniyle yavaş bir hızda veri gönderme bağlantıları bırakarak önlemeye yardımcı olur.

Varsayılan minimum 240 bayt/saniye, 5 saniye yetkisiz kullanım süresi ile hızıdır.

Minimum oran yanıt için de geçerlidir. İstek sınırı ve yanıt sınırı ayarlamak için kod olması dışında aynı olduğundan `RequestBody` veya `Response` özelliği ve arabirim adlarını. 

En az veri hızları yapılandırmak nasıl oluşturulduğunu gösteren bir örnek şudur *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

İstek başına oranları Ara yazılımında yapılandırabilirsiniz:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Aşağıdaki sınıflar diğer Kestrel seçenekleri hakkında daha fazla bilgi için bkz:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Kestrel seçenekleri hakkında daha fazla bilgi için bkz: [KestrelServerOptions sınıfı](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Uç nokta yapılandırması

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`. URL öneklerini ve Kestrel çağırarak üzerinde dinleme bağlantı noktalarını yapılandırma `Listen` veya `ListenUnixSocket` yöntemlere `KestrelServerOptions`. (`UseUrls`, `urls` komut satırı bağımsız değişkeni ve ayrıca iş ASPNETCORE_URLS ortam değişkeni not ettiğiniz sınırlamaları sahip ancak [bu makalenin ilerisinde yer](#useurls-limitations).)

**Bir TCP yuvası bağlama**

`Listen` Yöntemi bir TCP yuvasını bağlar ve seçenekleri lambda bir SSL sertifikası yapılandırmanıza olanak sağlar:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Nasıl Bu örnek SSL için özel bir uç nokta kullanarak yapılandırır fark [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Aynı API belirli uç noktaları diğer Kestrel ayarlarını yapılandırmak için kullanabilirsiniz.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Bir UNIX yuvası bağlama**

Bir UNIX yuvada ile Nginx, Gelişmiş performans için bu örnekte gösterildiği gibi dinleme:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Bağlantı noktası 0**

Bağlantı noktası numarası 0 belirtirseniz, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar. Aşağıdaki örnek, Kestrel gerçekten çalışma zamanında bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls sınırlamaları**

Uç noktaları çağırarak yapılandırabileceğiniz `UseUrls` yöntemi veya kullanarak `urls` komut satırı bağımsız değişkeni veya ASPNETCORE_URLS ortam değişkeni. Bu yöntemler Kestrel dışında sunucularıyla çalışmak için kodunuzu istiyorsanız kullanışlıdır. Ancak, bu sınırlamaları unutmayın:

* Bu yöntemlerle SSL kullanamazsınız.
* Her ikisi de kullanırsanız, `Listen` yöntemi ve `UseUrls`, `Listen` uç noktaları geçersiz kılma `UseUrls` uç noktaları.

**IIS için uç nokta yapılandırması**

IIS kullanırsanız, IIS için URL bağlamaları ya da çağırarak ayarladığınız hiçbir bağlama geçersiz kılma `Listen` veya `UseUrls`. Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`. URL öneklerini ve Kestrel kullanarak üzerinde dinleme bağlantı noktalarını yapılandırabilirsiniz `UseUrls` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni ya da ASP.NET Core yapılandırma sistemi. Bu yöntemler hakkında daha fazla bilgi için bkz: [barındırma](../../fundamentals/hosting.md). Ters proxy IIS kullandığınızda URL bağlama nasıl çalıştığı hakkında daha fazla bilgi için bkz: [ASP.NET Core Modülü](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>URL öneklerini

Çağırırsanız `UseUrls` veya `urls` komut satırı bağımsız değişkeni veya ASPNETCORE_URLS ortam değişkeni, URL öneklerini olabilir aşağıdaki biçimlerden birini. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Yalnızca HTTP URL öneklerini geçerlidir; Kestrel SSL'yi desteklemez kullanarak URL bağlamaları yapılandırdığınızda `UseUrls`.

* Bağlantı noktası numarası ile birlikte IPv4 adresi

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 tüm IPv4 adresleri bağlayan özel bir durumdur.


* Bağlantı noktası numarası ile birlikte IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [:] IPv4, IPv6 aynıdır 0.0.0.0.


* Bağlantı noktası numarası ile ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Ana makine adları, *, ve +, özel değildir. Tanınan bir IP adresi veya "localhost" değil herhangi bir şey tüm IPv4 ve IPv6 IP bağlarsınız. Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak gereksinim duyarsanız kullanın [HTTP.sys](httpsys.md) veya IIS, Nginx ya da Apache gibi bir ters proxy sunucusu.

* Bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte "Localhost" adı

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır. İstenen bağlantı noktası başka bir hizmet ya da geri döngü arabirimde tarafından kullanılıyor Kestrel başlatmak başarısız olur. Herhangi bir nedenden dolayı ya da geri döngü arabirimi kullanılabilir olup olmadığını (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı kaydeder. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Bağlantı noktası numarası ile birlikte IPv4 adresi

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 tüm IPv4 adresleri bağlayan özel bir durumdur.


* Bağlantı noktası numarası ile birlikte IPv6 adresi

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [:] IPv4, IPv6 aynıdır 0.0.0.0.


* Bağlantı noktası numarası ile ana bilgisayar adı

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Ana makine adları, \*, ve + özel değildir. Tanınan bir IP adresi veya "localhost" olmayan herhangi bir şey tüm IPv4 ve IPv6 IP bağlar. Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak gereksinim duyarsanız kullanın [WebListener](weblistener.md) veya IIS, Nginx ya da Apache gibi bir ters proxy sunucusu.

* Bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte "Localhost" adı

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

Bağlantı noktası numarası 0 belirtirseniz, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar. 0 bağlantı noktası bağlama için kullanılabilir, herhangi bir ana bilgisayar adı veya IP dışında `localhost` adı.

Aşağıdaki örnek, Kestrel gerçekten çalışma zamanında bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**SSL için URL öneklerini**

URL öneklerini ile eklediğinizden emin olun `https:` çağırırsanız `UseHttps` genişletme yöntemi, aşağıda gösterildiği gibi.

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

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [2.x için örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel kaynak kodu](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [1.x için örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel kaynak kodu](https://github.com/aspnet/KestrelHttpServer)

---
