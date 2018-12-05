---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Bir ASP.NET Core web uygulamasını HTTPS/TLS gerektirir öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b15c6b5ac77f047c40704c9e164165c55b6ae93b
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861530"
---
# <a name="enforce-https-in-aspnet-core"></a>ASP.NET core'da HTTPS'yi zorunlu kılma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge gösterir nasıl yapılır:

* Tüm istekler için HTTPS gerektirir.
* Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.

Hiçbir API, bir istemci ilk istek üzerine hassas verileri göndermesini engelleyebilir.

> [!WARNING]
> Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız. `RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır. API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır. Bu tür istemciler HTTP üzerinden bilgi gönderebilir. Web API'leri aşağıdakilerden birini yapmalısınız:
>
> * HTTP dinleme değil.
> * 400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.

## <a name="require-https"></a>HTTPS'yi zorunlu

::: moniker range=">= aspnetcore-2.1"

Bu üretim ASP.NET Core web uygulamaları çağrısı öneririz:

* HTTPS yeniden yönlendirmesi ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) HTTP isteklerini HTTPS için yönlendirme.
* HSTS ara yazılımı ([UseHsts](#http-strict-transport-security-protocol-hsts)) HTTP katı Aktarım güvenlik protokolü (HSTS) üst bilgileri istemcilere göndermek için.

> [!NOTE]
> Proxy bağlantı güvenliği (HTTPS) işlemek bir ters proxy yapılandırmasıyla dağıtılan uygulamalar sağlar. Proxy de HTTPS yeniden yönlendirmesi işliyorsa, HTTPS yeniden yönlendirmesi ara yazılım kullanmaya gerek yoktur. Proxy sunucusu, aynı zamanda HSTS üstbilgileri yazma işleyen varsa (örneğin, [yerel HSTS desteği (1709) IIS 10.0 veya üzeri](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS ara yazılım, uygulama tarafından gerekli değildir. Daha fazla bilgi için [geri çevirme HTTPS/HSTS, proje oluşturma sırasında](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Önceki vurgulanmış kodu:

* Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) tarafından geçersiz kılınmadığı sürece `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Kalıcı yeniden yönlendirmeleri yerine geçici yeniden yönlendirmeleri kullanmanızı öneririz. Bağlantıyı önbelleğe almayı, geliştirme ortamlarında tutarsız davranışlara neden olabilir. Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermek isterseniz, bkz. [yeniden yönlendirmeleri kalıcı olarak üretim ortamında yapılandırma](#configure-permanent-redirects-in-production) bölümü. Kullanmanızı öneririz [HSTS](#http-strict-transport-security-protocol-hsts) yalnızca kaynak güvenli istemcileri için sinyal istekleri uygulamada (yalnızca üretim) gönderilmelidir.

### <a name="port-configuration"></a>Bağlantı noktası yapılandırması

Güvenli olmayan bir istek HTTPS'ye yönlendirmek için bir bağlantı noktası için ara yazılımı kullanılabilir olması gerekir. Bağlantı noktası varsa:

* HTTPS için yeniden yönlendirme gerçekleşmez.
* Ara yazılım, "yeniden yönlendirme için https bağlantı noktasını belirlemek için başarısız oldu." uyarısı günlüğe kaydeder

Aşağıdaki yaklaşımlardan birini kullanarak HTTPS bağlantı noktasını belirtin:

* Ayarlama [HttpsRedirectionOptions.HttpsPort](#options).
* Ayarlama `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):

  **Anahtar**: `https_port`  
  **Tür**: *dize*  
  **Varsayılan**: varsayılan bir değer ayarlanmamış.  
  **Kullanılarak ayarlanan**: `UseSetting`  
  **Ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (ön ek `ASPNETCORE_` kullanırken [Web ana bilgisayarı](xref:fundamentals/host/web-host).)

  Yapılandırma sırasında bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> içinde `Program`:

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* Güvenli düzenini kullanarak bir bağlantı noktası belirtmek `ASPNETCORE_URLS` ortam değişkeni. Ortam değişkenini sunucusunu yapılandırır. Ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>. Bu yaklaşım ters proxy dağıtımlarda çalışmaz.
* Bir HTTPS URL'si, geliştirme kümesinde *launchsettings.json*. IIS Express kullanıldığında HTTPS etkinleştirin.
* Genel kullanıma yönelik edge dağıtımı için bir HTTPS URL'si uç nokta yapılandırma [Kestrel](xref:fundamentals/servers/kestrel) sunucu veya [HTTP.sys](xref:fundamentals/servers/httpsys) sunucusu. Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır. Ara yazılım aracılığıyla bağlantı noktası bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.

> [!NOTE]
> Bir ters proxy yapılandırması'nda bir uygulama çalıştırıldığında <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> kullanılamaz. Bu bölümde açıklanan yaklaşımlardan birini kullanarak bağlantı noktasını ayarlayın.

Kestrel'i veya HTTP.sys genel kullanıma yönelik bir uç sunucusu olarak kullanıldığında, hem de dinlenecek Kestrel veya HTTP.sys yapılandırılmalıdır:

* İstemci yeniden yönlendirilmiş burada güvenli bağlantı noktası (genellikle, üretim ve geliştirme 5001 443).
* Güvensiz bağlantı noktası (genellikle, üretimde 80) ile 5000 geliştirme.

Güvensiz bağlantı güvenli olmayan bir istek almak ve güvenli bağlantı noktasına istemciyi yeniden yönlendirmek için uygulamanın sırası istemci tarafından erişilebilir olmalıdır.

Daha fazla bilgi için [Kestrel uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) veya <xref:fundamentals/servers/httpsys>.

### <a name="deployment-scenarios"></a>Dağıtım senaryoları

İstemci ve sunucu arasında herhangi bir güvenlik duvarını Ayrıca iletişim bağlantı noktaları trafik için açık olması gerekir.

İstekleri bir ters proxy yapılandırma iletilir kullanırsanız [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) çağırmadan önce HTTPS yeniden yönlendirmesi ara yazılım. Üst bilgileri ara yazılım güncelleştirmeleri iletilen `Request.Scheme`kullanarak `X-Forwarded-Proto` başlığı. Ara yazılım verir yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri düzgün çalışması için. İletilen üstbilgileri ara yazılımı kullanılmaz, arka uç uygulama doğru düzenini almak değil ve bir yeniden yönlendirme döngüsüne. Ortak bir son kullanıcı hata iletisi, çok fazla yeniden yönlendirmeleri oluşmuş ' dir.

Azure App Service'e dağıtım yaparken, sunulan yönergeleri [öğretici: Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Seçenekler

Aşağıdaki vurgulanmış kodu çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Çağırma `AddHttpsRedirection` yalnızca değerlerini değiştirmek gerekli olan `HttpsPort` veya `RedirectStatusCode`.

Önceki vurgulanmış kodu:

* Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) için <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, varsayılan değer olan. Alanlarını kullanın <xref:Microsoft.AspNetCore.Http.StatusCodes> sınıfı atamaların `RedirectStatusCode`.
* HTTPS bağlantı noktası için 5001 ayarlar. 443 varsayılan değerdir.

#### <a name="configure-permanent-redirects-in-production"></a>Üretim ortamında yeniden yönlendirmeleri kalıcı olarak yapılandırma

Ara yazılım varsayılan olarak göndermek için bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) ile tüm yeniden yönlendirir. Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermeyi tercih ederseniz, ara yazılım seçenekleri yapılandırma koşullu bir denetimi geliştirme ortam için kaydırın.

Yapılandırma sırasında bir `IWebHostBuilder` içinde *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a>HTTPS yeniden yönlendirmesi ara yazılım alternatif bir yaklaşım

HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`). `AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz. Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).

HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS'yi zorunlu tutmak için kullanılır. `[RequireHttpsAttribute]` denetleyicileri veya yöntemleri donatmak veya genel olarak uygulanabilir. Genel öznitelik uygulamak için aşağıdaki kodu ekleyin. `ConfigureServices` içinde `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Tüm istekleri kullanmak önceki vurgulanan kod gerektirir `HTTPS`; bu nedenle, HTTP isteklerini dikkate alınmaz. Aşağıdaki vurgulanmış kodu tüm HTTP isteklerini HTTPS için yeniden yönlendirir:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting). Ara yazılım Ayrıca, uygulama yeniden yönlendirme yürütüldüğünde, durum kodu veya durum kodunu ve bağlantı noktası ayarlamak için verir.

Genel olarak HTTPS'yi zorunlu (`options.Filters.Add(new RequireHttpsAttribute());`) güvenlik en iyi uygulamadır. Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfaları için öznitelik değil olarak kabul genel HTTPS'yi zorunlu kadar güvenli. Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfaları eklendiğinde uygulanır.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP taşıma katı güvenlik protokolü (HSTS)

Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir. Olduğunda bir [HSTS destekleyen bir tarayıcı](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) bu üstbilgiyi alır:

* Tarayıcıya HTTP üzerinden herhangi bir iletişim göndermeyi önler etki alanı için yapılandırma depolar. Tarayıcı tüm iletişim HTTPS üzerinden zorlar.
* Tarayıcı kullanıcı, güvenilmeyen ya da geçersiz bir sertifika kullanmasını önler. Tarayıcı geçici olarak bu tür bir sertifika güven etmesine istemlerini devre dışı bırakır.

İstemci tarafından HSTS zorunlu kılındığından bazı sınırlamalar vardır:

* İstemci HSTS desteklemesi gerekir.
* HSTS HSTS ilkesi oluşturmak üzere en az bir başarılı HTTPS isteğini gerektirir.
* Uygulama gerekir her HTTP isteği denetleyin ve yeniden yönlendirme veya HTTP isteği reddedebilir.

ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi. Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` HSTS ayarları yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez. Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.

Üretim ortamları için ilk ayarlamak için ilk kez, HTTPS uygulama [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) birini kullanarak küçük bir değere <xref:System.TimeSpan> yöntemleri. HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın. HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir.

Aşağıdaki kodu:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar. Önyükleme bölümü olmayan [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak. Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.
* Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır.
* Açıkça Strıct aktarım güvenliği üst bilgi, max-age parametresini 60 gün olarak ayarlar. Aksi durumda, 30 gün için varsayılanları ayarlama. Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.
* Ekler `example.com` dışlanacak konaklar listesine.

`UseHsts` şu geri döngü konakları hariç tutar:

* `localhost` : IPv4 geri döngü adresi.
* `127.0.0.1` : IPv4 geri döngü adresi.
* `[::1]` : IPv6 geri döngü adresi.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Çevirme HTTPS/HSTS, proje oluşturma

Bağlantı güvenliği ağ genel kullanıma yönelik ucuna nerede işlendiğini bazı arka uç hizmeti senaryolarda, her düğümde bağlantı güvenliği yapılandırma gerekli değildir. Web uygulamaları Visual Studio'da veya gelen şablonlardan oluşturulan [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu etkinleştirme [HTTPS yeniden yönlendirmesi](#require-https) ve [HSTS](#http-strict-transport-security-protocol-hsts). Bu senaryolar gerektirmeyen dağıtımları için HTTPS/HSTS şablondan uygulama oluşturulduğunda çevirme.

Çevirme HTTPS/HSTS için:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.

![HTTPS onay kutusu seçili yapılandırma gösteren yeni ASP.NET Core Web uygulaması iletişim kutusu.](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Kullanım `--no-https` seçeneği. Örneğin

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>Windows ve macOS üzerinde ASP.NET Core HTTPS geliştirme sertifikasına güvenmek

.NET core SDK'sı, HTTPS geliştirme sertifikası içerir. Sertifika, ilk kez çalıştırma deneyimi bir parçası olarak yüklenir. Örneğin, `dotnet --info` aşağıdakine benzer bir çıktı üretir:

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

.NET Core SDK'sını yükleme ASP.NET Core HTTPS geliştirme sertifikası yerel kullanıcı sertifika deposuna yükler. Sertifika yüklü, ancak güvenilmeyen. Sertifika güven için dotnet çalıştırmak için tek seferlik bir adım gerçekleştirmeniz `dev-certs` aracı:

```console
dotnet dev-certs https --trust
```

Aşağıdaki komut hakkında Yardım sağlar. `dev-certs` aracı:

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Docker için bir geliştirici sertifikası ayarlama

Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Ek bilgiler

* <xref:host-and-deploy/proxy-load-balancer>
* [Apache ile Linux'ta ASP.NET Core barındıran: SSL yapılandırması](xref:host-and-deploy/linux-apache#ssl-configuration)
* [Nginx ile Linux'ta ASP.NET Core barındıran: SSL yapılandırması](xref:host-and-deploy/linux-nginx#configure-ssl)
* [IIS'de SSL ayarlama](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [OWASP HSTS tarayıcı desteği](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
