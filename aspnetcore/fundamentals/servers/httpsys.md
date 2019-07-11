---
title: ASP.NET core'da HTTP.sys web sunucusu uygulaması
author: guardrex
description: HTTP.sys, ASP.NET Core, Windows için bir web sunucusu hakkında bilgi edinin. HTTP.sys çekirdek modu sürücüsü üzerinde oluşturulmuş, HTTP.sys için IIS olmadan İnternet'e doğrudan bağlantı için kullanılan Kestrel alternatiftir.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/20/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 3f48cda8f89d3f3dcce220cad4e405472b510df4
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814884"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET core'da HTTP.sys web sunucusu uygulaması

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), ve [Luke Latham](https://github.com/guardrex)

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) olduğu bir [ASP.NET Core web sunucusu](xref:fundamentals/servers/index) Windows üzerinde yalnızca çalışır. HTTP.sys olan alternatif [Kestrel](xref:fundamentals/servers/kestrel) sunucu ve teklifler bazı özellikleri Kestrel sağlamaz.

> [!IMPORTANT]
> HTTP.sys ile uyumlu olmayan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve IIS veya IIS Express ile kullanılamaz.

HTTP.sys aşağıdaki özellikleri destekler:

* [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)
* Bağlantı noktası paylaşma
* SNI ile HTTPS
* HTTP/2 üzerinden TLS (Windows 10 veya üzeri)
* Doğrudan bir dosya aktarımı
* Yanıtları Önbelleğe Alma
* WebSockets (Windows 8 veya üzeri)

Desteklenen Windows sürümleri:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>HTTP.sys kullanıldığı durumlar

HTTP.sys dağıtımları için yararlıdır burada:

* IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak için bir gereksinim yoktur.

  ![HTTP.sys doğrudan Internet ile iletişim kurar.](httpsys/_static/httpsys-to-internet.png)

* Bir iç dağıtım Kestrel içinde kullanılabilir değil bir özellik gibi gerektirir [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

  ![HTTP.sys iç ağa ile doğrudan iletişim kurar.](httpsys/_static/httpsys-to-internal.png)

HTTP.sys, birçok türde saldırılara karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir. IIS'nin, HTTP.sys üzerine bir HTTP dinleyicisi olarak çalışır.

## <a name="http2-support"></a>HTTP/2 desteği

[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki gereksinimleri dayandırırsanız ASP.NET Core uygulamaları karşılanması için etkin:

* Windows Server 2016/Windows 10 veya üzeri
* [Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı
* TLS 1.2 veya sonraki bir bağlantı

::: moniker range=">= aspnetcore-2.2"

Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.

::: moniker-end

HTTP/2 varsayılan olarak etkindir. Bir HTTP/2 bağlantı değil, bağlantı, HTTP/1.1 geri döner. Gelecekteki bir Windows sürümünde, HTTP/2 yapılandırma bayrakları HTTP/2 HTTP.sys ile devre dışı bırakma olanağı dahil olmak üzere, kullanılabilir.

## <a name="kernel-mode-authentication-with-kerberos"></a>Çekirdek modu kimlik doğrulamasını Kerberos ile

Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder. Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez. Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir. Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.

## <a name="how-to-use-httpsys"></a>HTTP.sys kullanma

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>HTTP.sys kullanmak için ASP.NET Core uygulamasını yapılandırma

1. Proje dosyasındaki bir paket başvurusu kullanırken gerekli değildir [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 veya üzeri). Değil kullanırken `Microsoft.AspNetCore.App` metapackage, paket başvurusu ekleme [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> gerekli belirtme konak oluştururken genişletme yöntemi <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>. Aşağıdaki örnek, varsayılan değerlerine seçeneklerini ayarlar:

::: moniker range=">= aspnetcore-3.0"

   ```csharp
   public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .UseStartup<Startup>()
          .UseHttpSys(options =>
          {
              options.AllowSynchronousIO = false;
              options.Authentication.Schemes = AuthenticationSchemes.None;
              options.Authentication.AllowAnonymous = true;
              options.MaxConnections = null;
              options.MaxRequestBodySize = 30000000;
              options.UrlPrefixes.Add("http://localhost:5000");
          });
   ```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

::: moniker-end

   Ek HTTP.sys yapılandırma aracılığıyla işlenir [kayıt defteri ayarları](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

   **HTTP.sys seçenekleri**

::: moniker range=">= aspnetcore-3.0"

   | Özellik | Açıklama | Varsayılan |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | Zaman uyumlu giriş/çıkış için izin verilip verilmediğini `HttpContext.Request.Body` ve `HttpContext.Response.Body`. | `false` |
   | [Authentication.AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Anonim isteklere izin verin. | `true` |
   | [Authentication.Schemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | İzin verilen kimlik doğrulama şemasını belirtin. Dinleyici disposing önce herhangi bir zamanda değiştirilebilir. Değerleri tarafından sağlanan [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, ve `NTLM`. | `None` |
   | [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Girişimi [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) uygun üst bilgilerini içeren yanıtlar için önbelleğe alma. Yanıta dahil `Set-Cookie`, `Vary`, veya `Pragma` üstbilgileri. İçermesi gerekir bir `Cache-Control` üst bilgisi bu `public` ve ya da bir `shared-max-age` veya `max-age` değeri veya bir `Expires` başlığı. | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | En fazla eşzamanlı sayısı kabul eder. | 5 &times; [ortam.<br> ProcessorCount](xref:System.Environment.ProcessorCount) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Kabul etmek üzere kullanılan eşzamanlı bağlantı sayısı. Kullanım `-1` sonsuz için. Kullanma `null` kayıt defterinin makine genelindeki ayarını kullanmak için. | `null`<br>(sınırsız) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | Bkz: <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümü. | 30000000 bayt<br>(~28.6 MB) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Sıraya alınabilecek isteklerinin sayısı. | 1000 |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | Yanıt gövdesi yazma, hata nedeniyle istemci bağlantısını keser veya özel durumlar genellikle tamamlamak gösterir. | `false`<br>(normalde Tamamla) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | HTTP.sys kullanıma <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> yapılandırma, kayıt defterinde da yapılandırılabilir. Varsayılan değerleri dahil olmak üzere, her ayar hakkında daha fazla bilgi için API bağlantıları izleyin:<ul><li>[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP Sunucusu API varlık gövdesini bir canlı bağlantı boşaltma zaman izin verilir.</li><li>[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; ulaşması istek Varlık gövdesi için izin verilen süresi.</li><li>[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; süresi izin verilen HTTP Sunucusu API istek üstbilgisi ayrıştırılamıyor.</li><li>[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; boştaki bir bağlantının için izin verilen süresi.</li><li>[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; en düşük yanıt hızı gönderin.</li><li>[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; zaman izin isteğinin, uygulamayı alır önce isteği kuyruğunda kalır.</li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | Belirtin <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> HTTP.sys ile kaydetmek için. Kullanışlı [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), bir önek koleksiyona eklemek için kullanılır. Bu dinleyici disposing önce herhangi bir zamanda değiştirilebilir. |  |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

   | Özellik | Açıklama | Varsayılan |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | Zaman uyumlu giriş/çıkış için izin verilip verilmediğini `HttpContext.Request.Body` ve `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Anonim isteklere izin verin. | `true` |
   | [Authentication.Schemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | İzin verilen kimlik doğrulama şemasını belirtin. Dinleyici disposing önce herhangi bir zamanda değiştirilebilir. Değerleri tarafından sağlanan [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, ve `NTLM`. | `None` |
   | [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Girişimi [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) uygun üst bilgilerini içeren yanıtlar için önbelleğe alma. Yanıta dahil `Set-Cookie`, `Vary`, veya `Pragma` üstbilgileri. İçermesi gerekir bir `Cache-Control` üst bilgisi bu `public` ve ya da bir `shared-max-age` veya `max-age` değeri veya bir `Expires` başlığı. | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | En fazla eşzamanlı sayısı kabul eder. | 5 &times; [ortam.<br> ProcessorCount](xref:System.Environment.ProcessorCount) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Kabul etmek üzere kullanılan eşzamanlı bağlantı sayısı. Kullanım `-1` sonsuz için. Kullanma `null` kayıt defterinin makine genelindeki ayarını kullanmak için. | `null`<br>(sınırsız) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | Bkz: <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümü. | 30000000 bayt<br>(~28.6 MB) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Sıraya alınabilecek isteklerinin sayısı. | 1000 |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | Yanıt gövdesi yazma, hata nedeniyle istemci bağlantısını keser veya özel durumlar genellikle tamamlamak gösterir. | `false`<br>(normalde Tamamla) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | HTTP.sys kullanıma <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> yapılandırma, kayıt defterinde da yapılandırılabilir. Varsayılan değerleri dahil olmak üzere, her ayar hakkında daha fazla bilgi için API bağlantıları izleyin:<ul><li>[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP Sunucusu API varlık gövdesini bir canlı bağlantı boşaltma zaman izin verilir.</li><li>[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; ulaşması istek Varlık gövdesi için izin verilen süresi.</li><li>[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; süresi izin verilen HTTP Sunucusu API istek üstbilgisi ayrıştırılamıyor.</li><li>[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; boştaki bir bağlantının için izin verilen süresi.</li><li>[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; en düşük yanıt hızı gönderin.</li><li>[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; zaman izin isteğinin, uygulamayı alır önce isteği kuyruğunda kalır.</li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | Belirtin <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> HTTP.sys ile kaydetmek için. Kullanışlı [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), bir önek koleksiyona eklemek için kullanılır. Bu dinleyici disposing önce herhangi bir zamanda değiştirilebilir. |  |

::: moniker-end

   <a name="maxrequestbodysize"></a>

   **MaxRequestBodySize**

   Bayt cinsinden izin verilen maksimum herhangi bir istek gövdesi boyutu. Ayarlandığında `null`, en fazla istek gövdesi boyutu sınırsızdır. Bu sınırı her zaman sınırsız yükseltilen bağlantılar üzerinde etkisi yoktur.

   Bir ASP.NET Core MVC uygulaması için tek bir sınırı geçersiz kılmak için önerilen yöntem `IActionResult` kullanmaktır <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliği bir eylem yöntemi:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırını yapılandırmak uygulamanın çalışırsa, bir özel durum oluşturulur. Bir `IsReadOnly` özelliği belirtmek için kullanılabilir `MaxRequestBodySize` özelliği olan bir salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

   Uygulamayı'ı geçersiz kılmalıdır varsa <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> istek başına, kullanın <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. Visual Studio kullanıyorsanız, uygulamayı IIS veya IIS Express çalıştırmak için yapılandırılmamış emin olun.

   Visual Studio'da varsayılan başlatma için IIS Express profilidir. Proje bir konsol uygulaması çalıştırmak için el ile seçilen profil, aşağıdaki ekran görüntüsünde gösterildiği gibi değiştirin:

   ![Konsol uygulama profili seçin](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Windows Server'ı yapılandırma

1. Uygulama ve kullanımı için açılacak bağlantı noktaları belirleme [Windows Güvenlik Duvarı](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) veya [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) trafiğin HTTP.sys ulaşmasına izin vermek için güvenlik duvarı bağlantı noktalarını açmak için PowerShell cmdlet'i. Aşağıdaki komutları ve uygulama yapılandırması, 443 numaralı bağlantı noktası kullanılır.

1. Bir Azure VM Dağıtım yaparken, bağlantı noktaları açma [ağ güvenlik grubu](/azure/virtual-machines/windows/nsg-quickstart-portal). Aşağıdaki komutları ve uygulama yapılandırması, 443 numaralı bağlantı noktası kullanılır.

1. Edinin ve gerektiğinde X.509 sertifikaları yükleyin.

   Otomatik olarak imzalanan sertifikaları kullanarak Windows üzerinde oluşturun [New-SelfSignedCertificate PowerShell cmdlet'i](/powershell/module/pkiclient/new-selfsignedcertificate). Desteklenmeyen bir örnek için bkz [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).

   Sunucunun otomatik olarak imzalanan veya CA imzalı sertifika yükleme **yerel makine** > **kişisel** depolayın.

1. Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core, .NET Framework veya her ikisi de (uygulama .NET Framework'ü hedefleyen bir .NET Core uygulaması ise) yükleyin.

   * **.NET core** &ndash; uygulama, .NET Core gerektiriyorsa, edinme ve çalıştırma **.NET Core çalışma zamanı** Yükleyicisi'nden [.NET Core indirir](https://dotnet.microsoft.com/download). SDK tamamını sunucuda yüklemeyin.
   * **.NET framework** &ndash; uygulama .NET Framework gerekiyorsa, bkz. [.NET Framework Yükleme Kılavuzu](/dotnet/framework/install/). Gerekli .NET Framework'ü yükleyin. En son .NET Framework yükleyicisi [.NET Core indirir](https://dotnet.microsoft.com/download) sayfası.

   Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd), uygulama çalışma zamanı, dağıtımda içerir. Sunucuda hiçbir framework yüklemesi gerekir.

1. Uygulama URL'lerini ve bağlantı noktalarını yapılandırın.

   Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. URL ön ekleri ve bağlantı noktalarını yapılandırmak için Seçenekler şunlardır:

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * `urls` komut satırı bağımsız değişkeni
   * `ASPNETCORE_URLS` ortam değişkeni
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   Aşağıdaki kod örneği kullanma işlemini gösterir <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> sunucunun yerel IP adresi ile `10.0.0.4` 443 numaralı bağlantı noktasında:

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=6)]

   Bir avantajı `UrlPrefixes` bir hata iletisi hemen düzgün biçimlendirilmemiş ön ekleri için oluşturuldu.

   Ayarlarında `UrlPrefixes` geçersiz kılma `UseUrls` / `urls` / `ASPNETCORE_URLS` ayarları. Bu nedenle, bir avantajı `UseUrls`, `urls`ve `ASPNETCORE_URLS` ortam değişkenidir Kestrel ve HTTP.sys arasında geçiş yapmak kolaydır.

   HTTP.sys kullanan [HTTP Sunucusu API UrlPrefix dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır. Üst düzey joker bağlamaları, güvenlik açıklarını uygulama oluşturun. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Açık bir ana bilgisayar adlarını veya IP adresleri yerine joker karakterler kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetleyen bir güvenlik riski değildir (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan). Daha fazla bilgi için [RFC 7230: Bölüm 5.4: Konak](https://tools.ietf.org/html/rfc7230#section-5.4).

1. URL ön ekleri sunucuda preregister.

   HTTP.sys yapılandırmak için yerleşik bir aracı *netsh.exe*. *Netsh.exe* URL ön ekleri ayırabilir ve X.509 sertifikaları atamak için kullanılır. Aracı yöneticisi ayrıcalıkları gerektirir.

   Kullanım *netsh.exe* aracı uygulama için URL'leri kaydetmek için:

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * `<URL>` &ndash; Tam Tekdüzen Kaynak Konum Belirleyicisi (URL). Joker karakter bağlama kullanmayın. Geçerli ana bilgisayar adı veya yerel IP adresi kullanın. *URL'nin sonunda bir eğik çizgi içermelidir.*
   * `<USER>` &ndash; Kullanıcı veya kullanıcı grubunun adını belirtir.

   Aşağıdaki örnekte, yerel sunucunun IP adresi olan `10.0.0.4`:

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   Bir URL kaydedildiğinde, bu aracı ile yanıt veren `URL reservation successfully added`.

   Kayıtlı bir URL silmek için kullanın `delete urlacl` komutu:

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. X.509 sertifikaları sunucusuna kaydedin.

   Kullanım *netsh.exe* aracı uygulama için sertifikaları kaydetmek için:

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * `<IP>` &ndash; Bağlama için yerel IP adresi belirtir. Joker karakter bağlama kullanmayın. Geçerli bir IP adresi kullanın.
   * `<PORT>` &ndash; Bağlama için bir bağlantı noktasını belirtir.
   * `<THUMBPRINT>` &ndash; X.509 sertifika parmak izi.
   * `<GUID>` &ndash; Bilgilendirme amacıyla uygulamayı temsil etmek için bir geliştirici tarafından oluşturulan GUID.

   Başvuru amacıyla GUID uygulamada bir paket etiket olarak depola:

   * Visual Studio'da:
     * Uygulamada sağ tıklayarak uygulamanın proje özelliklerini açın **Çözüm Gezgini** seçerek **özellikleri**.
     * Seçin **paket** sekmesi.
     * Oluşturduğunuz GUID girin **etiketleri** alan.
   * Visual Studio kullanmadığınız durumlarda:
     * Uygulamanın proje dosyasını açın.
     * Ekleme bir `<PackageTags>` mevcut veya yeni bir özellik `<PropertyGroup>` oluşturduğunuz GUID'e sahip:

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   Aşağıdaki örnekte:

   * Yerel sunucunun IP adresidir `10.0.0.4`.
   * Çevrimiçi rastgele bir GUID Oluşturucu sağlar `appid` değeri.

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   Bir sertifika kaydedildiğinde, bu aracı ile yanıt veren `SSL Certificate successfully added`.

   Sertifika kaydı silmek için kullanın `delete sslcert` komutu:

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   Başvuru belgeleri için *netsh.exe*:

   * [Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
   * [UrlPrefix dizeleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. Uygulamayı çalıştırın.

   Yönetici ayrıcalıkları ile 1024'ten büyük bir bağlantı noktası numarası için localhost HTTP (HTTPS değil) kullanarak bağlanırken, uygulamayı çalıştırmak için gerekli değildir. Diğer yapılandırmalar için (örneğin, yerel bir IP adresi kullanarak veya bağlama bağlantı noktası 443) uygulamayı yönetici ayrıcalıklarıyla çalıştırın.

   Uygulama sunucusunun genel IP adresinde yanıt verir. Bu örnekte, genel IP adresi, Internet'ten sunucuya ulaştı `104.214.79.47`.

   Bu örnekte, bir geliştirme sertifikası kullanılır. Tarayıcının güvenilmeyen bir sertifika uyarısı atlayarak sonra sayfa güvenli bir şekilde yükler.

   ![Uygulamanın dizin sayfasını gösteren bir tarayıcı penceresi yüklenmedi](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Internet'ten veya kurumsal ağ istekleri etkileşim HTTP.sys tarafından barındırılan uygulamalar için ek yapılandırma Ara sunucuları ve yük dengeleyici barındırırken gerekli olabilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP.sys ile Windows kimlik doğrulamasını etkinleştirme](xref:security/authentication/windowsauth#httpsys)
* [HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [ASP.NET/HttpSysServer GitHub deposu (kaynak kodu)](https://github.com/aspnet/HttpSysServer/)
* [Ana bilgisayar](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
