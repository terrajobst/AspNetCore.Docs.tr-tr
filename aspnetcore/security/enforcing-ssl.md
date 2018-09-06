---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Web uygulaması nasıl bir ASP.NET Core HTTPS/TLS'ye gerektirecek şekilde gösterir.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 838cd00545f36736461616f806942249aaf6eee0
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893184"
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

<a name="require"></a>
## <a name="require-https"></a>HTTPS'yi zorunlu

::: moniker range=">= aspnetcore-2.1"

Tüm üretim ASP.NET Core web uygulamaları çağrısı öneririz:

* HTTPS yeniden yönlendirmesi Ara ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) tüm HTTP isteklerini HTTPS için yönlendirme.
* [UseHsts](#hsts), HTTP katı Aktarım güvenlik protokolü (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Önceki vurgulanmış kodu:

* Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).
* Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) tarafından geçersiz kılınmadığı sürece `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

> [!WARNING] 
>HTTPS'ye yönlendirmek için bir bağlantı noktası için ara yazılımı kullanılabilir olması gerekir. Bağlantı noktası varsa, HTTPS için yeniden yönlendirme gerçekleşmez. HTTPS bağlantı noktası aşağıdaki ayarı tarafından belirtilebilir:
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* `ASPNETCORE_HTTPS_PORT` Ortam değişkeni. 
>* Geliştirme, içinde bir HTTPS URL'si *launchsettings.json*. 
>* Doğrudan Kestrel veya HttpSys üzerinde yapılandırılan bir HTTPS URL'si. 

Aşağıdaki vurgulanmış kodu çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Çağırma `AddHttpsRedirection` yalnızca değerlerini değiştirmek gerekli olan ` HttpsPort` veya ` RedirectStatusCode`;

Önceki vurgulanmış kodu:

* Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) için `Status307TemporaryRedirect`, varsayılan değer olan.
* HTTPS bağlantı noktası için 5001 ayarlar. 443 varsayılan değerdir.

Aşağıdaki mekanizmalardan bağlantı noktasını otomatik olarak ayarlayın:

* Ara yazılım aracılığıyla bağlantı noktaları bulabilir [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) zaman aşağıdaki koşullar geçerlidir:

   * Kestrel'i veya HTTP.sys doğrudan HTTPS uç noktaları ile kullanılan (Ayrıca uygulamanın Visual Studio Code'nın hata ayıklayıcısı ile çalıştırma için geçerlidir).
   * Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.

* Visual Studio kullanılır:
   * IIS Express, HTTPS etkin sahiptir.
   * *launchSettings.json* ayarlar `sslPort` IIS Express için.

> [!NOTE]
> Bir uygulama bir ters proxy (örneğin, IIS, IIS Express) çalıştırın, `IServerAddressesFeature` kullanılamaz. Bağlantı noktasını el ile yapılandırılması gerekir. Bağlantı noktası olarak değil, istekleri yeniden yönlendirilen değildir.

Bağlantı noktası ayarlayarak yapılandırılabilir [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):

**Anahtar**: https_port  
**Tür**: *dize*  
**Varsayılan**: varsayılan bir değer ayarlanmamış.  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (ön ek `ASPNETCORE_` Web ana bilgisayarı kullanırken.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Bağlantı noktası URL'si ile ayarlayarak dolaylı olarak yapılandırılabilir `ASPNETCORE_URLS` ortam değişkeni. Ortam değişkenini sunucusunu yapılandırır ve ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur `IServerAddressesFeature`.

Bağlantı noktası ayarlanırsa:

* İstekleri yeniden yönlendirilen değildir.
* Ara yazılım, "yeniden yönlendirme için https bağlantı noktasını belirlemek için başarısız oldu." uyarısı günlüğe kaydeder

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

HTTPS için ilk kez uygulama, üretim ortamları için başlangıç HSTS değeri küçük bir değere ayarlayın. HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın. HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir. 

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

Yukarıdaki örnekte, ek konaklarının nasıl ekleneceğini gösterir.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Çevirme HTTPS projesi oluşturma

ASP.NET Core 2.1 veya üzeri web uygulaması şablonlarıyla (Visual Studio veya dotnet komut satırı) etkinleştirme [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts). HTTPS gerektirmeyen dağıtımları için HTTPS çevirme. Örneğin, burada HTTPS dışarıdan ucuna her düğümde HTTPS kullanarak işlenen bazı arka uç Hizmetleri gerekli olmayan.

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

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Docker için bir geliştirici sertifikası ayarlama

Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Ek bilgiler

* [OWASP HSTS tarayıcı desteği](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
