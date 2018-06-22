---
title: ASP.NET Core oturum ve uygulama durumu
author: rick-anderson
description: İstekler arasında oturum ve uygulama durumunu korumak için yaklaşım bulur.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 9c63d9313acb055e6c692a7fef3d28e94cb37093
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272889"
---
# <a name="session-and-app-state-in-aspnet-core"></a>ASP.NET Core oturum ve uygulama durumu

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), ve [Luke Latham](https://github.com/guardrex)

HTTP durum bilgisi olmayan bir protokoldür. Ek adımlar bırakmadan HTTP isteklerini kullanıcı değerleri veya uygulama durumu korumaz bağımsız iletileri edilir. Bu makalede istekler arasında kullanıcı verilerini ve uygulama durumunu korumak için çeşitli yaklaşımlar açıklanmaktadır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="state-management"></a>Durum Yönetimi

Durum, çeşitli yaklaşımlar kullanılarak depolanabilir. Her iki yaklaşımın bu konunun ilerleyen bölümlerinde açıklanmıştır.

| Depolama yaklaşımı | Depolama mekanizması |
| ---------------- | ----------------- |
| [Tanımlama bilgileri](#cookies) | HTTP tanımlama bilgilerini (sunucu tarafı uygulama kodu kullanarak depolanan veriler dahil) |
| [oturum durumu](#session-state) | HTTP tanımlama bilgilerini ve sunucu tarafı uygulama kodu |
| [TempData](#tempdata) | HTTP tanımlama bilgilerini veya oturum durumu |
| [Sorgu dizeleri](#query-strings) | HTTP sorgu dizeleri |
| [Gizli alanlar](#hidden-fields) | HTTP form alanları |
| [HttpContext.Items](#httpcontextitems) | Sunucu tarafı uygulama kodu |
| [Önbellek](#cache) | Sunucu tarafı uygulama kodu |
| [Bağımlılık Ekleme](#dependency-injection) | Sunucu tarafı uygulama kodu |

## <a name="cookies"></a>Tanımlama bilgileri

Tanımlama bilgilerini istekler genelinde verileri depolar. Tanımlama bilgilerini içeren tüm istekleri gönderildiğinden, kendi boyutu en az olarak tutulmalıdır. İdeal olarak, yalnızca bir tanımlayıcı bir tanımlama bilgisinde uygulama tarafından depolanan verilerle depolanması gerekir. Çoğu tarayıcı tanımlama bilgisi boyutu 4096 bayt kısıtlayın. Yalnızca sınırlı sayıda tanımlama bilgileri, her etki alanı için kullanılabilir.

Tanımlama bilgilerini oynama tabi olduğundan, uygulama tarafından doğrulanmalıdır. Tanımlama bilgileri, kullanıcılar tarafından silinebilir ve istemcilerde süresi dolacak. Ancak, tanımlama bilgileri genellikle en sağlam istemci üzerindeki veri kalıcılığını biçimidir.

Tanımlama bilgileri, genellikle içeriği bilinen bir kullanıcı için burada özelleştirilmiş kişiselleştirme için kullanılır. Kullanıcı yalnızca tanımlanan ve çoğu durumda kimliği değil. Tanımlama bilgisi, kullanıcının adı, hesap adı veya benzersiz bir kullanıcı kimliği (örneğin, bir GUID) depolayabilirsiniz. Tanımlama bilgisinin sonra kendi tercih edilen Web sitesi arka plan rengi gibi kullanıcının kişiselleştirilmiş ayarlarına erişmek için de kullanabilirsiniz.

Dikkatli olmanızı [Avrupa Birliği genel veri koruma düzenlemeleri (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) zaman tanımlama bilgilerini vermek ve gizlilikle ilgili ilgilidir. Daha fazla bilgi için bkz: [ASP.NET Core genel veri koruma düzenleme (GDPR) desteği](xref:security/gdpr).

## <a name="session-state"></a>oturum durumu

Kullanıcının bir web uygulaması gözatar sırada oturum durumu ASP.NET Core kullanıcı verilerinin depolanması için bir senaryodur. Oturum durumu, istemciden gelen isteklere arasında veri kalıcı hale getirmek için uygulama tarafından korunan bir depolama kullanır. Oturumunun veri önbelleği tarafından yedeklenen ve kısa ömürlü veriler kabul&mdash;site oturum verilerini çalışmaya devam etmelidir.

> [!NOTE]
> Oturum desteklenmiyor [SignalR](xref:signalr/index) uygulamalar için bir [SignalR hub'ı](xref:signalr/hubs) bağımsız bir HTTP bağlamını yürütür. Örneğin, bir uzun zaman oluşabilir yoklama isteği tutulan açık isteğin HTTP bağlamını ömür ötesinde hub tarafından.

ASP.NET Core her istek ile uygulamaya gönderilen bir oturum kimliği içeren bir istemciye bir tanımlama bilgisi sağlayarak oturum durumunu korur. Uygulamanın, oturum verilerini getirmek için oturum kimliği kullanır.

Oturum durumu aşağıdaki davranışları sergiler:

* Oturum tanımlama bilgisi tarayıcıya özgü olduğundan, oturumlar tarayıcılar arasında paylaşılmaz.
* Tarayıcı oturumu sona erdiğinde oturum tanımlama bilgileri silinir.
* Süresi dolmuş bir oturum için bir tanımlama bilgisi alınmazsa, aynı oturum tanımlama bilgisi kullanan yeni bir oturum oluşturulur.
* Boş oturumları olmayan korunur&mdash;oturum içine oturum istekler genelinde kalıcı olması için ayarlama en az bir değere sahip olmalıdır. Bir oturumu korunur değil, yeni bir oturum kimliği her yeni bir istek için oluşturulur.
* Uygulama, son istekten sonra sınırlı bir süre için bir oturum korur. Uygulamanın oturum zaman aşımı ayarlar veya 20 dakikalık varsayılan değerini kullanır. Oturum durumu, belirli bir oturum için belirli ancak veri oturumlarında kalıcı depolama burada gerektirmeyen kullanıcı verilerini depolamak için idealdir.
* Oturum verileri silinir ya da zaman [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) uygulama çağrılmadan veya oturumun süresi dolduğunda.
* Bir istemci tarayıcısı kapatıldı veya ne zaman oturum tanımlama bilgisi silinmiş veya istemcide süresi uygulama kodu bildirmek için varsayılan bir mekanizma yoktur.

> [!WARNING]
> Hassas verileri kavramak depolamayın. Kullanıcı olmayan Tarayıcıyı kapatın ve oturum tanımlama bilgisi temizleyin. Bazı tarayıcılar tarayıcı pencerelerini arasında geçerli bir oturum tanımlama bilgileri korur. Bir oturum için tek bir kullanıcı kısıtlı olmayabilir&mdash;sonraki kullanıcının aynı oturum tanımlama bilgisiyle uygulama göz atmak devam edebilir.

Bellek içi önbellek sağlayıcı uygulama bulunduğu sunucu belleğini oturum verilerini depolar. Bir sunucu grubu senaryoda:

* Kullanım *Yapışkan oturumları* her oturum belirli uygulama örneği için tek bir sunucu üzerinde bağlamanın. [Azure uygulama hizmeti](https://azure.microsoft.com/services/app-service/) kullanan [uygulama isteği yönlendirme (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) Yapışkan oturumlar varsayılan olarak zorlamak için. Ancak, Yapışkan oturumları ölçeklenebilirliği etkileyen ve web uygulama güncelleştirmeleri zorlaştırabilir. Bir Redis veya SQL Server kullanmak için daha iyi bir yaklaşım olduğu dağıtılmış Yapışkan oturumları gerektirmeyen önbellek. Daha fazla bilgi için bkz: [iş dağıtılmış önbellek ile](xref:performance/caching/distributed).
* Oturum tanımlama bilgisi aracılığıyla şifrelenmiş [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). Veri koruma, her makinede oturum tanımlama bilgileri okumak için düzgün şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz: [ASP.NET Çekirdeği'nde veri koruma](xref:security/data-protection/index) ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).

### <a name="configure-session-state"></a>Oturum durumunu yapılandırın

::: moniker range=">= aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) yer aldığı paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), oturum durumunu yönetmek için ara yazılım sağlar. Oturum Ara etkinleştirmek için `Startup` içermelidir:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) paket oturum durumu yönetmek için ara yazılım sağlar. Oturum Ara etkinleştirmek için `Startup` içermelidir:

::: moniker-end

* Herhangi bir [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) bellek önbellekleri. `IDistributedCache` Uygulama oturumu için bir yedekleme deposu olarak kullanılır. Daha fazla bilgi için bkz: [iş dağıtılmış önbellek ile](xref:performance/caching/distributed).
* Çağrı [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) içinde `ConfigureServices`.
* Çağrı [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) içinde `Configure`.

Aşağıdaki kod, varsayılan bir bellek içi uygulama bellek içi oturum sağlayıcısı ayarlanacağı gösterilmiştir `IDistributedCache`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

Ara yazılım sıralama önemlidir. Önceki örnekte, bir `InvalidOperationException` özel durum oluştu, `UseSession` sonra çağrılan `UseMvc`. Daha fazla bilgi için bkz: [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering).

[İçeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) oturum durumu yapılandırıldıktan sonra kullanılabilir.

`HttpContext.Session` önce erişilemez `UseSession` çağrıldı.

Uygulama yanıt akışına yazmak başladıktan sonra yeni bir oturum tanımlama ile yeni bir oturum oluşturulamıyor. Özel durum web sunucusu günlüğüne kaydedilir ve tarayıcıda görüntülenen değil.

### <a name="load-session-state-asynchronously"></a>Oturum durumu zaman uyumsuz olarak yükleme

Arka plandaki gelen oturum kayıtları ASP.NET Core varsayılan oturum sağlayıcısında yükler [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) yedekleme deposu zaman uyumsuz olarak yalnızca IF [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) yöntemi açıkça çağrılan önce [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ayarlamak](/dotnet/api/microsoft.aspnetcore.http.isession.set), veya [kaldırmak](/dotnet/api/microsoft.aspnetcore.http.isession.remove) yöntemleri. Varsa `LoadAsync` ilk olarak, temel olarak adlandırılmaz oturum kayıt yüklendiği zaman uyumlu olarak, hangi ölçekte performans cezasına sebep.

Uygulamaların bu deseni zorlamak için Kaydır [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) ve [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) uygulamaları, bir özel durum sürümleri ile `LoadAsync` değil yöntemi çağrılmadan önce `TryGetValue`, `Set`, veya `Remove`. Sarmalanan sürümleri hizmetler kapsayıcısının kaydedin.

### <a name="session-options"></a>Oturum seçenekleri

Oturum Varsayılanları geçersiz kılmak için kullanın [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

::: moniker range=">= aspnetcore-2.0"

| Seçenek | Açıklama |
| ------ | ----------- |
| [Tanımlama bilgisi](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Tanımlama bilgisi oluşturmak için kullanılan ayarları belirler. [Ad](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) varsayılan olarak [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). [Yol](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) varsayılan olarak [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`. [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) varsayılan olarak `false`. |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` İçeriğinin terk önce ne kadar süreyle oturum boşta kalabileceği gösterir. Her oturum erişimini zaman aşımı sıfırlar. Bu, yalnızca oturum olmayan tanımlama bilgisinin içeriğini geçerlidir unutmayın. Varsayılan değer 20 dakikadır. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | En yüksek süreyi Mağaza'dan oturum yüklemeye veya geri deposuna kaydetmek için izin. Bu yalnızca zaman uyumsuz işlemleri için geçerli unutmayın. Bu zaman aşımı kullanarak devre dışı bırakılabilir [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). Varsayılan değer 1 dakikadır. |

Oturum tanımlama bilgisi izlemek ve tek bir tarayıcıdan istekleri belirlemek için kullanır. Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNetCore.Session`, ve bir yol kullanır `/`. Tanımlama bilgisi varsayılan etki alanı belirtmiyor olduğundan, istemci tarafı komut dosyası için kullanılabilir sayfada yapılan değil (çünkü [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Seçenek | Açıklama |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | Tanımlama bilgisi oluşturmak için kullanılan etki alanını belirler. `CookieDomain` Varsayılan olarak ayarlanmamış. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | Tarayıcı tanımlama bilgisine istemci tarafı JavaScript tarafından erişilecek izin verip vermeyeceğini belirler. Varsayılan değer `true`, tanımlama bilgisinin başka bir deyişle, yalnızca HTTP isteklerine geçirilir ve sayfada komut dosyası için kullanılabilir hale değil. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | Oturum kimliği sürdürmek için kullanılan tanımlama bilgisinin adını belirler Varsayılan değer [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | Tanımlama bilgisi oluşturmak için kullanılan yolu belirler. Varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | Tanımlama bilgisinin HTTPS isteklerinde yalnızca iletilip iletilmeyeceğini belirler. Varsayılan değer [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`). |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` İçeriğinin terk önce ne kadar süreyle oturum boşta kalabileceği gösterir. Her oturum erişimini zaman aşımı sıfırlar. Bu, yalnızca oturum olmayan tanımlama bilgisinin içeriğini geçerlidir unutmayın. Varsayılan değer 20 dakikadır. |

Oturum tanımlama bilgisi izlemek ve tek bir tarayıcıdan istekleri belirlemek için kullanır. Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNet.Session`, ve bir yol kullanır `/`.

::: moniker-end

Tanımlama bilgisi oturum Varsayılanları geçersiz kılmak için kullanın `SessionOptions`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

Uygulama kullandığı [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) sunucusunun önbelleğine içeriğini terk önce ne kadar oturum boşta kalabileceği belirlemek için özellik. Bu özellik tanımlama bilgisinin süre sonu bağımsızdır. Geçtiği her isteği [oturum Ara](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) zaman aşımı sıfırlar.

Oturum durumu *kilitleme*. Son istekten iki isteği aynı anda bir oturum içeriğini değiştirmeye çalışırsanız, ilk geçersiz kılar. `Session` olarak uygulanan bir *tutarlı oturum*, yani tüm içeriği birlikte depolanır. İki isteği farklı oturum değerlerini değiştirmek aradığınızda, son isteği tarafından ilk oturum değişikliklerinin geçersiz kılabilir.

### <a name="set-and-get-session-values"></a>Ayarlama ve oturum değerleri alma

Oturum durumu Razor sayfalarından erişilen [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sınıf veya MVC [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller) ile sınıf [içeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Bu özellik bir [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) uygulaması.

::: moniker range=">= aspnetcore-2.0"

`ISession` Uygulamasını tamsayı ve dize değerlerini ayarlama ve alma için birkaç genişletme yöntemleri sağlar. Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (eklemek bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişmek için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor. Her iki paketin içinde yer alan [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`ISession` Uygulamasını tamsayı ve dize değerlerini ayarlama ve alma için birkaç genişletme yöntemleri sağlar. Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (eklemek bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişmek için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor.

::: moniker-end

`ISession` genişletme yöntemleri:

* [Get (ISession, dize)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString (ISession, dize)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [Setınt32 (ISession, dize, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString (ISession, dize, dize)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

Aşağıdaki örnekte oturum değerini alır. `IndexModel.SessionKeyName` anahtarı (`_Name` örnek uygulamasında) bir Razor sayfalarının sayfasında:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

Aşağıdaki örnek, ayarlama ve bir tamsayı ve bir dize alma gösterilmektedir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

Bellek içi önbellek kullanırken bile bir dağıtılmış önbellek senaryosunu sağlamak üzere tüm oturum verilerini seri hale getirilmelidir. En az dize ve sayı serileştiricileri verilmiştir (ve uzantı yöntemleri, bkz: [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). JSON gibi başka bir mekanizma kullanarak kullanıcı tarafından karmaşık türler seri hale getirilmelidir.

Ayarlama ve serileştirilebilir nesneler alma için aşağıdaki genişletme yöntemleri ekleyin:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

Aşağıdaki örnek, ayarlama ve serileştirilebilir bir nesne ile genişletme yöntemleri alma gösterilmektedir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core sunan [Razor sayfalarının sayfa modelin TempData özelliği](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) veya [MVC denetleyicisi TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata). Bu özellik, dosyayı okuma kadar verileri depolar. [Tutmak](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) ve [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) yöntemleri, verileri silme olmadan incelemek için kullanılabilir. Veriler birden çok tek bir istek için gerekli olduğunda TempData yeniden yönlendirme için özellikle yararlıdır. TempData tanımlama bilgilerini veya oturum durumu kullanarak TempData sağlayıcıları tarafından uygulanır.

### <a name="tempdata-providers"></a>TempData sağlayıcıları

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core 2.0 veya sonraki sürümlerde, tanımlama bilgisi tabanlı TempData sağlayıcısı TempData tanımlama bilgilerini depolamak için varsayılan olarak kullanılır.

Tanımlama bilgisi verileri kullanılarak şifrelenir [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), ile kodlanmış [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), ardından öbekli. Tanımlama bilgisinin öbekli çünkü tek tanımlama bilgisi ASP.NET 1.x uygulanmaz çekirdek bulunan sınır boyutu. Şifrelenmiş verileri sıkıştırmak için güvenlik sorunları gibi yol açabileceğinden tanımlama bilgisi verileri sıkıştırılmaz [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlali](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları. Tanımlama bilgisi tabanlı TempData sağlayıcısı hakkında daha fazla bilgi için bkz: [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.0 ve 1.1, oturum durumu TempData sağlayıcısı varsayılan sağlayıcıdır.

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>TempData bir sağlayıcı seçin

TempData sağlayıcısı seçme bazı noktalar gibi içerir:

1. Uygulama zaten oturum durumu kullanıyor mu? Bu durumda, oturum durumu TempData sağlayıcısı kullanarak uygulama (yanı sıra veri boyutu) için ek ücret ödemeden sahiptir.
2. Uygulama TempData yalnızca tutumlu kullanmaz görece küçük miktarda veriyi (en fazla 500 bayt) için? Böylece, tanımlama bilgisi TempData sağlayıcı her istek için küçük bir maliyet eklerse, TempData taşır. Aksi durumda, oturum durumu TempData sağlayıcısı TempData tüketilen kadar gidiş büyük miktarda veri her istekte önlemek yararlı olabilir.
3. Uygulamayı bir sunucu grubunda birden çok sunucu üzerinde çalışıyor mu? Bu nedenle, varsa tanımlama bilgisi TempData sağlayıcısı veri koruması dışında kullanmak için gereken ek yapılandırma (bkz [veri koruması](xref:security/data-protection/index) ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers)).

> [!NOTE]
> Çoğu web istemcileri (örneğin, web tarayıcıları) her tanımlama bilgisi, tanımlama bilgilerinin toplam sayısı veya her ikisi de en büyük boyutu sınırları uygulayın. Tanımlama bilgisi TempData sağlayıcı kullanırken, uygulama bu sınırı aşan olmaz doğrulayın. Verilerin toplam boyutu göz önünde bulundurun. Hesap için şifreleme ve Öbekleme nedeniyle tanımlama bilgisi boyutu artar.

### <a name="configure-the-tempdata-provider"></a>TempData sağlayıcısı yapılandırma

::: moniker range=">= aspnetcore-2.0"

Tanımlama bilgisi tabanlı TempData sağlayıcısı varsayılan olarak etkindir.

Oturum tabanlı TempData sağlayıcı etkinleştirmek için [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) genişletme yöntemi:

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aşağıdaki `Startup` sınıf kodu oturum tabanlı TempData sağlayıcı yapılandırır:

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

Ara yazılım sıralama önemlidir. Önceki örnekte, bir `InvalidOperationException` özel durum oluştu, `UseSession` sonra çağrılan `UseMvc`. Daha fazla bilgi için bkz: [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> .NET Framework hedefleme ve oturum tabanlı TempData sağlayıcısını kullanarak eklerseniz, [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) projeye paket.

## <a name="query-strings"></a>Sorgu dizeleri

Verileri sınırlı miktarda bir istekten başka bir yeni isteğin sorgu dizesi için ekleyerek geçirilebilir. Bu durum e-posta veya sosyal ağlar paylaşılan katıştırılmış durumuyla bağlantılarına izin verir kalıcı bir şekilde yakalamak için yararlıdır. Hiçbir zaman URL sorgu dizeleri ortak olduğundan, sorgu dizeleri için hassas verileri kullanın.

İstenmeyen paylaşımı ek olarak, sorgu dizelerine verileri dahil olmak üzere fırsatı oluşturabilirsiniz [siteler arası istek sahteciliği (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) kullanıcıların kimlik doğrulaması sırasında kötü amaçlı siteleri ziyaret içine kandırarak saldırıları. Saldırganlar uygulamasından kullanıcı verileri çalmaya veya kullanıcı adına kötü amaçlı eylemleri gerçekleştirin. Herhangi bir korunan uygulama veya oturum durumu CSRF saldırılarına karşı korumanız gerekir. Daha fazla bilgi için bkz: [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).

## <a name="hidden-fields"></a>Gizli alanlar

Veri gizli form alanlarını kaydedilir ve bir sonraki istekte geri gönderilen. Bu, çok sayfalı formlarında yaygındır. İstemci olası verileri değiştirme olduğundan, uygulama her zaman gizli alanlarında depolanan verilerin düzeltin gerekir.

## <a name="httpcontextitems"></a>HttpContext.Items

[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) koleksiyonu, tek bir isteği işlerken verileri depolamak için kullanılır. Bir istek işlendikten sonra koleksiyonun içeriğini atılır. `Items` Koleksiyonu genellikle bileşenler veya farklı zamanlarda isteği sırasında zaman çalışır ve parametreleri geçirmek için hiçbir doğrudan şekilde iletişim kurmak için ara yazılım izin vermek için kullanılır.

Aşağıdaki örnekte, [ara yazılımı](xref:fundamentals/middleware/index) ekler `isVerified` için `Items` koleksiyonu.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Değerini başka bir ara yazılım ardışık erişebilirsiniz `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

Yalnızca tek bir uygulama tarafından kullanılan ara yazılımı `string` anahtarları kabul edilebilir. Uygulama örnekleri arasında paylaşılan Ara benzersiz nesne anahtarları anahtar çakışmaları önlemek için kullanmanız gerekir. Aşağıdaki örnek, bir ara yazılım sınıfında tanımlanan benzersiz nesne anahtarı kullanmayı gösterir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

Başka bir kod depolanan değerine erişebilirsiniz `HttpContext.Items` ara yazılım sınıfı tarafından kullanıma sunulan anahtarı kullanarak:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

Bu yaklaşım da koddaki anahtar dizelerinin kullanılmasını ortadan avantajı vardır.

## <a name="cache"></a>Önbellek

Önbelleğe alma, depolamak ve veri almak için etkili bir yoldur. Uygulama, önbelleğe alınmış öğeleri ömrü kontrol edebilirsiniz.

Önbelleğe alınmış verileri belirli bir istek, kullanıcı veya oturum ile ilişkili değil. **Dikkatli olun, diğer kullanıcıların isteklerine tarafından alınan önbellek kullanıcıya özgü verilere değil.**

Daha fazla bilgi için bkz: [yanıtlarını önbelleğe](xref:performance/caching/index) konu.

## <a name="dependency-injection"></a>Bağımlılık ekleme

Kullanım [bağımlılık ekleme](xref:fundamentals/dependency-injection) veri tüm kullanıcılar tarafından kullanılabilmesini sağlamak için:

1. Veri içeren bir hizmet tanımlayın. Örneğin, adlı bir sınıf `MyAppData` tanımlanır:

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. Hizmet sınıfına ekleyin `Startup.ConfigureServices`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. Veri Hizmeti sınıfı kullanılmasına neden:

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

## <a name="common-errors"></a>Sık karşılaşılan hataları

* "Çözümlenemiyor hizmet türü 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' için 'Microsoft.AspNetCore.Session.DistributedSessionStore' etkinleştirmeye çalışırken."

  En az bir yapılandırmak devrederek nedeni genellikle `IDistributedCache` uygulaması. Daha fazla bilgi için bkz: [iş ile dağıtılmış önbellek](xref:performance/caching/distributed) ve [bellek içi önbellek](xref:performance/caching/memory).

* Ara yazılım başarısız oturum (örneğin, yedekleme deposu kullanılamıyorsa) oturumu kalıcı olayda ara yazılım özel durumu günlüğe kaydeder ve isteğin normal şekilde devam eder. Bu, beklenmeyen davranışa yol açar.

  Örneğin, bir kullanıcı oturumu alışveriş sepeti depolar. Kullanıcı bir öğeyi sepete ekler, ancak yürütme başarısız olur. Öğe doğru değilse, sepete eklendi kullanıcıya raporları için uygulama hatası hakkında bilmiyor.

  Hataları denetlemek için önerilen yaklaşım çağırmaktır `await feature.Session.CommitAsync();` uygulama yapıldığında uygulama kodundan oturuma yazma. `CommitAsync` yedekleme deposu kullanılamıyorsa, bir özel durum oluşturur. Varsa `CommitAsync` başarısız olursa, uygulama özel durumu işleyebilir. `LoadAsync` veri deposu kullanılamaz olduğu aynı koşullar altında oluşturur.
