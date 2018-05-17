---
title: Bir ASP.NET Core HTTPS zorla
author: rick-anderson
description: Bir ASP.NET Core HTTPS/TLS gerektiren web uygulaması gösterilmektedir.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: edc69443455677ba80ebb0a73e193d4d6741e470
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a>Bir ASP.NET Core HTTPS zorla

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge gösterir nasıl yapılır:

- HTTPS için tüm istekleri gerektirir.
- Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.

> [!WARNING]
> Yapmak **değil** kullanmak `RequireHttpsAttribute` Web API'lerde hassas bilgiler alırsınız. `RequireHttpsAttribute` HTTP tarayıcılarından HTTPS'ye yeniden yönlendirmek için HTTP durum kodları kullanır. API istemcileri değil anlamak veya HTTP yönlendirir HTTPS uymaktadır. Bu tür istemciler HTTP üzerinden bilgi gönderebilir. Web API'leri aşağıdakilerden birini yapmalısınız:
>
>* HTTP dinleme değil.
>* Durum kodu 400 (Hatalı istek) ile bağlantı kapatın ve istek hizmet yok.

<a name="require"></a>
## <a name="require-https"></a>HTTPS gerektirir

::: moniker range=">= aspnetcore-2.1"
Web uygulamaları çağrısı tüm ASP.NET Core öneririz `UseHttpsRedirection` HTTPS için tüm HTTP isteklerini yeniden yönlendirmek için. Varsa `UseHsts` çağrılır uygulamada, onu önce çağrılmalıdır `UseHttpsRedirection`.

Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]


Aşağıdaki kod:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

* Ayarlar `RedirectStatusCode`.
* HTTPS bağlantı noktası için 5001 ayarlar.

::: moniker-end


::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) HTTPS gerektirecek şekilde kullanılır. `[RequireHttpsAttribute]` denetleyicileri veya yöntemleri işaretleme veya genel olarak uygulanabilir. Öznitelik genel uygulamak için aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Tüm istekleri kullanır önceki vurgulanmış kodu gerektirir `HTTPS`; bu nedenle, HTTP isteklerini yok sayılır. Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).

HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır. Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfalarının özniteliğine değil olarak kabul güvenli olarak genel HTTPS gerektiren. Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfalarının eklendiğinde uygulanır.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP katı Aktarım güvenlik protokolünü (HSTS)

Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı Aktarım güvenlik (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) özel yanıt üstbilgisi kullanımı ile bir web uygulaması tarafından belirtilen bir güvenlik katılımı yeniliktir. Bu üst desteklenen bir tarayıcı aldıktan sonra bu tarayıcı iletişimlerle belirtilen etki alanı için HTTP üzerinden gönderilmesini engeller ve bunun yerine tüm iletişimler HTTPS üzerinden gönderir. Ayrıca, HTTPS istekleri tarayıcılarda geçişli tıklatma önler.

ASP.NET Core 2.1 veya sonrası ile HSTS uygulayan `UseHsts` genişletme yöntemi. Aşağıdaki kod çağrıları `UseHsts` zaman uygulama değil de [geliştirme modunu](xref:fundamentals/environments):

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` HSTS üstbilgisi tarayıcılar tarafından yüksek oranda cachable olduğundan öneri geliştirme değil. Varsayılan olarak, yerel bir geri döngü adresine UseHsts dışlar.

Aşağıdaki kod:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Strict Aktarım güvenlik üstbilgisi önyüklemesi parametresinin ayarlar. Önyükleme değil parçası [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeniden yüklemeyi HSTS sitelerinde önceden yüklemek için web tarayıcısı tarafından desteklenir, ancak. Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.
* Etkinleştirir [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilkesi uygulanır. 
* Açıkça katı taşıma güvenliği üstbilgiye max-age parametresinin 60 gün olarak ayarlar. Aksi durumda ayarlanırsa, varsayılan olarak 30 gün. Bkz: [, max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.
* Ekler `example.com` dışlamak için ana bilgisayarlar listesine.

`UseHsts` şu geri döngü konakları dışlar:

* `localhost` : IPv4 geri döngü adresi.
* `127.0.0.1` : IPv4 geri döngü adresi.
* `[::1]` : IPv6 geri döngü adresi.

Önceki örnekte, ek ana bilgisayar eklemek gösterilmiştir.
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Üyelikten çıkmak HTTPS projesi oluşturma

Sonraki web uygulama şablonlardan (Visual Studio veya dotnet komut satırı) ve ASP.NET Core 2.1 etkinleştirmek [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts). HTTPS gerektirmeyen dağıtımları için HTTPS çevirin. Örneğin, burada HTTPS dışarıdan sınırında her düğümde HTTPS kullanarak işlenen bazı arka uç hizmetlerinin gerekli değildir.

HTTPS çevirin için:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

İşaretini **HTTPS için Yapılandır** onay kutusu.

![Varlık diyagramı](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Kullanım `--no-https` seçeneği. Örneğin

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Bir geliştirici sertifikası için Docker ayarlama

Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
