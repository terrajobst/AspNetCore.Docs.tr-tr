---
title: ASP.NET Core 'de HTTP. sys Web sunucusu uygulama
author: guardrex
description: Windows üzerinde ASP.NET Core için bir Web sunucusu olan HTTP. sys hakkında bilgi edinin. HTTP. sys çekirdek modu sürücüsü üzerine inşa edilen HTTP. sys, Kestrel için IIS olmadan doğrudan Internet bağlantısı için kullanılabilen bir alternatiftir.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f9e564119604e13bdc48a6c36de7d283c56f68f0
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333810"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 'de HTTP. sys Web sunucusu uygulama

[Tom Dykstra](https://github.com/tdykstra), [Chris](https://github.com/Tratcher), ve [Luke Latham](https://github.com/guardrex) tarafından

[Http. sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) , yalnızca Windows üzerinde çalışan [ASP.NET Core için bir Web sunucusudur](xref:fundamentals/servers/index) . HTTP. sys, [Kestrel](xref:fundamentals/servers/kestrel) Server için bir alternatiftir ve Kestrel tarafından sağlamayan bazı özellikler sunar.

> [!IMPORTANT]
> HTTP. sys [ASP.NET Core modülüyle](xref:host-and-deploy/aspnet-core-module) uyumlu DEĞILDIR ve ııs veya IIS Express ile kullanılamaz.

HTTP. sys aşağıdaki özellikleri destekler:

* [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)
* Bağlantı noktası Paylaşımı
* SNı ile HTTPS
* TLS üzerinden HTTP/2 (Windows 10 veya üzeri)
* Doğrudan dosya iletimi
* Yanıtları Önbelleğe Alma
* WebSockets (Windows 8 veya üzeri)

Desteklenen Windows sürümleri:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>HTTP. sys ne zaman kullanılır

HTTP. sys, şu durumlarda olduğu dağıtımlar için yararlıdır:

* Sunucuyu IIS kullanmadan doğrudan Internet 'e sunmaya gerek vardır.

  ![HTTP. sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

* İç dağıtım, [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)gibi Kestrel 'de kullanılamayan bir özelliği gerektirir.

  ![HTTP. sys doğrudan iç ağla iletişim kurar](httpsys/_static/httpsys-to-internal.png)

HTTP. sys pek çok tür saldırılara karşı koruyan ve tam özellikli bir Web sunucusu için sağlamlık, güvenlik ve ölçeklenebilirlik sağlayan çok sayıda teknolojiden oluşur. IIS, HTTP. sys ' nin üstünde HTTP dinleyicisi olarak çalışır.

## <a name="http2-support"></a>HTTP/2 desteği

Aşağıdaki temel gereksinimler karşılanıyorsa ASP.NET Core uygulamalar için [http/2](https://httpwg.org/specs/rfc7540.html) etkinleştirilir:

* Windows Server 2016/Windows 10 veya üzeri
* [Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı
* TLS 1,2 veya üzeri bağlantı

::: moniker range=">= aspnetcore-2.2"

HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/1.1`.

::: moniker-end

HTTP/2 varsayılan olarak etkindir. HTTP/2 bağlantısı kurulmadıysa, bağlantı HTTP/1.1 'ye geri döner. Windows 'un gelecek bir sürümünde http/2 yapılandırma bayrakları http/2 ' yi HTTP. sys ile devre dışı bırakma özelliği de dahil olmak üzere kullanılabilir olacaktır.

## <a name="kernel-mode-authentication-with-kerberos"></a>Kerberos ile çekirdek modu kimlik doğrulaması

HTTP. sys, Kerberos kimlik doğrulama protokolü ile çekirdek modu kimlik doğrulamasına temsilciler. Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP. sys ile desteklenmez. Makine hesabı, Active Directory alındığı ve kullanıcının kimliğini doğrulamak için istemci tarafından sunucuya iletilen Kerberos belirtecinin/biletinin şifresini çözmek için kullanılmalıdır. Uygulamanın kullanıcısına değil, ana bilgisayar için hizmet asıl adını (SPN) kaydedin.

## <a name="how-to-use-httpsys"></a>HTTP. sys kullanma

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>ASP.NET Core uygulamasını HTTP. sys kullanacak şekilde yapılandırma

::: moniker range="< aspnetcore-3.0"

[Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) ([NuGet.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) kullanılırken proje dosyasındaki bir paket başvurusu gerekli değildir. @No__t-0 metapackage kullanmadığınız durumlarda [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)öğesine bir paket başvurusu ekleyin.

::: moniker-end

Ana bilgisayarı oluştururken gerekli <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions> belirterek <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> genişletme yöntemini çağırın. Aşağıdaki örnek, seçeneklerini varsayılan değerlerine ayarlar:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=4-12)]

::: moniker-end

Ek HTTP. sys yapılandırması, [kayıt defteri ayarları](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)aracılığıyla işlenir.

   **HTTP. sys seçenekleri**

::: moniker range=">= aspnetcore-3.1"

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | @No__t-0 ve `HttpContext.Response.Body` için zaman uyumlu giriş/çıkışa izin verilip verilmeyeceğini denetleyin. | `false` |
| [Authentication. AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Anonim isteklere izin verin. | `true` |
| [Authentication. düzenleri](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | İzin verilen kimlik doğrulama düzenlerini belirtin. Dinleyici elden atılırken önce herhangi bir zamanda değiştirilebilir. Değerler [Authenticationdüzenlerinin numaralandırması](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes)tarafından sağlanır: `Basic`, `Kerberos`, `Negotiate`, `None` ve `NTLM`. | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Uygun üst bilgileri içeren yanıtlar için [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) önbelleğe almayı deneyin. Yanıt `Set-Cookie`, `Vary` veya `Pragma` üst bilgilerini içeremez. @No__t-1 ve bir `shared-max-age` ya da `max-age` değeri ya da `Expires` üst bilgisi içeren `Cache-Control` üst bilgisi içermelidir. | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | En fazla eşzamanlı kabul sayısı. | 5 &times; [ortam. <br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Kabul edilecek eşzamanlı bağlantı sayısı üst sınırı. Sonsuz için `-1` kullanın. Kayıt defterinin makine genelindeki ayarını kullanmak için `null` kullanın. | `null`<br>(makine genelinde<br>ayarlanmasını |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümüne bakın. | 30000000 bayt<br>(~ 28,6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Sıraya alınabilen en fazla istek sayısı. | 1000 |
| `RequestQueueMode` | Bu, sunucunun istek kuyruğunu oluşturma ve yapılandırmadan sorumlu olup olmadığını veya mevcut bir kuyruğa iliştirilmesinin gerekip gerekmediğini belirtir.<br>Mevcut bir kuyruğa eklenirken, mevcut yapılandırma seçeneklerinin çoğu geçerli değildir. | `RequestQueueMode.Create` |
| `RequestQueueName` | HTTP. sys istek kuyruğunun adı. | `null` (anonim kuyruk) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | İstemci bağlantısının kesilmesinden kaynaklanan yanıt gövdesi yazmasının, özel durumlar oluşturması veya normal şekilde tamamlanması gerektiğini belirtin. | `false`<br>(normal olarak) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | HTTP. sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> yapılandırmasını kullanıma sunun ve bu da kayıt defterinde yapılandırılabilir. Varsayılan değerler de dahil olmak üzere her bir ayar hakkında daha fazla bilgi edinmek için API bağlantılarını izleyin:<ul><li>[TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) @no__t-http sunucusu API 'sinin varlık gövdesini etkin tut bağlantısı üzerinde boşaltmasına izin verilir.</li><li>[TimeoutManager. entitybody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) @no__t-istek varlığı gövdesinin gelmesi Için 1 saat izin verilir.</li><li>[TimeoutManager. HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) @no__t-http sunucusu API 'sinin istek üst bilgisini ayrıştırması Için 1 saat izin verilir.</li><li>[TimeoutManager. ıdleconnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) @no__t-boştaki bir bağlantı Için 1 saate izin verilir.</li><li>[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) @no__t-yanıt için en düşük gönderme hızı.</li><li>[TimeoutManager. requestqueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) @no__t-isteğin, istek sırasında, uygulamanın onu seçmeden önce kalmasını sağlamak için Izin verilen süre.</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | HTTP. sys ile kaydetmek için <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> belirtin. En yararlı olan [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), koleksiyona bir ön ek eklemek için kullanılır. Bunlar, dinleyici elden atılıyor öncesinde herhangi bir zamanda değiştirilebilir. |  |

::: moniker-end

::: moniker range="= aspnetcore-3.0"

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | @No__t-0 ve `HttpContext.Response.Body` için zaman uyumlu giriş/çıkışa izin verilip verilmeyeceğini denetleyin. | `false` |
| [Authentication. AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Anonim isteklere izin verin. | `true` |
| [Authentication. düzenleri](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | İzin verilen kimlik doğrulama düzenlerini belirtin. Dinleyici elden atılırken önce herhangi bir zamanda değiştirilebilir. Değerler [Authenticationdüzenlerinin numaralandırması](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes)tarafından sağlanır: `Basic`, `Kerberos`, `Negotiate`, `None` ve `NTLM`. | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Uygun üst bilgileri içeren yanıtlar için [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) önbelleğe almayı deneyin. Yanıt `Set-Cookie`, `Vary` veya `Pragma` üst bilgilerini içeremez. @No__t-1 ve bir `shared-max-age` ya da `max-age` değeri ya da `Expires` üst bilgisi içeren `Cache-Control` üst bilgisi içermelidir. | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | En fazla eşzamanlı kabul sayısı. | 5 &times; [ortam. <br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Kabul edilecek eşzamanlı bağlantı sayısı üst sınırı. Sonsuz için `-1` kullanın. Kayıt defterinin makine genelindeki ayarını kullanmak için `null` kullanın. | `null`<br>(makine genelinde<br>ayarlanmasını |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümüne bakın. | 30000000 bayt<br>(~ 28,6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Sıraya alınabilen en fazla istek sayısı. | 1000 |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | İstemci bağlantısının kesilmesinden kaynaklanan yanıt gövdesi yazmasının, özel durumlar oluşturması veya normal şekilde tamamlanması gerektiğini belirtin. | `false`<br>(normal olarak) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | HTTP. sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> yapılandırmasını kullanıma sunun ve bu da kayıt defterinde yapılandırılabilir. Varsayılan değerler de dahil olmak üzere her bir ayar hakkında daha fazla bilgi edinmek için API bağlantılarını izleyin:<ul><li>[TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) @no__t-http sunucusu API 'sinin varlık gövdesini etkin tut bağlantısı üzerinde boşaltmasına izin verilir.</li><li>[TimeoutManager. entitybody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) @no__t-istek varlığı gövdesinin gelmesi Için 1 saat izin verilir.</li><li>[TimeoutManager. HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) @no__t-http sunucusu API 'sinin istek üst bilgisini ayrıştırması Için 1 saat izin verilir.</li><li>[TimeoutManager. ıdleconnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) @no__t-boştaki bir bağlantı Için 1 saate izin verilir.</li><li>[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) @no__t-yanıt için en düşük gönderme hızı.</li><li>[TimeoutManager. requestqueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) @no__t-isteğin, istek sırasında, uygulamanın onu seçmeden önce kalmasını sağlamak için Izin verilen süre.</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | HTTP. sys ile kaydetmek için <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> belirtin. En yararlı olan [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), koleksiyona bir ön ek eklemek için kullanılır. Bunlar, dinleyici elden atılıyor öncesinde herhangi bir zamanda değiştirilebilir. |  |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | @No__t-0 ve `HttpContext.Response.Body` için zaman uyumlu giriş/çıkışa izin verilip verilmeyeceğini denetleyin. | `true` |
| [Authentication. AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Anonim isteklere izin verin. | `true` |
| [Authentication. düzenleri](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | İzin verilen kimlik doğrulama düzenlerini belirtin. Dinleyici elden atılırken önce herhangi bir zamanda değiştirilebilir. Değerler [Authenticationdüzenlerinin numaralandırması](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes)tarafından sağlanır: `Basic`, `Kerberos`, `Negotiate`, `None` ve `NTLM`. | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Uygun üst bilgileri içeren yanıtlar için [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) önbelleğe almayı deneyin. Yanıt `Set-Cookie`, `Vary` veya `Pragma` üst bilgilerini içeremez. @No__t-1 ve bir `shared-max-age` ya da `max-age` değeri ya da `Expires` üst bilgisi içeren `Cache-Control` üst bilgisi içermelidir. | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | En fazla eşzamanlı kabul sayısı. | 5 &times; [ortam. <br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Kabul edilecek eşzamanlı bağlantı sayısı üst sınırı. Sonsuz için `-1` kullanın. Kayıt defterinin makine genelindeki ayarını kullanmak için `null` kullanın. | `null`<br>(makine genelinde<br>ayarlanmasını |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümüne bakın. | 30000000 bayt<br>(~ 28,6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Sıraya alınabilen en fazla istek sayısı. | 1000 |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | İstemci bağlantısının kesilmesinden kaynaklanan yanıt gövdesi yazmasının, özel durumlar oluşturması veya normal şekilde tamamlanması gerektiğini belirtin. | `false`<br>(normal olarak) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | HTTP. sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> yapılandırmasını kullanıma sunun ve bu da kayıt defterinde yapılandırılabilir. Varsayılan değerler de dahil olmak üzere her bir ayar hakkında daha fazla bilgi edinmek için API bağlantılarını izleyin:<ul><li>[TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) @no__t-http sunucusu API 'sinin varlık gövdesini etkin tut bağlantısı üzerinde boşaltmasına izin verilir.</li><li>[TimeoutManager. entitybody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) @no__t-istek varlığı gövdesinin gelmesi Için 1 saat izin verilir.</li><li>[TimeoutManager. HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) @no__t-http sunucusu API 'sinin istek üst bilgisini ayrıştırması Için 1 saat izin verilir.</li><li>[TimeoutManager. ıdleconnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) @no__t-boştaki bir bağlantı Için 1 saate izin verilir.</li><li>[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) @no__t-yanıt için en düşük gönderme hızı.</li><li>[TimeoutManager. requestqueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) @no__t-isteğin, istek sırasında, uygulamanın onu seçmeden önce kalmasını sağlamak için Izin verilen süre.</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | HTTP. sys ile kaydetmek için <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> belirtin. En yararlı olan [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), koleksiyona bir ön ek eklemek için kullanılır. Bunlar, dinleyici elden atılıyor öncesinde herhangi bir zamanda değiştirilebilir. |  |

::: moniker-end

<a name="maxrequestbodysize"></a>

**MaxRequestBodySize**

Herhangi bir istek gövdesinin bayt olarak izin verilen en büyük boyutu. @No__t-0 olarak ayarlandığında, en büyük istek gövdesi boyutu sınırsızdır. Bu sınırın, her zaman sınırsız olan yükseltilmiş bağlantıları üzerinde hiçbir etkisi yoktur.

Tek bir @no__t için ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem-0, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Uygulama isteği okumayı başlattıktan sonra bir istek üzerindeki sınırı yapılandırmayı denerse bir özel durum oluşur. @No__t-0 özelliği, `MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirtmek için kullanılabilir, yani sınırı yapılandırmak için çok geç.

Uygulamanın istek başına <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> ' ı geçersiz kılması gerekiyorsa, <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature> ' i kullanın:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

Visual Studio kullanıyorsanız, uygulamanın IIS veya IIS Express çalıştıracak şekilde yapılandırılmadığından emin olun.

Visual Studio 'da varsayılan başlatma profili IIS Express içindir. Projeyi konsol uygulaması olarak çalıştırmak için, aşağıdaki ekran görüntüsünde gösterildiği gibi seçili profili el ile değiştirin:

![Konsol uygulaması profilini seçin](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Windows Server 'ı yapılandırma

1. Uygulamanın açmak için bağlantı noktalarını belirleme ve [Windows Güvenlik Duvarı](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) 'Nı veya [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell CMDLET 'ini kullanarak trafiğin http. sys ' ye ulaşmasını sağlamak için güvenlik duvarı bağlantı noktalarını açın. Aşağıdaki komutlarda ve uygulama yapılandırmasında, 443 numaralı bağlantı noktası kullanılır.

1. Bir Azure sanal makinesine dağıtım yaparken, [ağ güvenlik grubundaki](/azure/virtual-machines/windows/nsg-quickstart-portal)bağlantı noktalarını açın. Aşağıdaki komutlarda ve uygulama yapılandırmasında, 443 numaralı bağlantı noktası kullanılır.

1. Gerekirse, X. 509.440 sertifikalarını edinin ve yükler.

   Windows 'da, [New-SelfSignedCertificate PowerShell cmdlet 'ini](/powershell/module/pkiclient/new-selfsignedcertificate)kullanarak otomatik olarak imzalanan sertifikalar oluşturun. Desteklenmeyen bir örnek için bkz [. UpdateIISExpressSSLForChrome. ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).

   Sunucunun **Yerel makinesine** > **Kişisel** deposuna OTOMATIK olarak imzalanan veya CA imzalı sertifikalar yükler.

1. Uygulama [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise, .net core, .NET Framework veya her ikisini de (uygulama .NET Framework hedefleyen bir .NET Core uygulaması ise) yükler.

   * **.Net** Core &ndash; uygulama .NET Core gerektiriyorsa .NET Core [Indirmelerinde](https://dotnet.microsoft.com/download) **.NET Core çalışma zamanı** yükleyicisini edinip çalıştırın. Tam SDK 'Yı sunucuya yüklemeyin.
   * **.NET Framework** &ndash; uygulama .NET Framework gerektiriyorsa, [.NET Framework yükleme kılavuzuna](/dotnet/framework/install/)bakın. Gerekli .NET Framework yüklemesi. En son .NET Framework yükleyicisi [.NET Core İndirmeleri](https://dotnet.microsoft.com/download) sayfasından edinilebilir.

   Uygulama, [kendi içinde bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise, uygulama çalışma zamanını dağıtımda içerir. Sunucuda çerçeve yüklemesi gerekmez.

1. Uygulamada URL 'Leri ve bağlantı noktalarını yapılandırın.

   Varsayılan olarak, ASP.NET Core `http://localhost:5000` ' a bağlanır. URL öneklerini ve bağlantı noktalarını yapılandırmak için seçenekler şunları içerir:

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * `urls` komut satırı bağımsız değişkeni
   * `ASPNETCORE_URLS` ortam değişkeni
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   Aşağıdaki kod örneği, 443 numaralı bağlantı noktasında sunucunun yerel IP adresi @no__t <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> ' ın nasıl kullanılacağını gösterir:

::: moniker range=">= aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/3.x/Program.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/2.x/Program.cs?highlight=6)]

::: moniker-end

   @No__t-0 ' nın bir avantajı, hatalı biçimli ön ekler için hemen bir hata iletisi oluşturulmasından oluşur.

   @No__t-0 ' daki ayarlar `UseUrls` @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 ayarlarını geçersiz kılar. Bu nedenle, `UseUrls`, `urls` ve @no__t 2 ortam değişkeninin bir avantajı, Kestrel ve HTTP. sys arasında geçiş yapmak daha kolay olabilir.

   HTTP. sys, [http sunucusu API UrlPrefix dize biçimlerini](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)kullanır.

   > [!WARNING]
   > Üst düzey joker karakter bağlamaları (`http://*:80/` ve `http://+:80` **) kullanılmamalıdır.** Üst düzey joker karakter bağlamaları uygulama güvenliği güvenlik açıklarını oluşturur. Bu hem güçlü hem de zayıf Joker karakterlere yöneliktir. Joker karakterler yerine açık konak adlarını veya IP adreslerini kullanın. Alt etki alanı joker bağlantısı (örneğin, `*.mysub.com`), tüm üst etki alanını (Bu güvenlik açığı olan `*.com` ' in aksine) kontrol ediyorsanız bir güvenlik riski değildir. Daha fazla bilgi için bkz. [RFC 7230: bölüm 5,4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).

1. Ön ek, sunucudaki URL öneklerini ister.

   HTTP. sys ' yi yapılandırmaya yönelik yerleşik araç *netsh. exe*' dir. *netsh. exe* , URL öneklerini ayırmak ve X. 509.440 sertifikaları atamak için kullanılır. Araç, yönetici ayrıcalıkları gerektirir.

   Uygulamanın URL 'Lerini kaydettirmek için *netsh. exe* aracını kullanın:

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * `<URL>` &ndash; tam Tekdüzen Kaynak Konum Belirleyicisi (URL). Joker karakter bağlama kullanmayın. Geçerli bir ana bilgisayar adı veya yerel IP adresi kullanın. *URL, sondaki eğik çizgi içermelidir.*
   * `<USER>` &ndash; Kullanıcı veya Kullanıcı grubu adını belirtir.

   Aşağıdaki örnekte, sunucunun yerel IP adresi `10.0.0.4` ' dır:

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   Bir URL kaydedildiğinde, araç `URL reservation successfully added` ile yanıt verir.

   Kayıtlı bir URL 'yi silmek için `delete urlacl` komutunu kullanın:

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. X. 509.440 sertifikalarını sunucusuna kaydedin.

   Uygulamanın sertifikalarını kaydetmek için *netsh. exe* aracını kullanın:

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * `<IP>` &ndash; bağlamanın yerel IP adresini belirtir. Joker karakter bağlama kullanmayın. Geçerli bir IP adresi kullanın.
   * `<PORT>` &ndash; bağlama yönelik bağlantı noktasını belirtir.
   * `<THUMBPRINT>` &ndash; X. 509.440 sertifikası parmak izi.
   * `<GUID>` &ndash;, uygulamayı bilgilendirme amacıyla temsil etmek için bir geliştirici tarafından oluşturulan GUID.

   Başvuru amacıyla, GUID 'yi uygulamada bir paket etiketi olarak depolayın:

   * Visual Studio 'da:
     * **Çözüm Gezgini** ' de uygulamaya sağ tıklayıp **Özellikler**' i seçerek uygulamanın proje özelliklerini açın.
     * **Paket** sekmesini seçin.
     * **Etiketler** alanında oluşturduğunuz GUID 'yi girin.
   * Visual Studio kullanmadığınız durumlarda:
     * Uygulamanın proje dosyasını açın.
     * Oluşturduğunuz GUID ile yeni veya mevcut bir `<PropertyGroup>` `<PackageTags>` özelliği ekleyin:

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   Aşağıdaki örnekte:

   * Sunucunun yerel IP adresi `10.0.0.4` ' dır.
   * Çevrimiçi rastgele bir GUID Oluşturucusu `appid` değeri sağlar.

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   Bir sertifika kaydedildiğinde, araç `SSL Certificate successfully added` ile yanıt verir.

   Bir sertifika kaydını silmek için `delete sslcert` komutunu kullanın:

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   *Netsh. exe*için başvuru belgeleri:

   * [Köprü Metni Aktarım Protokolü (HTTP) için Netsh komutları](https://technet.microsoft.com/library/cc725882.aspx)
   * [UrlPrefix dizeleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. Uygulamayı çalıştırın.

   1024 'den büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak localhost 'a bağlanırken uygulamayı çalıştırmak için yönetici ayrıcalıklarına gerek yoktur. Diğer yapılandırmalarda (örneğin, yerel bir IP adresi veya 443 numaralı bağlantı noktasına bağlama), uygulamayı yönetici ayrıcalıklarıyla çalıştırın.

   Uygulama, sunucunun genel IP adresinde yanıt verir. Bu örnekte, sunucuya `104.214.79.47` ' a ait genel IP adresinde Internete ulaşıldı.

   Bu örnekte bir geliştirme sertifikası kullanılır. Tarayıcının güvenilmeyen sertifika uyarısı atlandıktan sonra sayfa güvenli bir şekilde yüklenir.

   ![Uygulamanın dizin sayfasını yüklü olarak gösteren tarayıcı penceresi](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy sunucusu ve yük dengeleyici senaryoları

Internet veya şirket ağından gelen isteklerle etkileşime geçen HTTP. sys tarafından barındırılan uygulamalar için, proxy sunucularının ve yük dengeleyiciler 'nin arkasında barındırılırken ek yapılandırma gerekebilir. Daha fazla bilgi için bkz. [proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP. sys ile Windows kimlik doğrulamasını etkinleştirme](xref:security/authentication/windowsauth#httpsys)
* [HTTP Sunucusu API 'SI](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [ASPNET/HttpSysServer GitHub deposu (kaynak kodu)](https://github.com/aspnet/HttpSysServer/)
* [Ana bilgisayar](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
