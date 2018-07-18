---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Web uygulaması nasıl bir ASP.NET Core HTTPS/TLS'ye gerektirecek şekilde gösterir.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 331c17de33b5c13221385ffb4282bc16bde32289
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095723"
---
# <a name="enforce-https-in-aspnet-core"></a>ASP.NET core'da HTTPS'yi zorunlu kılma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge gösterir nasıl yapılır:

* Tüm istekler için HTTPS gerektirir.
* Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.

> [!WARNING]
> Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız. `RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır. API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır. Bu tür istemciler HTTP üzerinden bilgi gönderebilir. Web API'leri aşağıdakilerden birini yapmalısınız:
>
> * HTTP dinleme değil.
> * 400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.

<a name="require"></a>
## <a name="require-https"></a>HTTPS'yi zorunlu

::: moniker range=">= aspnetcore-2.1"

Tüm ASP.NET Core web uygulamaları çağrı HTTPS yeniden yönlendirmesi ara yazılım öneririz ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) tüm HTTP isteklerini HTTPS için yönlendirme.

Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Aşağıdaki kod çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Önceki vurgulanmış kodu:

* Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) için `Status307TemporaryRedirect`, varsayılan değer olan. Üretim uygulamaları çağırmalıdır [UseHsts](#hsts).
* HTTPS bağlantı noktası için 5001 ayarlar. 443 varsayılan değerdir.

Aşağıdaki mekanizmalardan bağlantı noktasını otomatik olarak ayarlayın:

* Ara yazılım aracılığıyla bağlantı noktaları bulabilir [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) zaman aşağıdaki koşullar geçerlidir:
  - Kestrel'i veya HTTP.sys doğrudan HTTPS uç noktaları ile kullanılan (Ayrıca uygulamanın Visual Studio Code'nın hata ayıklayıcısı ile çalıştırma için geçerlidir).
  - Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.
* Visual Studio kullanılır:
  - IIS Express, HTTPS etkin sahiptir.
  - *launchSettings.json* ayarlar `sslPort` IIS Express için.

> [!NOTE]
> Bir uygulama bir ters proxy (örneğin, IIS, IIS Express) çalıştırın, `IServerAddressesFeature` kullanılamaz. Bağlantı noktasını el ile yapılandırılması gerekir. Bağlantı noktası olarak değil, istekleri yeniden yönlendirilen değildir.

Bağlantı noktası ayarlayarak yapılandırılabilir:

* `ASPNETCORE_HTTPS_PORT` ortam değişkeni.
* `http_port` ana bilgisayar yapılandırma anahtarı (örneğin, aracılığıyla *hostsettings.json* veya komut satırı bağımsız değişkeni).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). Bağlantı noktası için 5001 olarak gösteren önceki örneğe bakın.

> [!NOTE]
> Bağlantı noktası URL'si ile ayarlayarak dolaylı olarak yapılandırılabilir `ASPNETCORE_URLS` ortam değişkeni. Ortam değişkenini sunucusunu yapılandırır ve ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur `IServerAddressesFeature`.

Bağlantı noktası ayarlanırsa:

* İstekleri yeniden yönlendirilen değildir.
* Ara yazılım, bir uyarı günlüğe kaydeder.

> [!NOTE]
> HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`). `AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz. Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).
>
> HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.

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

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP taşıma katı güvenlik protokolü (HSTS)

Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) özel yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir. Desteklenen bir tarayıcı bu üst bilgi aldıktan sonra bu tarayıcı her türlü iletişimi, HTTP üzerinden belirtilen etki alanına gönderilmesini engeller ve bunun yerine, tüm iletişimler HTTPS üzerinden gönderir. Ayrıca, HTTPS tıklayarak tarayıcılarında istekleri engeller.

ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi. Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` HSTS üstbilgi yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez. Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.

Aşağıdaki kodu:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar. Preload değil parçası [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak. Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.
* Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır. 
* Açıkça Strıct aktarım güvenliği üst max-age parametresini 60 gün olarak ayarlar. Aksi durumda, 30 gün için varsayılanları ayarlama. Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.
* Ekler `example.com` dışlanacak konaklar listesine.

`UseHsts` şu geri döngü konakları hariç tutar:

* `localhost` : IPv4 geri döngü adresi.
* `127.0.0.1` : IPv4 geri döngü adresi.
* `[::1]` : IPv6 geri döngü adresi.

Yukarıdaki örnekte, ek konaklarının nasıl ekleneceğini gösterir.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Çevirme HTTPS projesi oluşturma

ASP.NET Core 2.1 veya üzeri web uygulaması şablonlarıyla (Visual Studio veya dotnet komut satırı) etkinleştirme [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts). HTTPS gerektirmeyen dağıtımları için HTTPS çevirme. Örneğin, burada HTTPS dışarıdan ucuna her düğümde HTTPS kullanarak işlenen bazı arka uç Hizmetleri gerekli değildir.

Çevirme HTTPS için:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.

![Varlık diyagramı](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Kullanım `--no-https` seçeneği. Örneğin

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Docker için bir geliştirici sertifikası ayarlama

Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
