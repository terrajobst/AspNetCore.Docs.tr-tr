---
title: ASP.NET Core Ara
author: rick-anderson
description: ASP.NET Core ara yazılımı ve istek ardışık düzenini hakkında bilgi edinin.
ms.author: riande
ms.date: 01/22/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: d22c7208390ed2de2ca31ead46ecb21bc41671bf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279590"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core Ara

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>Ara yazılım nedir?

Ara yazılım istekleri ve yanıtları işlemek için bir uygulama ardışık düzenine birleştirilmiş bir yazılımdır. Her bileşen:

* İstek ardışık düzende sonraki bileşene geçmek seçer.
* İş önce ve ardışık düzende sonraki bileşene çağrıldıktan sonra gerçekleştirebilirsiniz. 

İstek temsilciler istek ardışık düzenini oluşturmak için kullanılır. İstek temsilcileri her HTTP isteği işler.

Temsilcileri kullanarak yapılandırılmış olan istek [çalıştırmak](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [harita](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), ve [kullanım](/dotnet/api/microsoft.aspnetcore.builder.useextensions) genişletme yöntemleri. Yeniden kullanılabilir bir sınıfta tanımlanabilir veya ayrı istek temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntemi olarak belirtilen satır içi olabilir. Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılımı*, veya *ara yazılımı bileşenleri*. Her ara yazılım bileşeni istek kanalında, ardışık düzende sonraki bileşene çağırma veya zincir uygunsa kısa devre sorumludur.

[HTTP modülleri Ara geçirmek](xref:migration/http-modules) istek ardışık düzenlerinde ASP.NET Core ve ASP.NET arasındaki fark açıklanır 4.x ve daha fazla ara yazılımı örnekleri sağlar.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma

ASP.NET Core istek ardışık düzen isteği temsilciler (iş parçacığı yürütme aşağıdaki siyah ok) Bu diyagramda gösterildiği gibi birbiri ardından, olarak adlandırılan, bir dizi oluşur:

![İstek işleme düzeni ulaşan, üç middlewares ve uygulama bırakarak yanıt aracılığıyla işleme isteği gösteriliyor. Her ara yazılım, mantığını çalışır ve next() deyimi, bir sonraki ara yazılım isteği kapalı aktarır. Üçüncü ara yazılım istek işledikten sonra geri ters sırada uygulama istemcisine yanıt olarak bırakarak önce kendi next() deyimleri sonra ek işleme için önceki iki middlewares aracılığıyla istek geçirir.](index/_static/request-delegate-pipeline.png)

Her temsilci, önce ve sonra İleri temsilci işlemleri yapabilirsiniz. Ayrıca, bir temsilci bir istek istek ardışık düzenini kısa devre adlı bir sonraki temsilci değil geçmesine karar verebilirsiniz. Gereksiz iş önler çünkü kısa devre genellikle iyi bir şeydir. Örneğin, statik dosya ara yazılımlarını statik bir dosya için bir istek dönün ve kalan ardışık düzenini kısa devre oluşturur. Özel durum işleme temsilciler kanalının sonraki aşamalarında oluşan özel durumlarını yakalayabilirsiniz ardışık düzeninde çağrılması gerekir.

En basit olası ASP.NET Core uygulama tüm istekleri işleyen tek istek temsilci ayarlar. Bu durumda, gerçek istek ardışık düzenini içermez. Bunun yerine, tek bir anonim işlevi her HTTP isteğine yanıt olarak adlandırılır.

[!code-csharp[](index/sample/Middleware/Startup.cs)]

İlk [uygulama. Çalıştırma](/dotnet/api/microsoft.aspnetcore.builder.runextensions) temsilci ardışık sonlandırır.

İle birlikte birden çok istek temsilcileri zincirleme [uygulama. Kullanım](/dotnet/api/microsoft.aspnetcore.builder.useextensions). `next` Parametresi ardışık düzende sonraki temsilci temsil eder. (Ardışık düzen tarafından kısa devre oluşturur olduğunu unutmayın *değil* çağırma *sonraki* parametresi.) Bu örnekte gösterilmiştir gibi öncesinde ve sonrasında sonraki temsilci genellikle eylemleri gerçekleştirebilirsiniz:

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Çağrı yok `next.Invoke` yanıtı istemciye gönderildikten sonra. Değişikliklerini `HttpResponse` yanıt başlatıldıktan sonra bir özel durum oluşturur. Örneğin, üst bilgileri, durum kodu, vb., ayarlama gibi değişiklikler, bir özel durum oluşturur. Yanıt gövdesi çağrıldıktan sonra Yazma `next`:
> - Bir protokolü ihlali neden olabilir. Örneğin, birden çok belirtilen yazma `content-length`.
> - Gövde biçimi bozulmasına neden olabilir. Örneğin, bir HTML altbilgi CSS dosyaya yazma.
>
> [HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) üstbilgileri gönderilen ve/veya gövdesi yazılmış varsa göstermek için yararlı bir ipucu olur.

## <a name="ordering"></a>Sıralama

Ara yazılım bileşenlerinin içinde eklendiğinden sipariş `Configure` , bunlar çağrılan isteklerinde sırası ve yanıtı için ters sırada yöntemi tanımlar. Bu sıralama, güvenlik, performans ve işlevselliği için önemlidir.

(Aşağıda gösterilen) yapılandırma yöntemi aşağıdaki ara yazılımı bileşenleri ekler:

1. Özel durum/hata işleme
2. Statik dosya sunucusu
3. Kimlik doğrulaması
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

Yukarıdaki kod `UseExceptionHandler` ardışık düzenine eklenen ilk ara yazılım bileşeni; bu nedenle, daha sonra çağrılarında oluşan özel durumları yakalar.

Böylece istekleri işlemek ve kalan bileşenleri geçmeden kısa devre oluşturur statik dosya ara yazılımlarını erken ardışık düzen adı verilir. Statik dosya ara yazılımlarını sağlar **hiçbir** yetkilendirme denetimleri. Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir. Bkz: [statik dosyalar](xref:fundamentals/static-files) statik dosyaları güvenli bir yaklaşım için.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


İstek statik dosya ara yazılım tarafından işlenen değil, bu kimlik Ara geçirildiğinde (`app.UseAuthentication`), kimlik doğrulaması gerçekleştirir. Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil. İstek kimliğini doğrular rağmen yalnızca bir özel Razor sayfasını veya denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

İstek statik dosya ara yazılım tarafından işlenen değil, bu kimlik Ara geçirildiğinde (`app.UseIdentity`), kimlik doğrulaması gerçekleştirir. Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil. İstek kimliğini doğrular rağmen yalnızca belirli denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.

-----------

Aşağıdaki örnek, burada statik dosyalar için istek yanıt sıkıştırma Ara önce statik dosya ara yazılımı tarafından işlenen sıralama bir ara yazılımı gösterir. Statik dosyalar bu ara yazılım sıralama ile sıkıştırılmaz. MVC yanıtlarının [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sıkıştırılabilir.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Kullanmak için çalıştırmak ve eşleme

HTTP kullanarak ardışık düzen yapılandırma `Use`, `Run`, ve `Map`. `Use` Yöntemi kısa devre oluşturur ardışık düzen (diğer bir deyişle, çağrı değil, bir `next` isteği temsilci). `Run` bir kural ve bazı ara yazılımı bileşenleri getirebilir `Run[Middleware]` ardışık düzen sonunda çalışacak yöntemleri.

`Map*` Uzantılar, ardışık düzen dallanma için bir kural kullanılır. [Harita](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) istek ardışık düzenini belirtilen istek yolu eşleşmeleri üzerinde göre dallandırır. İstek yolu belirtilen yolun ile başlarsa, şube yürütülür.

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:

| İstek | Yanıt |
| --- | --- |
| localhost:1234 | Merhaba harita olmayan temsilci gelen.  |
| localhost:1234 / map1 | Harita Test 1 |
| localhost:1234 / map2 | Harita Test 2 |
| localhost:1234 / map3 | Merhaba harita olmayan temsilci gelen.  |

Zaman `Map` olan kullanıldığında, eşleşen yolu segment(s) çıkarılır `HttpRequest.Path` ve için eklenen `HttpRequest.PathBase` her istek için.

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) istek ardışık düzenini belirtilen koşulun sonucuna göre dallandırır. Herhangi bir koşul türü `Func<HttpContext, bool>` istekleri dalı ardışık eşlemek için kullanılır. Aşağıdaki örnekte, bir koşul bir sorgu dizesi değişkeni varolup olmadığını algılamak için kullanılan `branch`:

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:

| İstek | Yanıt |
| --- | --- |
| localhost:1234 | Merhaba harita olmayan temsilci gelen.  |
| localhost:1234 /? şube Yöneticisi = | Kullanılan şube Yöneticisi =|

`Map` iç içe, örneğin destekler:

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map` Ayrıca birden çok parçalı bir kerede örneğin eşleştirebilirsiniz:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Yerleşik Ara

ASP.NET Core aşağıdaki ara yazılımı bileşenleri yanı sıra ile bunlar eklenmesi gereken sırayı açıklaması gelir:

| Ara yazılım | Açıklama | Sırası |
| ---------- | ----------- | ----- |
| [Kimlik Doğrulaması](xref:security/authentication/identity) | Kimlik doğrulama desteği sağlar. | Önce `HttpContext.User` gereklidir. Terminal OAuth geri aramalar için. |
| [CORS](xref:security/cors) | Çıkış noktaları arası kaynak paylaşımını yapılandırır. | CORS kullanan bileşenleri önce. |
| [Tanılama](xref:fundamentals/error-handling) | Tanılama yapılandırır. | Hatalar oluşturur bileşenlerini önce. |
| [İletilen üstbilgileri](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Geçerli istek üzerine yönlendirilirken üstbilgileri iletir. | Güncelleştirilmiş alanları tüketen bileşenleri önce (örnek: düzeni, ana bilgisayar, istemci IP yöntemi). |
| [HTTP yöntemini geçersiz kılma](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | Yöntemini geçersiz kılmak gelen bir POST isteği sağlar. | Consume güncelleştirilmiş yöntemi bileşenleri önce. |
| [HTTPS yeniden yönlendirmesi](xref:security/enforcing-ssl#require-https) | Tüm HTTP isteklerini yeniden yönlendir HTTPS (ASP.NET Core 2.1 veya sonrası). | URL tüketen bileşenleri önce. |
| [HTTP katı taşıma güvenliği (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Bir özel yanıt üst bilgisi (ASP.NET Core 2.1 veya sonrası) eklediği güvenlik geliştirme ara yazılımı. | Yanıtları gönderilmeden önce ve sonra değiştirme isteklerini (örneğin, iletilen üst bilgiler, URL yeniden yazma işlemi) bileşenleri. |
| [Yanıtları Önbelleğe Alma](xref:performance/caching/middleware) | Yanıt önbelleğe alma işlemi için destek sağlar. | Önbelleğe alma gerektiren bileşenler önce. |
| [Yanıt sıkıştırma](xref:performance/response-compression) | Yanıtları sıkıştırma için destek sağlar. | Sıkıştırma iste bileşenleri önce. |
| [İstek yerelleştirme](xref:fundamentals/localization) | Yerelleştirme desteği sağlar. | Yerelleştirme önce hassas bileşenleri. |
| [Yönlendirme](xref:fundamentals/routing) | Tanımlar ve istek yolları kısıtlar. | Yollar eşleştirmek için terminal. |
| [Oturum](xref:fundamentals/app-state) | Kullanıcı oturumlarını yönetmek için destek sağlar. | Oturum gerektiren bileşenler önce. |
| [Statik dosyalar](xref:fundamentals/static-files) | Statik dosya ve Dizin tarama hizmet vermek için destek sağlar. | Bir isteği dosyaları eşleşirse terminal. |
| [URL yeniden yazma işlemi](xref:fundamentals/url-rewriting) | URL yeniden yazma işlemi ve istekleri yönlendirme için destek sağlar. | URL tüketen bileşenleri önce. |
| [WebSockets](xref:fundamentals/websockets) | WebSockets Protokolü sağlar. | WebSocket isteklerini kabul etmek için gerekli bileşenleri önce. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Yazma Ara

Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile gösteriliyor. Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

Not: Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır. Bkz: [ Genelleştirme ve Yerelleştirme](xref:fundamentals/localization) ASP.NET Core'nın yerleşik yerelleştirme desteği.

Kültürün, örneğin geçirerek ara yazılım sınayabilirsiniz `http://localhost:7997/?culture=no`.

Aşağıdaki kod bir sınıfa ara yazılım temsilci taşır:

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> ASP.NET Core içinde 1.x, ara yazılım `Task` yöntemin adı olmalı `Invoke`. ASP.NET Core 2.0 veya sonraki sürümlerde, adı ya da olabilir `Invoke` veya `InvokeAsync`.

Ara yazılım aracılığıyla aşağıdaki uzantısı yöntemi gösterir [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

Aşağıdaki kod Ara çağırır `Configure`:

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Ara yazılım izlemelidir [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/) bağımlılıklarını kendi oluşturucusuna gösterme tarafından. Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*. Bkz: *istek başına bağımlılıkları* üstündeyse ara yazılım istek içinde Hizmetleri paylaşmasına gerekir.

Ara yazılımı bileşenleri bağımlılıklarını bağımlılık ekleme Oluşturucu parametreleri üzerinden gelen çözebilirsiniz. [`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) Ayrıca ek parametreler doğrudan kabul edebilir.

### <a name="per-request-dependencies"></a>İstek başına bağımlılıkları

Ara yazılım değil istek başına, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım Oluşturucu tarafından kullanılan yaşam süresi olmayan paylaşılan hizmetler diğer bağımlılık tarafından eklenen türleriyle her isteği sırasında. Gereken paylaşıyorsanız bir *kapsamlı* , Ara ve diğer türleri arasında hizmet, bu hizmetlere ekleme `Invoke` yöntemin imzası. `Invoke` Yöntemi tarafından bağımlılık ekleme doldurulur ek parametreleri kabul edebilir. Örneğin:

```csharp
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP modülleri Ara geçirme](xref:migration/http-modules)
* [Uygulama Başlatma](xref:fundamentals/startup)
* [İstek Özellikleri](xref:fundamentals/request-features)
* [Ara yazılımı Fabrika tabanlı etkinleştirme](xref:fundamentals/middleware/extensibility)
* [Bir üçüncü taraf kapsayıcısı ile Ara yazılım etkinleştirme](xref:fundamentals/middleware/extensibility-third-party-container)
