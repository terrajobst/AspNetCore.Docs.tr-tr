---
title: ASP.NET core'da oturum ve uygulama durumu
author: rick-anderson
description: İstekleri arasında oturum ve uygulama durumunu korumak için yaklaşımları keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 072699113a45056ec3ea79436ad56896ba0a4197
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095820"
---
# <a name="session-and-app-state-in-aspnet-core"></a>ASP.NET core'da oturum ve uygulama durumu

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), ve [Luke Latham](https://github.com/guardrex)

HTTP durum bilgisi olmayan bir protokoldür. Ek adımlar yapmadan kullanıcı değerleri veya uygulama durumunu tutmaz bağımsız mesaj HTTP isteği adı verilir. Bu makalede, istekler arasında kullanıcı verileri ve uygulama durumunu korumak için çeşitli yaklaşımlar açıklanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="state-management"></a>Durum Yönetimi

Durum, çeşitli yaklaşımlar kullanılarak depolanabilir. Her bir yaklaşım, bu konunun ilerleyen bölümlerinde açıklanmıştır.

| Depolama yaklaşımı | Depolama mekanizması |
| ---------------- | ----------------- |
| [Tanımlama bilgileri](#cookies) | HTTP tanımlama bilgileri (sunucu tarafı uygulama kodu kullanarak depolanan veriler dahil) |
| [Oturum durumu](#session-state) | HTTP tanımlama bilgileri ve sunucu tarafı uygulama kodu |
| [TempData](#tempdata) | HTTP tanımlama bilgileri veya oturum durumu |
| [Sorgu dizeleri](#query-strings) | HTTP sorgu dizeleri |
| [Gizli alanları](#hidden-fields) | HTTP form alanları |
| [HttpContext.Items](#httpcontextitems) | Sunucu tarafı uygulama kodu |
| [Önbellek](#cache) | Sunucu tarafı uygulama kodu |
| [Bağımlılık Ekleme](#dependency-injection) | Sunucu tarafı uygulama kodu |

## <a name="cookies"></a>Tanımlama bilgileri

Tanımlama bilgileri, istekler arasında verileri depolar. Tanımlama bilgileri her bir istekle gönderildiğinden, bunların boyutu için en az tutulmalıdır. İdeal olarak, yalnızca bir tanımlayıcı bir tanımlama bilgisinde, uygulama tarafından depolanan verilerle depolanmalıdır. Tarayıcılarının çoğu tanımlama bilgisi boyutu 4096 bayt ile sınırlandırır. Yalnızca sınırlı sayıda tanımlama bilgileri, her etki alanı için kullanılabilir.

Tanımlama bilgilerini oynama tabi olduğu için uygulama tarafından doğrulanmalıdır. Tanımlama bilgileri, kullanıcılar tarafından silinebilir ve istemcilerde süresi dolar. Ancak, tanımlama bilgileri genellikle en sağlam istemci üzerindeki veri kalıcılığı biçimindedir.

Tanımlama bilgileri, genellikle içeriği bilinen bir kullanıcı için burada özelleştirilmiş kişiselleştirme için kullanılır. Kullanıcı yalnızca tanımlanan ve çoğu zaman kimliği doğrulanmış değil. Tanımlama bilgisi, kullanıcının adı, hesap adı veya benzersiz kullanıcı kimliği (örneğin, bir GUID) depolayabilirsiniz. Tanımlama bilgisi ardından kendi tercih edilen Web sitesi arka plan rengi gibi kullanıcının kişiselleştirilmiş ayarlara erişmek için de kullanabilirsiniz.

Dikkatli olmanızı [Avrupa Birliği genel veri koruma yönetmeliklerine (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) tanımlama bilgileri vermek ve gizlilikle ilgili ne zaman ilgilidir. Daha fazla bilgi için [ASP.NET Core genel veri koruma yönetmeliği (GDPR) desteği](xref:security/gdpr).

## <a name="session-state"></a>Oturum durumu

Kullanıcının bir web uygulaması gözatar sırada oturum durumu kullanıcı verilerinin depolanması için bir ASP.NET Core senaryo ' dir. Oturum durumu, istemciden gelen isteklere arasında verileri kalıcı hale getirmek için uygulama tarafından korunan bir depolama kullanır. Oturum verilerini bir önbelleği tarafından desteklenen ve kısa ömürlü veri kabul&mdash;site oturum verileri çalışmaya devam etmelidir.

> [!NOTE]
> Oturum desteklenmiyor [SignalR](xref:signalr/index) uygulamaları çünkü bir [SignalR hub'ı](xref:signalr/hubs) bağımsız bir HTTP bağlamını yürütür. Örneğin, bir uzun olduğunda ortaya çıkabilir yoklama isteği açık tarafından tutulan isteğin HTTP bağlamını ömrünü ötesinde bir hub'ı.

ASP.NET Core, uygulamanın her istek ile gönderilen bir oturum kimliği içeren istemciye bir tanımlama bilgisi sağlayarak oturum durumu korur. Uygulama, oturum verilerini almak için oturum kimliği kullanır.

Oturum durumu aşağıdaki davranışları sergileyen:

* Oturum tanımlama bilgisinin tarayıcıya belirli olduğundan, oturumları tarayıcılar arasında paylaşılmaz.
* Tarayıcı oturumu sona erdiğinde oturum tanımlama bilgileri silinir.
* Süresi dolmuş bir oturum için bir tanımlama bilgisi alınmazsa, aynı oturum tanımlama bilgisini kullanan yeni bir oturum oluşturulur.
* Boş oturumlar olmayan korunur&mdash;oturum içine istekler genelinde oturumu kalıcı olarak en az bir değere sahip olmalıdır. Bir oturum korunmaz, yeni oturum kimliği her yeni isteği için oluşturulur.
* Uygulamayı sınırlı bir süre sonra son istek için bir oturum korur. Uygulama oturum zaman aşımı ayarlar veya 20 dakikalık varsayılan değeri kullanır. Oturum durumu, belirli bir oturum için özeldir ancak burada veri oturumlarda kalıcı depolama alanı gerektirmez kullanıcı verilerini depolamak için idealdir.
* Oturum verileri silinir ya da [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) oturum süresinin sona erdiği veya uygulama çağrılır.
* Uygulama kodu istemci tarayıcısı kapatıldı veya ne zaman oturum tanımlama bilgisi silindi veya istemcide süresi size bildirmek için varsayılan bir mekanizma yoktur.

> [!WARNING]
> Hassas verileri kavramak depolamayın. Kullanıcı olmayan Tarayıcıyı kapatın ve oturum tanımlama bilgisini temizlemek. Bazı tarayıcılar tarayıcı pencereleri arasında geçerli bir oturum tanımlama bilgileri korur. Bir oturum için tek bir kullanıcı kısıtlı olmayabilir&mdash;sonraki kullanıcı aynı oturum tanımlama bilgisinin uygulamayla göz atmak devam edebilir.

Bellek içi önbelleği sağlayıcısı uygulama bulunduğu sunucunun bellekte oturum verilerini depolar. Bir sunucu grubu senaryoda:

* Kullanım *Yapışkan oturumlar* tek bir sunucu üzerinde her oturum belirli uygulama örneğine bağlamak için. [Azure App Service](https://azure.microsoft.com/services/app-service/) kullanan [uygulama isteği yönlendirme (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) Yapışkan oturumlar varsayılan olarak zorunlu kılmak için. Ancak, Yapışkan oturumlar ölçeklenebilirliği etkileyebilir ve web uygulama güncelleştirmeleri zorlaştırabilir. Bir Redis veya SQL Server kullanmak için daha iyi bir yaklaşım olan dağıtılmış önbellek, Yapışkan oturumlar gerektirmez. Daha fazla bilgi için [dağıtılmış Önbellekle çalışma](xref:performance/caching/distributed).
* Oturum tanımlama bilgisi ile şifrelenmiş [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). Veri koruma, her bir makinede oturum tanımlama bilgileri okumak için düzgün şekilde yapılandırılmalıdır. Daha fazla bilgi için [ASP.NET Core veri koruması](xref:security/data-protection/index) ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).

### <a name="configure-session-state"></a>Oturum durumunu yapılandırın

::: moniker range=">= aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) dahil edilen paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), oturum durumu yönetmek için bir ara yazılım sağlar. Oturum ara yazılım etkinleştirmek için `Startup` içermelidir:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) paket, oturum durumu yönetmek için bir ara yazılım sağlar. Oturum ara yazılım etkinleştirmek için `Startup` içermelidir:

::: moniker-end

* Herhangi bir [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) bellek önbelleğe alır. `IDistributedCache` Uygulama oturumu için bir yedekleme deposu kullanılır. Daha fazla bilgi için [dağıtılmış Önbellekle çalışma](xref:performance/caching/distributed).
* Bir çağrı [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) içinde `ConfigureServices`.
* Bir çağrı [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) içinde `Configure`.

Aşağıdaki kod, varsayılan bir bellek içi uygulama ile bellek içi oturum sağlayıcısını ayarlama işlemi gösterilmektedir `IDistributedCache`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

Ara yazılım sırası önemlidir. Yukarıdaki örnekte, bir `InvalidOperationException` özel durum oluşursa, `UseSession` sonra çağrılan `UseMvc`. Daha fazla bilgi için [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering).

[İçeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) oturum durumu yapılandırıldıktan sonra kullanılabilir.

`HttpContext.Session` önce erişilemez `UseSession` çağrıldı.

Uygulama yanıt akışına yazma başladıktan sonra yeni oturum tanımlama bilgisi ile yeni bir oturum oluşturulamıyor. Özel durum web sunucusu günlüğe kaydedilen ve tarayıcıda gösterilmez.

### <a name="load-session-state-asynchronously"></a>Oturum durumunu zaman uyumsuz olarak yükleme

Temel oturum kayıtları ASP.NET Core varsayılan oturum sağlayıcısında yükler [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) yedekleme deposu zaman uyumsuz olarak yalnızca şu durumlarda [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) yöntemi açıkça çağrılır önce [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ayarlamak](/dotnet/api/microsoft.aspnetcore.http.isession.set), veya [Kaldır](/dotnet/api/microsoft.aspnetcore.http.isession.remove) yöntemleri. Varsa `LoadAsync` ilk olarak, temel olarak adlandırılmaz oturumu kayıt yüklendiği zaman uyumlu olarak, hangi ölçekte bir performans cezasına sebep.

Bu düzen zorunlu uygulamanız için sarmalamanız [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) ve [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) uygulamaları bağlanamazsa özel durum sürümleri ile `LoadAsync` değil yönteminden önce `TryGetValue`, `Set`, veya `Remove`. Sarmalanan sürümleri Hizmetleri kapsayıcısında kaydedin.

### <a name="session-options"></a>Oturum seçenekleri

Oturum Varsayılanları geçersiz kılmak için kullanın [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

::: moniker range=">= aspnetcore-2.0"

| Seçenek | Açıklama |
| ------ | ----------- |
| [Tanımlama bilgisi](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Tanımlama bilgisi oluşturmak için kullanılan ayarları belirler. [Adı](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) varsayılan olarak [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). [Yol](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) varsayılan olarak [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`. [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) varsayılan olarak `false`. |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` İçeriğini terk önce ne kadar süreyle oturumun boşta kalabileceği gösterir. Her oturum erişimi zaman aşımı sıfırlar. Bu, yalnızca oturum tanımlama içeriği geçerlidir unutmayın. Varsayılan değer 20 dakikadır. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | En yüksek süreyi Mağaza'dan oturum yüklemek veya geri depoya kaydetmeye izin. Bu, yalnızca zaman uyumsuz işlemler için uygulanabilir unutmayın. Bu zaman aşımı kullanarak devre dışı bırakılabilir [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). Varsayılan değer 1 dakikadır. |

Oturum tanımlama bilgisi istekleri tek bir tarayıcıdan belirlemek ve izlemek için kullanır. Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNetCore.Session`, ve bir yol kullanır `/`. Tanımlama bilgisinin varsayılan bir etki alanını belirtmeyen çünkü, istemci tarafı komut dosyası için sayfada kullanılabilir değil (çünkü [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Seçenek | Açıklama |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | Tanımlama bilgisi oluşturmak için kullanılan etki alanını belirler. `CookieDomain` Varsayılan olarak ayarlanmamış. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | Tarayıcı tanımlama bilgisine istemci tarafı JavaScript tarafından erişilecek izin veriyorsa belirler. Varsayılan `true`, tanımlama bilgisi başka bir deyişle, yalnızca HTTP isteklerine geçirilir ve sayfaya betiğe kullanılabilir hale değil. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | Oturum kimliği kalıcı hale getirmek için kullanılan tanımlama bilgisi adını belirler Varsayılan değer [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | Tanımlama bilgisi oluşturmak için kullanılan yolu belirler. Varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | Tanımlama bilgisinin yalnızca HTTPS isteklerini iletilip iletilmeyeceğini belirler. Varsayılan değer [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`). |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` İçeriğini terk önce ne kadar süreyle oturumun boşta kalabileceği gösterir. Her oturum erişimi zaman aşımı sıfırlar. Bu, yalnızca oturum tanımlama içeriği geçerlidir unutmayın. Varsayılan değer 20 dakikadır. |

Oturum tanımlama bilgisi istekleri tek bir tarayıcıdan belirlemek ve izlemek için kullanır. Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNet.Session`, ve bir yol kullanır `/`.

::: moniker-end

Tanımlama bilgisi oturumu Varsayılanları geçersiz kılmak için kullanın `SessionOptions`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

Uygulamanın kullandığı [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) sunucusunun önbelleğine içeriğini terk önce ne kadar bir oturumun boşta kalabileceği belirlemek için özellik. Bu özellik, tanımlama bilgisi süre sonu bağımsızdır. Geçtiği her isteğin [oturumu ara yazılım](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) zaman aşımı sıfırlar.

Oturum durumu *kilitlenmeyen*. Son istek, iki isteği aynı anda bir oturumu içeriğini değiştirme girişimi, ilk geçersiz kılar. `Session` olarak uygulanan bir *tutarlı oturumu*, yani tüm içeriği birlikte depolanır. Farklı oturuma değerlerini değiştirmek iki istek arama, son istek oturumu değişikliklerinin ilk tarafından geçersiz kılabilir.

### <a name="set-and-get-session-values"></a>Ayarlayın ve oturumu değerlerini alma

Oturum durumu, bir Razor sayfaları erişildiğinde [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sınıfı veya MVC [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller) sınıfıyla [içeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Bu özellik bir [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) uygulaması.

::: moniker range=">= aspnetcore-2.0"

`ISession` Uygulamasını tamsayı ve dize değerleri ayarlama ve alma için birkaç genişletme yöntemleri sağlar. Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (ekleme bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişim kazanmak için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor. Her iki paketi de dahil edilen [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`ISession` Uygulamasını tamsayı ve dize değerleri ayarlama ve alma için birkaç genişletme yöntemleri sağlar. Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (ekleme bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişim kazanmak için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor.

::: moniker-end

`ISession` Uzantı yöntemleri:

* [Get (ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString (ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [Setınt32 (ISession, dize, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString (ISession, String, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

Aşağıdaki örnekte oturum değerini alır. `IndexModel.SessionKeyName` anahtarı (`_Name` örnek uygulamada) bir Razor sayfaları sayfasında:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

Aşağıdaki örnek, tamsayı ve bir dize almak ve ayarlamak gösterilmektedir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

Bellek içi önbelleği kullanırken bile bir dağıtılmış önbellek senaryoyu etkinleştirmek için tüm oturum verilerini seri hale getirilmelidir. En az dize ve sayı seri hale getiricileri genişletme sağlanır (uzantı yöntemleri ve yöntemler bkz [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). JSON gibi başka bir mekanizma kullanarak kullanıcı tarafından karmaşık türler seri hale getirilmelidir.

Serileştirilebilir nesneler almak ve ayarlamak için aşağıdaki uzantı yöntemlerini ekleyin:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

Aşağıdaki örnek, genişletme yöntemleri ile seri hale getirilebilir bir nesne almak ve ayarlamak gösterilmektedir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core sunan [Razor sayfaları sayfa modeli TempData özelliği](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) veya [MVC denetleyicisinin TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata). Bu özellik, okunur kadar verileri depolar. [Tutmak](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) ve [Özet](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) yöntemleri silme olmadan verileri incelemek için kullanılabilir. Veriler birden çok tek bir istek için gerekli olduğunda TempData yeniden yönlendirme için özellikle yararlıdır. TempData tanımlama veya oturum durumu kullanarak TempData sağlayıcıları tarafından uygulanır.

### <a name="tempdata-providers"></a>TempData sağlayıcıları

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core 2.0 veya sonraki sürümlerde, tanımlama bilgisi tabanlı TempData sağlayıcısı TempData tanımlama bilgilerinizde depolamak için varsayılan olarak kullanılır.

Tanımlama bilgisi verisi kullanılarak şifrelenir [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), ile kodlanmış [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), sonra parçalı. Tanımlama bilgisi öbekli çünkü tek bir tanımlama bilgisi ASP.NET Core 1.x uygulanmaz bulunan sınırı boyutu. Şifrelenmiş verileri sıkıştırmak için güvenlik sorunları gibi neden olabileceği için tanımlama bilgisi verisi sıkıştırılmaz [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları. Tanımlama bilgisi tabanlı TempData sağlayıcısı hakkında daha fazla bilgi için bkz. [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.0 ve 1.1, oturum durumu TempData sağlayıcısı varsayılan sağlayıcıdır.

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>TempData sağlayıcısını seçin

TempData sağlayıcısı seçme gibi çeşitli konuları içerir:

1. Uygulama zaten oturum durumu kullanıyor mu? Bu durumda, oturum durumu TempData sağlayıcısını kullanarak ek ücret ödemeden (yanı sıra veri boyutu) uygulamasına sahiptir.
2. Uygulamayı yalnızca gelişigüzel TempData kullanmaz için görece küçük miktarlarda veri (bayt cinsinden en fazla 500)? Bu nedenle, tanımlama bilgisi TempData sağlayıcı her istek için küçük bir maliyet eklerse, TempData taşır. Aksi durumda, oturum durumu TempData sağlayıcısı TempData tüketilen kadar gidiş dönüşü her isteğin verilerde büyük miktarda önlemek yararlı olabilir.
3. Uygulamayı bir sunucu grubunda birden çok sunucu üzerinde çalışıyor mu? Bu nedenle, varsa tanımlama bilgisi TempData sağlayıcısı veri koruma dışında kullanmak için ek yapılandırma (bkz [veri koruma](xref:security/data-protection/index) ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers)).

> [!NOTE]
> Çoğu web istemcileri (örneğin, web tarayıcıları) her bir tanımlama bilgisi, tanımlama bilgilerinin toplam sayısı veya her ikisi de en büyük boyutu üst sınırı uygular. Tanımlama bilgisi TempData sağlayıcı kullanırken, uygulama bu sınırları aşmanız olmaz doğrulayın. Verilerin toplam boyutuna göz önünde bulundurun. Hesap için şifreleme ve Öbekleme nedeniyle tanımlama bilgisi boyutu artar.

### <a name="configure-the-tempdata-provider"></a>TempData sağlayıcısını yapılandırma

::: moniker range=">= aspnetcore-2.0"

Tanımlama bilgisi tabanlı TempData sağlayıcısı varsayılan olarak etkindir.

Oturum tabanlı TempData sağlayıcıyı etkinleştirmek için [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) genişletme yöntemi:

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aşağıdaki `Startup` sınıf kodunu oturum tabanlı TempData sağlayıcısı yapılandırır:

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

Ara yazılım sırası önemlidir. Yukarıdaki örnekte, bir `InvalidOperationException` özel durum oluşursa, `UseSession` sonra çağrılan `UseMvc`. Daha fazla bilgi için [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> .NET Framework'ü hedefleyen ve oturum tabanlı TempData sağlayıcısını kullanarak eklerseniz [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) projeye paket.

## <a name="query-strings"></a>Sorgu dizeleri

Sınırlı miktarda veri bir istekten başka bir yeni isteğin sorgu dizesi olanağı ekleyerek geçirilebilir. Bu, gömülü durumuyla e-posta veya sosyal ağlar paylaşılan bağlantılara izin verir, kalıcı bir şekilde durumu yakalamak için yararlıdır. Hiçbir zaman URL'si sorgu dizeleri ortak olduğundan, hassas verileri için sorgu dizelerini kullanın.

İstenmeyen paylaşmaya ek olarak, sorgu dizelerine veriler dahil olmak üzere olanaklarını oluşturabilirsiniz [siteler arası istek sahteciliği (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) kullanıcıların kimlik doğrulaması sırasında kötü amaçlı siteleri ziyaret etmeye ikna saldırıları. Saldırganların uygulamadaki kullanıcı verilerini çalabilir veya kullanıcı adına kötü amaçlı eylemler. Herhangi bir korunan bir uygulama veya oturum durumu CSRF saldırılarına karşı korumanız gerekir. Daha fazla bilgi için [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).

## <a name="hidden-fields"></a>Gizli alanları

Veri gizli form alanları kaydedilebilir ve bir sonraki istekte geri gönderildi. Bu, çok sayfalı formlarında yaygındır. İstemci, olası verilerle değiştirmesine çünkü uygulama gizli alanlar depolanan veriler her zaman düzeltin gerekir.

## <a name="httpcontextitems"></a>HttpContext.Items

[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) koleksiyonu tek bir isteği işlerken veri depolamak için kullanılır. İstek işlendikten sonra koleksiyonun içeriği atılır. `Items` Bileşenleri veya ara yazılım istek sırada, farklı noktalarda çalışır ve parametreleri geçirmek için doğrudan hiçbir şekilde iletişim kurmasına izin vermek için genellikle koleksiyonu kullanılır.

Aşağıdaki örnekte, [ara yazılım](xref:fundamentals/middleware/index) ekler `isVerified` için `Items` koleksiyonu.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

İşlem hattı daha sonra başka bir ara yazılım değerini erişip `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

Yalnızca tek bir uygulama tarafından kullanılan ara yazılımı `string` anahtarları kabul edilebilir. Uygulama örnekleri arasında paylaşılan bir ara yazılım, anahtar çarpışmalarını için benzersiz nesne anahtarları kullanmanız gerekir. Aşağıdaki örnek, bir ara yazılım sınıfı içinde tanımlanan bir benzersiz nesne anahtarı kullanma işlemini gösterir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

Diğer kod içinde depolanan değeri erişip `HttpContext.Items` ara yazılım sınıfı tarafından kullanıma sunulan anahtarını kullanarak:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

Bu yaklaşım, aynı zamanda koddaki anahtar dizeleri kullanımı ortadan avantajına sahiptir.

## <a name="cache"></a>Önbellek

Önbelleğe alma, veri depolayıp almak için etkili bir yoldur. Uygulama, önbelleğe alınmış öğeleri ömrünü kontrol edebilirsiniz.

Önbelleğe alınan veriler belirli bir istek, kullanıcı veya oturum ile ilişkili değil. **Dikkatli olun diğer kullanıcıların isteklerinden alınan önbellek kullanıcıya özgü verilere değil.**

Daha fazla bilgi için [yanıtları önbelleğe](xref:performance/caching/index) konu.

## <a name="dependency-injection"></a>Bağımlılık ekleme

Kullanım [bağımlılık ekleme](xref:fundamentals/dependency-injection) veri tüm kullanıcılar için kullanılabilir hale getirmek için:

1. Veri içeren bir hizmet tanımlar. Örneğin, adında bir sınıf `MyAppData` tanımlanır:

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. Hizmet sınıfa eklemek `Startup.ConfigureServices`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. Veri Hizmeti sınıfındaki kullan:

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a>Sık karşılaşılan hatalar

* "Hizmet türü 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' için 'Microsoft.AspNetCore.Session.DistributedSessionStore' etkinleştirmeye çalışılırken çözmek alınamıyor."

  Bu en az bir yapılandırma devrederek genellikle kaynaklanır `IDistributedCache` uygulaması. Daha fazla bilgi için [dağıtılmış Önbellekle çalışma](xref:performance/caching/distributed) ve [bellek içi önbelleğe alma](xref:performance/caching/memory).

* Ara yazılım başarısız oturum (örneğin, yedekleme deposunun kullanılabilir durumda değilse) bir oturumu kalıcı olayda ara yazılım özel durumu günlüğe kaydeder ve isteğin normal şekilde devam eder. Bu beklenmeyen davranışa yol açar.

  Örneğin, bir kullanıcı oturumunda bir alışveriş sepeti depolar. Kullanıcı bir öğeyi sepete ekler, ancak yürütme başarısız. Öğesi true değilse, sepetine eklendi kullanıcıya raporların uygulama başarısızlığı hakkında bilmez.

  Hataları denetlemek için önerilen yaklaşım çağırmaktır `await feature.Session.CommitAsync();` uygulama bittiğinde uygulama kodundan oturuma yazma. `CommitAsync` yedekleme deposu kullanılamıyorsa bir özel durum oluşturur. Varsa `CommitAsync` başarısız olursa, uygulama özel durumu işleyebilir. `LoadAsync` veri deposu kullanılamaz olduğu aynı koşullarda oluşturur.

## <a name="additional-resources"></a>Ek kaynaklar

<xref:host-and-deploy/web-farm>
