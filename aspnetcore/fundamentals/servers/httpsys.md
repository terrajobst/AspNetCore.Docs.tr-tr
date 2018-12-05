---
title: ASP.NET core'da HTTP.sys web sunucusu uygulaması
author: guardrex
description: HTTP.sys, ASP.NET Core, Windows için bir web sunucusu hakkında bilgi edinin. HTTP.sys çekirdek modu sürücüsü üzerinde oluşturulmuş, HTTP.sys için IIS olmadan İnternet'e doğrudan bağlantı için kullanılan Kestrel alternatiftir.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 8810fd295e8c4269812e712ce2fdc9b9fa2bbb4f
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861699"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET core'da HTTP.sys web sunucusu uygulaması

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), ve [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Bu konu, ASP.NET Core 2.0 veya sonraki bir sürüme geçerlidir. ASP.NET Core önceki sürümlerde HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener).

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) olduğu bir [ASP.NET Core web sunucusu](xref:fundamentals/servers/index) Windows üzerinde yalnızca çalışır. HTTP.sys olan alternatif [Kestrel](xref:fundamentals/servers/kestrel) sunucu ve teklifler bazı özellikleri Kestrel sağlamaz.

> [!IMPORTANT]
> HTTP.sys uyumlu [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve IIS veya IIS Express ile kullanılamaz.

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

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

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

2. Çağrı [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) gerekli belirtme web ana bilgisayarı oluşturulurken genişletme yöntemi [HTTP.sys seçenekleri](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Ek HTTP.sys yapılandırma aracılığıyla işlenir [kayıt defteri ayarları](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

   **HTTP.sys seçenekleri**

   | Özellik | Açıklama | Varsayılan |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Zaman uyumlu giriş/çıkış için izin verilip verilmediğini `HttpContext.Request.Body` ve `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Anonim isteklere izin verin. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | İzin verilen kimlik doğrulama şemasını belirtin. Dinleyici disposing önce herhangi bir zamanda değiştirilebilir. Değerleri tarafından sağlanan [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, ve `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Girişimi [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) uygun üst bilgilerini içeren yanıtlar için önbelleğe alma. Yanıta dahil `Set-Cookie`, `Vary`, veya `Pragma` üstbilgileri. İçermesi gerekir bir `Cache-Control` üst bilgisi bu `public` ve ya da bir `shared-max-age` veya `max-age` değeri veya bir `Expires` başlığı. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | En fazla eşzamanlı sayısı kabul eder. | 5 &times; [ortam.<br> ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Kabul etmek üzere kullanılan eşzamanlı bağlantı sayısı. Kullanım `-1` sonsuz için. Kullanma `null` kayıt defterinin makine genelindeki ayarını kullanmak için. | `null`<br>(sınırsız) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Bkz: <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümü. | 30000000 bayt<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Sıraya alınabilecek isteklerinin sayısı. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Yanıt gövdesi yazma, hata nedeniyle istemci bağlantısını keser veya özel durumlar genellikle tamamlamak gösterir. | `false`<br>(normalde Tamamla) |
   | [Zaman aşımı](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | HTTP.sys kullanıma [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) yapılandırma, kayıt defterinde da yapılandırılabilir. Varsayılan değerleri dahil olmak üzere, her ayar hakkında daha fazla bilgi için API bağlantıları izleyin:<ul><li>[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; HTTP Sunucusu API varlık gövdesini bir canlı bağlantı boşaltma zaman izin verilir.</li><li>[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; ulaşması istek Varlık gövdesi için izin verilen süresi.</li><li>[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; süresi izin verilen HTTP Sunucusu API istek üstbilgisi ayrıştırılamıyor.</li><li>[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; boştaki bir bağlantının için izin verilen süresi.</li><li>[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; en düşük yanıt hızı gönderin.</li><li>[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; zaman izin isteğinin, uygulamayı alır önce isteği kuyruğunda kalır.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Belirtin [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) HTTP.sys ile kaydetmek için. Kullanışlı [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), bir önek koleksiyona eklemek için kullanılır. Bu dinleyici disposing önce herhangi bir zamanda değiştirilebilir. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Bayt cinsinden izin verilen maksimum herhangi bir istek gövdesi boyutu. Ayarlandığında `null`, en fazla istek gövdesi boyutu sınırsızdır. Bu sınırı her zaman sınırsız yükseltilen bağlantılar üzerinde etkisi yoktur.

   Bir ASP.NET Core MVC uygulaması için tek bir sınırı geçersiz kılmak için önerilen yöntem `IActionResult` kullanmaktır [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) özniteliği bir eylem yöntemi:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırını yapılandırmak uygulamanın çalışırsa, bir özel durum oluşturulur. Bir `IsReadOnly` özelliği belirtmek için kullanılabilir `MaxRequestBodySize` özelliği olan bir salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

   Uygulamayı'ı geçersiz kılmalıdır varsa [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) istek başına, kullanın [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. Visual Studio kullanıyorsanız, uygulamayı IIS veya IIS Express çalıştırmak için yapılandırılmamış emin olun.

   Visual Studio'da varsayılan başlatma için IIS Express profilidir. Proje bir konsol uygulaması çalıştırmak için el ile seçilen profil, aşağıdaki ekran görüntüsünde gösterildiği gibi değiştirin:

   ![Konsol uygulama profili seçin](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Windows Server'ı yapılandırma

1. Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core, .NET Framework veya her ikisi de (uygulama .NET Framework'ü hedefleyen bir .NET Core uygulaması ise) yükleyin.

   * **.NET core** &ndash; uygulama, .NET Core gerektiriyorsa, edinme ve .NET Core Yükleyicisi'nden çalıştırmak [.NET tüm indirmeleri](https://www.microsoft.com/net/download/all).
   * **.NET framework** &ndash; uygulama .NET Framework gerekiyorsa, bkz. [.NET Framework: Yükleme Kılavuzu](/dotnet/framework/install/) yükleme yönergeleri bulmak için. Gerekli .NET Framework'ü yükleyin. En son .NET Framework yükleyicisi şu yolda bulunabilir: [.NET tüm indirmeleri](https://www.microsoft.com/net/download/all).

2. URL'ler ve uygulama için bağlantı noktalarını yapılandırın.

   Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. URL ön ekleri ve bağlantı noktalarını yapılandırmak için kullanmayı seçenekleri içerir:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` komut satırı bağımsız değişkeni
   * `ASPNETCORE_URLS` ortam değişkeni
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   Aşağıdaki kod örneği kullanma işlemini gösterir [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Bir avantajı `UrlPrefixes` bir hata iletisi hemen düzgün biçimlendirilmemiş ön ekleri için oluşturuldu.

   Ayarlarında `UrlPrefixes` geçersiz kılma `UseUrls` / `urls` / `ASPNETCORE_URLS` ayarları. Bu nedenle, bir avantajı `UseUrls`, `urls`ve `ASPNETCORE_URLS` ortam değişkenidir Kestrel ve HTTP.sys arasında geçiş yapmak kolaydır. Daha fazla bilgi için `UseUrls`, `urls`, ve `ASPNETCORE_URLS`, bakın [ASP.NET Core ana](xref:fundamentals/host/index) konu.

   HTTP.sys kullanan [HTTP Sunucusu API UrlPrefix dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır. Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık bir ana bilgisayar adları kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan). Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

3. HTTP.sys için bağlama ve x.509 sertifikaları ayarlama yapılacak URL ön ekleri preregister.

   URL ön ekleri Windows önceden kayıtlı olmayan uygulama yönetici ayrıcalıklarıyla çalıştırın. Tek özel durum 1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil)'ı kullanarak Localhost'a bağlama sırasında dir. Bu durumda, yönetici ayrıcalıkları gerekli değildir.

   1. HTTP.sys yapılandırmak için yerleşik bir aracı *netsh.exe*. *Netsh.exe* URL ön ekleri ayırabilir ve X.509 sertifikaları atamak için kullanılır. Aracı yöneticisi ayrıcalıkları gerektirir.

      Aşağıdaki örnek, 80 ve 443 bağlantı noktaları için URL ön ekleri ayırmada komutları gösterir:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      Aşağıdaki örnek, bir X.509 sertifikası atamak gösterilmektedir:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      Başvuru belgeleri için *netsh.exe*:

      * [Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
      * [UrlPrefix dizeleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. Gerekli olursa, otomatik olarak imzalanan X.509 sertifikaları oluşturun.

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. Trafiğin HTTP.sys ulaşmasına izin vermek için güvenlik duvarı bağlantı noktalarını açın. Kullanım *netsh.exe* veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Internet'ten veya kurumsal ağ istekleri etkileşim HTTP.sys tarafından barındırılan uygulamalar için ek yapılandırma Ara sunucuları ve yük dengeleyici barındırırken gerekli olabilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [ASP.NET/HttpSysServer GitHub deposu (kaynak kodu)](https://github.com/aspnet/HttpSysServer/)
* <xref:fundamentals/host/index>
* <xref:test/troubleshoot>
