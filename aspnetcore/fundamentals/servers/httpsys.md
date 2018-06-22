---
title: HTTP.sys web server ASP.NET Core uygulamasında
author: rick-anderson
description: HTTP.sys, ASP.NET Core Windows için bir web sunucusu hakkında bilgi edinin. HTTP.sys çekirdek modu sürücüsü üzerinde oluşturulmuş, HTTP.sys bir IIS olmadan Internet'e doğrudan bağlantı için kullanılan Kestrel alternatifidir.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
uid: fundamentals/servers/httpsys
ms.openlocfilehash: ae76c9d3adde524fd246b0228d74861ea2b81272
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278680"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>HTTP.sys web server ASP.NET Core uygulamasında

Tarafından [zel Dykstra](https://github.com/tdykstra), [Chris fillerin](https://github.com/Tratcher), ve [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Bu konu, ASP.NET Core 2.0 veya sonraki bir sürüme geçerlidir. ASP.NET Core önceki sürümlerde HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener).

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) olan bir [web sunucusu için ASP.NET Core](xref:fundamentals/servers/index) Windows'da yalnızca çalışır. HTTP.sys olan alternatif [Kestrel](xref:fundamentals/servers/kestrel) ve Kestrel sağlamaz bazı özellikler sunar.

> [!IMPORTANT]
> HTTP.sys ile uyumsuz [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve IIS veya IIS Express ile kullanılamaz.

HTTP.sys aşağıdaki özellikleri destekler:

* [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)
* Bağlantı noktası paylaşma
* SNI ile HTTPS
* TLS (Windows 10 veya üstü) üzerinden HTTP/2
* Doğrudan dosya aktarımı
* Yanıt önbelleğe alma
* WebSockets (Windows 8 veya üzeri)

Desteklenen Windows sürümlerine:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya sonraki sürümü

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>HTTP.sys kullanma zamanı

HTTP.sys dağıtımları için yararlıdır burada:

* IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak için bir gereksinimi yoktur.

  ![HTTP.sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

* Bir iç dağıtım özelliği Kestrel içinde yok gibi gerektirir [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).

  ![HTTP.sys iç ağ ile doğrudan iletişim kurar](httpsys/_static/httpsys-to-internal.png)

HTTP.sys türlerde saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir. IIS'nin bir HTTP dinleyicisi HTTP.sys üstünde çalışır. 

## <a name="how-to-use-httpsys"></a>HTTP.sys kullanma

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>HTTP.sys kullanmak için ASP.NET Core uygulamayı yapılandırma

1. Proje dosyasında paket başvuru kullanırken gerekli değil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 veya sonrası). Değil kullanırken `Microsoft.AspNetCore.App` metapackage, paket için bir başvuru ekleyin [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Çağrı [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) gerekli belirtme web ana bilgisayarı oluştururken genişletme yöntemi [HTTP.sys seçenekleri](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Ek HTTP.sys yapılandırma aracılığıyla gerçekleştirilir [kayıt defteri ayarları](https://support.microsoft.com/kb/820129).

   **HTTP.sys seçenekleri**

   | Özellik | Açıklama | Varsayılan |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Zaman uyumlu giriş/çıkış için izin verilip verilmediğini denetleme `HttpContext.Request.Body` ve `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Anonim isteklere izin verir. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | İzin verilen kimlik doğrulama şemasını belirtin. Dinleyici atma önce herhangi bir zamanda değiştirilebilir. Tarafından sağlanan değerler [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, ve `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Girişimi [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) uygun üst bilgileri ile yanıtlarını önbelleğe alma. Yanıt değil içerebilir `Set-Cookie`, `Vary`, veya `Pragma` üstbilgileri. İçermesi gerekir bir `Cache-Control` üstbilgi o `public` ve ya da bir `shared-max-age` veya `max-age` değeri veya bir `Expires` üstbilgi. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | En fazla eşzamanlı kabul eder. | 5 &times; [ortamı.<br> ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [maxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Kabul etmek için eşzamanlı bağlantı sayısı. Kullanım `-1` sonsuz için. Kullanmak `null` kayıt defterindeki makine genelinde ayarı kullanmak için. | `null`<br>(sınırsız) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Bkz: <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümü. | 30000000 bayt<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Sıraya alınabilecek isteklerinin sayısı. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Başarısız istemci nedeniyle keser yanıt gövdesi yazma veya özel durumlar oluşturma normal şekilde tamamlayın gösterir. | `false`<br>(normal şekilde tamamlayın.) |
   | [Zaman aşımları](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | HTTP.sys kullanıma [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) kayıt defterinde da yapılandırılabilir yapılandırma. Varsayılan değerler dahil olmak üzere her ayar hakkında daha fazla bilgi edinmek için API bağlantıları izleyin:<ul><li>[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; süresi izin verilen varlık gövdesini tutma bağlantıda boşaltmak HTTP Sunucu API'si.</li><li>[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; gelmesi istek Varlık gövdesi için izin verilen süresi.</li><li>[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; süresi izin verilen HTTP Sunucusu API istek üstbilgisi ayrıştırılamıyor.</li><li>[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; boştaki bir bağlantı için izin verilen süresi.</li><li>[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; en düşük gönderme yanıtı için oranı.</li><li>[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; süresi izin verilen isteğin istek sırasında uygulama alır önce kalabileceği.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Belirtin [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) HTTP.sys ile kaydetmek için. En yararlı olduğu [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), bir önek koleksiyona eklemek için kullanılır. Bunlar dinleyicisi atma önce herhangi bir zamanda değiştirilebilir. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Bayt cinsinden izin verilen en fazla istek gövdesi boyutu. Ayarlandığında `null`, en büyük istek gövdesi sınırsız boyutudur. Bu sınır her zaman sınırsız yükseltilmiş bağlantıları üzerinde etkisi yoktur.

   Tek bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem `IActionResult` kullanmaktır [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) bir eylem yönteminin özniteliği:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırını yapılandırmak uygulama çalışırsa özel durum oluşur. Bir `IsReadOnly` özelliği, aşağıdaki durumlarda belirtmek için kullanılabilir `MaxRequestBodySize` özelliği olan bir salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.

   Uygulama geçersiz kılmalıdır varsa [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) istek başına, kullanın [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. Visual Studio kullanarak, uygulama IIS veya IIS Express çalışmak üzere yapılandırılmamış emin olun.

   Visual Studio'da varsayılan başlatma için IIS Express profilidir. Projeyi bir konsol uygulaması çalıştırmak için el ile seçilen profil aşağıdaki ekran görüntüsünde gösterildiği gibi değiştirin:

   ![Konsol uygulama profili seçin](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Windows Server yapılandırın

1. Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core, .NET Framework veya her ikisi de (uygulama .NET Framework'ü hedefleme .NET Core uygulama ise) yükleyin.

   * **.NET core** &ndash; uygulamayı .NET Core gerektiriyorsa, edinin ve .NET Core Yükleyicisi'nden çalıştırın [.NET tüm yüklemelerini](https://www.microsoft.com/net/download/all).
   * **.NET framework** &ndash; uygulama .NET Framework gerektiriyorsa, bkz: [.NET Framework: Yükleme Kılavuzu](/dotnet/framework/install/) yükleme yönergeleri bulmak için. Gerekli .NET Framework'ü yükleyin. En son .NET Framework yükleyici bulunabilir [.NET tüm yüklemelerini](https://www.microsoft.com/net/download/all).

2. URL'ler ve uygulama için bağlantı noktalarını yapılandırın.

   Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`. URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanarak Seçenekler şunlardır:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` Komut satırı bağımsız değişkeni
   * `ASPNETCORE_URLS` Ortam değişkeni
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   Aşağıdaki kod örneğinde nasıl kullanılacağını gösterir [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Bir avantajı `UrlPrefixes` bir hata iletisi hemen düzgün biçimlendirilmemiş önekleri için oluşturulmuş.

   Ayarlarında `UrlPrefixes` geçersiz kılma `UseUrls` / `urls` / `ASPNETCORE_URLS` ayarlar. Bu nedenle, bir avantajı `UseUrls`, `urls`ve `ASPNETCORE_URLS` ortam değişkenidir HTTP.sys Kestrel arasında geçiş yapmak kolaydır. Daha fazla bilgi için `UseUrls`, `urls`, ve `ASPNETCORE_URLS`, bkz: [ASP.NET Core ana](xref:fundamentals/host/index) konu.

   HTTP.sys kullanan [HTTP Sunucusu API UrlPrefix dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılabilir. Üst düzey joker bağlamaları uygulamanızı güvenlik açıkları için yedekleme açabilirsiniz. Bu, güçlü ve zayıf joker karakterler için geçerlidir. Joker karakterler yerine açık ana bilgisayar adları kullanın. Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanı denetlemek, bu güvenlik riskinin yok (tersine `*.com`, açık olduğu). Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.

3. HTTP.sys'ye bağlayın ve x.509 sertifikalar ayarlamak için URL öneklerini preregister.

   URL öneklerini Windows preregistered değil, uygulama yönetici ayrıcalıklarıyla çalıştırın. 1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlama sırasında tek özel durumdur. Bu durumda, yönetici ayrıcalıkları gerekli değildir.

   1. HTTP.sys yapılandırmak için yerleşik bir araçtır *netsh.exe*. *Netsh.exe* URL öneklerini ayırmak ve X.509 sertifikalarını atamak için kullanılır. Aracı, yönetici ayrıcalıkları gerektirir.

      Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için komutları gösterir:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      Aşağıdaki örnek, bir X.509 sertifikası atama gösterilmiştir:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      Başvuru belgelerini için *netsh.exe*:

      * [Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
      * [UrlPrefix dizeleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. X.509 sertifikaları otomatik olarak imzalanan, gerekirse oluşturun.

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. HTTP.sys ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın. Kullanım *netsh.exe* veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy sunucusu ve yük dengeleyici senaryoları

Internet'ten veya bir şirket ağı isteklerle etkileşim HTTP.sys tarafından barındırılan uygulamalar için ek yapılandırma proxy sunucuları ve yük dengeleyici arkasında barındırdığında gerekli olabilir. Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [ASPNET/HttpSysServer GitHub deposunu (kaynak kodu)](https://github.com/aspnet/HttpSysServer/)
* [ASP.NET Core ana bilgisayar](xref:fundamentals/host/index)
