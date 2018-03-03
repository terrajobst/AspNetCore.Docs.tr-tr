---
title: ASP.NET Core oturum ve uygulama durumu
author: rick-anderson
description: "Koruma uygulama ve kullanıcı (oturum) durumunu yaklaşımları istekler arasında."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 6b81cadf39c0db373f82b8de7d8d3901d51ea088
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>ASP.NET Core oturum ve uygulama durumunda giriş

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), ve [Diana LaRose](https://github.com/DianaLaRose)

HTTP durum bilgisi olmayan bir protokoldür. Bir web sunucusu her HTTP isteği bağımsız bir istek olarak değerlendirir ve kullanıcı değerlerini önceki isteklerinden korumaz. Bu makalede, uygulama ve istekler arasında oturum durumunu korumak için farklı yolları açıklanmaktadır. 

## <a name="session-state"></a>oturum durumu

Oturum durumunu kaydetmek ve kullanıcı web uygulamanızı gözatar sırasında kullanıcı verilerini depolamak için kullanabileceğiniz ASP.NET Core bir özelliğidir. Sunucu üzerindeki bir sözlük veya karma tablo oluşan, oturum durumu verileri üzerinden yapılan istekler bir tarayıcıdan devam eder. Oturumunun veri önbelleği tarafından desteklenir.

ASP.NET Core her istek ile sunucuya gönderilen oturum kimliği içeren bir tanımlama bilgisi istemci vererek oturum durumunu korur. Sunucu, oturum verilerini almak için oturum kimliği kullanır. Oturum tanımlama bilgisi tarayıcıya özgü olduğundan, tarayıcılar arasında oturumları paylaşamaz. Yalnızca tarayıcı oturumu sona erdiğinde oturum tanımlama bilgileri silinir. Süresi dolmuş bir oturum için bir tanımlama bilgisi alınmazsa, aynı oturum tanımlama bilgisi kullanan yeni bir oturum oluşturulur. 

Sunucunun son istekten sonra sınırlı bir süre için bir oturum korur. Oturum zaman aşımını ayarlamanız veya 20 dakikalık varsayılan değeri kullanın. Oturum durumu, belirli bir oturuma özeldir, ancak kalıcı olarak kalıcı olmasını gerektirmez kullanıcı verilerini depolamak için idealdir. Veri silindiğinden yedekleme depolama alanından ya da çağrılırken `Session.Clear` veya oturum veri deposunda ne zaman sona erer. Sunucu, tarayıcı kapatıldığında veya oturum tanımlama bilgisi silindiğinde bilmiyor.

> [!WARNING]
> Hassas verileri oturumunda depolamayın. İstemci değil Tarayıcıyı kapatın ve oturum tanımlama bilgisi temizleyin (ve bazı tarayıcılar oturum tanımlama bilgileri windows Canlı). Ayrıca, bir oturum için tek bir kullanıcı kısıtlı olmayabilir; sonraki kullanıcı ile aynı oturumu devam edebilir.

Bellek içi oturum sağlayıcısı, yerel sunucuda oturum verilerini depolar. Bir sunucu grubunda web uygulamanızı çalıştırmak planlıyorsanız, belirli bir sunucuya her oturum bağlamanın Yapışkan oturumları kullanmanız gerekir. Windows Azure Web siteleri platform varsayılan Yapışkan oturumlarına (uygulama isteği yönlendirme veya ARR). Ancak, Yapışkan oturumları ölçeklenebilirliği etkileyen ve web uygulama güncelleştirmeleri zorlaştırabilir. Daha iyi bir seçenek Redis kullanmaktır veya SQL Server dağıtılmış, hangi Yapışkan oturumları gerektirmeyen önbelleğe alır. Daha fazla bilgi için bkz: [dağıtılmış bir önbellekle çalışmaya](xref:performance/caching/distributed). Hizmet sağlayıcıları ayarlama hakkında daha fazla bilgi için bkz: [yapılandırma oturum](#configuring-session) bu makalenin ilerisinde yer.

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET Core MVC sunan [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Bu özellik, dosyayı okuma kadar verileri depolar. `Keep` Ve `Peek` yöntemleri, verileri silme olmadan incelemek için kullanılabilir. `TempData` birden çok tek bir istek için veri gerektiğinde yeniden yönlendirme için özellikle yararlı olacaktır. `TempData` TempData sağlayıcıları tarafından Örneğin, tanımlama bilgilerini veya oturum durumu kullanılarak uygulanır.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>TempData sağlayıcıları

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 ve daha sonra tanımlama bilgisi tabanlı TempData sağlayıcısı TempData tanımlama bilgilerini depolamak için varsayılan olarak kullanılır.

Tanımlama bilgisi verileri ile kodlanmış [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Tanımlama bilgisinin şifrelenir ve öbekli çünkü tek tanımlama bilgisi ASP.NET 1.x uygulanmaz çekirdek bulunan sınır boyutu. Şifrelenmiş verileri sıkıştırmak için güvenlik sorunları gibi yol açabileceğinden tanımlama bilgisi verileri sıkıştırılmış [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlali](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları. Tanımlama bilgisi tabanlı TempData sağlayıcısı hakkında daha fazla bilgi için bkz: [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.0 ve 1.1, oturum durumu TempData sağlayıcısı varsayılandır.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>TempData sağlayıcısı seçme

TempData sağlayıcısı seçme bazı noktalar gibi içerir:

1. Uygulama zaten başka bir amaçla oturum durumu kullanıyor mu? Bu durumda, oturum durumu TempData sağlayıcısı kullanarak uygulama (yanı sıra veri boyutu) için ek ücret ödemeden sahiptir.
2. Uygulama TempData yalnızca tutumlu görece küçük miktarda veri (en fazla 500 bayt) kullanıyor mu? Böylece, tanımlama bilgisi TempData sağlayıcı küçük bir maliyeti her istek ekleyeceğinizi, TempData taşır. Aksi durumda, oturum durumu TempData sağlayıcısı TempData tüketilen kadar gidiş büyük miktarda veri her istekte önlemek yararlı olabilir.
3. Uygulama bir web grubunda (birden çok sunucu) çalışıyor mu? Bu durumda, tanımlama bilgisi TempData sağlayıcısı kullanmak için ek yapılandırma yoktur.

> [!NOTE]
> Çoğu web istemcileri (örneğin, web tarayıcıları) her tanımlama bilgisi, tanımlama bilgilerinin toplam sayısı veya her ikisi de en büyük boyutu sınırları uygulayın. Bu nedenle, tanımlama bilgisi TempData sağlayıcı kullanırken, uygulama bu sınırı aşan olmaz doğrulayın. Şifreleme ek yüklerini için hesap oluşturma ve parçalama verilerin toplam boyutu göz önünde bulundurun.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>TempData sağlayıcısı yapılandırma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Tanımlama bilgisi tabanlı TempData sağlayıcısı varsayılan olarak etkindir. Aşağıdaki `Startup` sınıf kodu oturum tabanlı TempData sağlayıcı yapılandırır:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aşağıdaki `Startup` sınıf kodu oturum tabanlı TempData sağlayıcı yapılandırır:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

Sıralama ara yazılımı bileşenleri için kritik öneme sahiptir. Önceki örnekte, türünde bir özel durum `InvalidOperationException` oluşur, `UseSession` sonra çağrılan `UseMvcWithDefaultRoute`. Bkz: [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering) daha fazla ayrıntı için.

> [!IMPORTANT]
> .NET Framework hedefleme ve oturum tabanlı sağlayıcısını kullanarak eklerseniz, [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet paketini projenize.

## <a name="query-strings"></a>Sorgu dizeleri

Verileri sınırlı miktarda bir istekten başka bir yeni isteğin sorgu dizesi için ekleyerek geçirebilirsiniz. Bu durum e-posta veya sosyal ağlar paylaşılan katıştırılmış durumuyla bağlantılarına izin verir kalıcı bir şekilde yakalamak için yararlıdır. Ancak, bu nedenle, hiçbir zaman sorgu dizeleri için hassas verileri kullanmanız gerekir. Kolayca paylaşılmasını ek olarak, sorgu dizelerine verileri dahil olmak üzere fırsatı oluşturabilirsiniz [siteler arası istek sahteciliği (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) kullanıcıların kimlik doğrulaması sırasında kötü amaçlı siteleri ziyaret içine kandırarak saldırıları. Saldırganlar uygulamanızdan kullanıcı verileri çalmaya veya kullanıcı adına kötü amaçlı eylemleri gerçekleştirin. Korunan application veya session durumu CSRF saldırılarına karşı korumanız gerekir. CSRF saldırıları hakkında daha fazla bilgi için bkz: [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).

## <a name="post-data-and-hidden-fields"></a>Gönderme verisi ve Gizli alanlar

Veri gizli form alanlarını kaydedilir ve bir sonraki istekte geri gönderilen. Bu, çok sayfalı formlarında yaygındır. İstemci olası verileri değiştirme olduğundan, Bununla birlikte, sunucu her zaman bu düzeltin gerekir. 

## <a name="cookies"></a>Tanımlama bilgileri

Tanımlama bilgileri, web uygulamalarında kullanıcıya özgü verileri depolamak için bir yol sağlar. Tanımlama bilgilerini içeren tüm istekleri gönderildiğinden, kendi boyutu en az olarak tutulmalıdır. İdeal olarak, yalnızca bir tanımlayıcı bir tanımlama bilgisinde sunucuda depolanan gerçek verilerle depolanması gerekir. Çoğu tarayıcısı tanımlama bilgilerini 4096 bayt kısıtlayın. Ayrıca, tanımlama bilgileri, yalnızca sınırlı sayıda her etki alanı için kullanılabilir.  

Tanımlama bilgilerini oynama tabi olduğundan, bunlar sunucuda doğrulanması gerekir. Bir istemcide tanımlama bilgisinin dayanıklılık kullanıcı müdahalesi ve sona erme tarihi tabi olsa da, bunlar genellikle istemci üzerindeki veri kalıcılığını en sağlam biçiminde demektir.

Tanımlama bilgileri, genellikle içeriği bilinen bir kullanıcı için burada özelleştirilmiş kişiselleştirme için kullanılır. Kullanıcı yalnızca tanımlanır ve çoğu durumda kimlik doğrulaması değil çünkü, genellikle bir tanımlama bilgisi kullanıcı adı, hesap adı veya benzersiz bir kullanıcı kimliği (örneğin, bir GUID) tanımlama bilgisinde depolayarak güvenliğini sağlayabilirsiniz. Tanımlama bilgisinin sonra bir sitenin kullanıcı kişiselleştirme altyapı erişmek için de kullanabilirsiniz.

## <a name="httpcontextitems"></a>HttpContext.Items

`Items` Koleksiyonudur gerekli olan verileri depolamak için iyi bir konum belirli bir isteği işlerken yalnızca. Koleksiyonun içeriğini sonra her istek atılır. `Items` Koleksiyonu en iyi bileşenler veya ara yazılım için bir yol olarak farklı noktalarda isteği sırasında zaman çalışır ve parametreleri geçirmek için hiçbir doğrudan şekilde iletişim kurmak için kullanılır. Daha fazla bilgi için bkz: [HttpContext.Items ile çalışma](#working-with-httpcontextitems), bu makalenin ilerisinde yer.

## <a name="cache"></a>Önbellek

Önbelleğe alma, depolamak ve veri almak için etkili bir yoldur. Yaşam süresi ve diğer noktalar göre önbelleğe alınmış öğelerin kontrol edebilirsiniz. Daha fazla bilgi edinmek [önbelleğe alma](../performance/caching/index.md).

<a name="session"></a>
## <a name="working-with-session-state"></a>Oturum durumu ile çalışma

### <a name="configuring-session"></a>Oturum yapılandırma

`Microsoft.AspNetCore.Session` Paket oturum durumu yönetmek için ara yazılım sağlar. Oturum Ara etkinleştirmek için `Startup` içermelidir:

- Herhangi bir [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) bellek önbellekleri. `IDistributedCache` Uygulama oturumu için bir yedekleme deposu olarak kullanılır.
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) çağrısı, NuGet paketi "Microsoft.AspNetCore.Session" gerektirir.
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) çağırın.

Aşağıdaki kod, bellek içi oturum Sağlayıcısı'nı ayarlama gösterilmektedir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Oturumdan başvurabilir `HttpContext` yüklenmiş ve yapılandırılmış sonra.

Erişmeye çalıştığınızda `Session` önce `UseSession` çağrılıp çağrılmadığını, özel durum `InvalidOperationException: Session has not been configured for this application or request` atılır.

Yeni bir oluşturmayı denerseniz `Session` (diğer bir deyişle, oturum tanımlama bilgisi oluşturulup oluşturulmadığını) yazmak zaten başlamıştır sonra `Response` akışı, özel durum `InvalidOperationException: The session cannot be established after the response has started` atılır. Özel web sunucusu günlüğünde bulunabilir; tarayıcıda görüntülenmez.

### <a name="loading-session-asynchronously"></a>Oturum zaman uyumsuz olarak yükleme 

Arka plandaki gelen oturum kaydı ASP.NET Core varsayılan oturum sağlayıcısında yükler [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) depolama zaman uyumsuz olarak yalnızca IF [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) yöntemi önce açıkça çağrılır  `TryGetValue`, `Set`, veya `Remove` yöntemleri. Varsa `LoadAsync` ilk olarak, temel olarak adlandırılmaz oturum kayıt yüklendiği zaman uyumlu olarak, hangi olası ölçeklendirmenizi uygulamanın etkileyebilir.

Uygulamanız bu deseni zorlamak için Kaydır [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) ve [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) uygulamaları, bir özel durum sürümleri ile `LoadAsync` yöntemi değil önce adlı `TryGetValue`, `Set`, veya `Remove`. Sarmalanan sürümleri hizmetler kapsayıcısının kaydedin.

### <a name="implementation-details"></a>Uygulama Ayrıntıları

Oturum tanımlama bilgisi izlemek ve tek bir tarayıcıdan istekleri belirlemek için kullanır. Varsayılan olarak, bu tanımlama bilgisi adı ". AspNet.Session"ve onu kullanan bir yolu"/". Tanımlama bilgisi varsayılan bir etki alanı belirtin değil çünkü, istemci tarafı komut dosyası için sayfada kullanılamayacağı (çünkü `CookieHttpOnly` varsayılan olarak `true`).

Oturum Varsayılanları geçersiz kılmak için kullanın `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Sunucunun kullandığı `IdleTimeout` içeriğinin terk önce ne kadar oturum boşta kalabileceği belirlemek için özellik. Bu özellik tanımlama bilgisinin süre sonu bağımsızdır. (Okuma veya yazma) oturum Ara geçtiği her istek zaman aşımını sıfırlar.

Çünkü `Session` olan *kilitleme*, her ikisi de denemesi oturum, son içeriğini değiştirmek iki istek ilk kılıyorsa. `Session` olarak uygulanan bir *tutarlı oturum*, yani tüm içeriği birlikte depolanır. (Farklı anahtarlar) oturum farklı bölümlerini değiştirmek iki isteği hala etkisi birbirine.

### <a name="setting-and-getting-session-values"></a>Ayarlama ve oturum değerleri alma

Oturumu aracılığıyla erişilir `Session` özelliği `HttpContext`. Bu özellik bir [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) uygulaması.

Aşağıdaki örnek, ayarlama ve int ve bir dize alma gösterir:

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Aşağıdaki genişletme yöntemleri eklerseniz, ayarlayın ve oturumuna serileştirilebilir nesneler alın:

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

Aşağıdaki örnek, ayarlama ve alma serileştirilebilir bir nesne gösterilmektedir:

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>HttpContext.Items ile çalışma

`HttpContext` Özet türü bir sözlük koleksiyonu için destek sağlar `IDictionary<object, object>`adlı `Items`. Bu koleksiyon başından kullanılabilir bir *HttpRequest* ve her istek sonunda atılır. Anahtarlı bir girdi için bir değer atayarak ya da belirli bir anahtar değeri isteyen erişebilir.

Aşağıdaki örnekte, [ara yazılımı](xref:fundamentals/middleware/index) ekler `isVerified` için `Items` koleksiyonu.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Ardışık başka bir ara yazılım, erişebilir:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Yalnızca tek bir uygulama tarafından kullanılacak olan ara yazılımı `string` anahtarları kabul edilebilir. Ancak, uygulamalar arasında paylaşılır Ara herhangi olasılığını anahtar çakışmaları önlemek için benzersiz nesne anahtarları kullanmanız gerekir. Birden çok uygulama arasında çalışmalıdır ara yazılımı geliştiriyorsanız, aşağıda gösterildiği gibi ara yazılımı sınıfında tanımlanan benzersiz nesne anahtarı kullanın:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Başka bir kod depolanan değerine erişebilirsiniz `HttpContext.Items` ara yazılım sınıfı tarafından kullanıma sunulan anahtarı kullanarak:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Bu yaklaşım, aynı zamanda "Sihirli dizelerde" kod birden fazla yerde yinelenmesinin ortadan avantajına sahiptir.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>Uygulama durumu verileri

Kullanım [bağımlılık ekleme](xref:fundamentals/dependency-injection) veri tüm kullanıcılar tarafından kullanılabilmesini sağlamak için:

1. Veri içeren bir hizmet tanımlama (örneğin, adlı bir sınıf `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Hizmet sınıfına ekleyin `ConfigureServices` (örneğin `services.AddSingleton<MyAppData>();`).
3. Her denetleyici veri hizmeti sınıfında kullanılmasına neden:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>Oturumla çalışırken sık karşılaşılan hataları

* "Çözümlenemiyor hizmet türü 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' için 'Microsoft.AspNetCore.Session.DistributedSessionStore' etkinleştirmeye çalışırken."

  En az bir yapılandırmak devrederek nedeni genellikle `IDistributedCache` uygulaması. Daha fazla bilgi için bkz: [dağıtılmış bir önbellekle çalışmaya](xref:performance/caching/distributed) ve [bellek önbelleğe alma işleminde](xref:performance/caching/memory).

* Ara yazılım başarısız için oturum oturumu kalıcı olması durumunda (örneğin: veritabanı kullanılabilir durumda değilse), özel durumu günlüğe kaydeder ve onu yuttuğu. İstek daha sonra normal olarak, hangi çok beklenmeyen davranışlara müşteri adayları devam eder.

Tipik bir örnek:

Birisi bir alışveriş sepeti oturumunda depolar. Kullanıcı bir öğe ekler, ancak yürütme başarısız olur. "Öğesi, hangi true değil eklendi" iletisi raporları için uygulama hatası hakkında bilmiyor.

Bu tür hataları denetlemek için önerilen yol çağırmaktır `await feature.Session.CommitAsync();` tamamladığınızda uygulama kodundan oturuma yazma. Sonra şu hata ile şeyleri yapabilirsiniz. Çağrılırken aynı şekilde çalışır `LoadAsync`.

### <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core 1.x: Bu belgede kullanılan kod örneği](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: Bu belgede kullanılan kod örneği](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
