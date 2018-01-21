---
title: ASP.NET Core Middleware
author: rick-anderson
description: "ASP.NET Core ara yazılımı ve istek ardışık düzenini hakkında bilgi edinin."
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: af16046c97964e8e1c16a4f5989fcfa794741c4d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a>ASP.NET Core ara yazılım temelleri

<a name="fundamentals-middleware"></a>

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>Ara yazılım nedir

Ara yazılım istekleri ve yanıtları işlemek için bir uygulama ardışık düzenine birleştirilmiş bir yazılımdır. Her bileşen:

* İstek ardışık düzende sonraki bileşene geçmek seçer.
* İş önce ve ardışık düzende sonraki bileşene çağrıldıktan sonra gerçekleştirebilirsiniz. 

İstek temsilciler istek ardışık düzenini oluşturmak için kullanılır. İstek temsilcileri her HTTP isteği işler.

Temsilcileri kullanarak yapılandırılmış olan istek [çalıştırmak](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [harita](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), ve [kullanım](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) genişletme yöntemleri. Yeniden kullanılabilir bir sınıfta tanımlanabilir veya ayrı istek temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntemi olarak belirtilen satır içi olabilir. Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılımı*, veya *ara yazılımı bileşenleri*. Her ara yazılım bileşeni istek kanalında, ardışık düzende sonraki bileşene çağırma veya zincir uygunsa kısa devre sorumludur.

[Ara yazılım için geçirme HTTP modülleri](../migration/http-modules.md) daha fazla ara yazılımı örnekleri sağlar ve ASP.NET Core içindeki istek ardışık düzen ve önceki sürümler arasındaki farkı açıklanmaktadır.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma

ASP.NET Core istek ardışık düzen isteği temsilciler (iş parçacığı yürütme aşağıdaki siyah ok) Bu diyagramda gösterildiği gibi birbiri ardından, olarak adlandırılan, bir dizi oluşur:

![İstek işleme düzeni ulaşan, üç middlewares ve uygulama bırakarak yanıt aracılığıyla işleme isteği gösteriliyor. Her ara yazılım, mantığını çalışır ve next() deyimi, bir sonraki ara yazılım isteği kapalı aktarır. Üçüncü ara yazılım istek işledikten sonra bu ek next() deyimleri sonra her sırayla istemcisine yanıt olarak uygulamadan çıkmadan önce işlemek için geri önceki iki middlewares aracılığıyla taraf.](middleware/_static/request-delegate-pipeline.png)

Her temsilci, önce ve sonra İleri temsilci işlemleri yapabilirsiniz. Ayrıca, bir temsilci bir istek istek ardışık düzenini kısa devre adlı bir sonraki temsilci değil geçmesine karar verebilirsiniz. Gereksiz iş önler çünkü kısa devre genellikle iyi bir şeydir. Örneğin, statik dosya ara yazılımlarını statik bir dosya için bir istek dönün ve kalan ardışık düzenini kısa devre oluşturur. Özel durum işleme temsilciler kanalının sonraki aşamalarında oluşan özel durumlarını yakalayabilirsiniz ardışık düzeninde çağrılması gerekir.

En basit olası ASP.NET Core uygulama tüm istekleri işleyen tek istek temsilci ayarlar. Bu durumda, gerçek istek ardışık düzenini içermez. Bunun yerine, tek bir anonim işlevi her HTTP isteğine yanıt olarak adlandırılır.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

İlk [uygulama. Çalıştırma](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) temsilci ardışık sonlandırır.

İle birlikte birden çok istek temsilcileri zincirleme [uygulama. Kullanım](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). `next` Parametresi ardışık düzende sonraki temsilci temsil eder. (Ardışık düzen tarafından kısa devre oluşturur olduğunu unutmayın *değil* çağırma *sonraki* parametresi.) Bu örnekte gösterilmiştir gibi öncesinde ve sonrasında sonraki temsilci genellikle eylemleri gerçekleştirebilirsiniz:

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Çağırmayın `next.Invoke` yanıtı istemciye gönderildikten sonra. Değişikliklerini `HttpResponse` yanıt başlatıldıktan sonra bir özel durum oluşturur. Örneğin, üst bilgileri, durum kodu, vb., ayarlama gibi değişiklikler, bir özel durum oluşturur. Yanıt gövdesi çağrıldıktan sonra Yazma `next`:
> - Bir protokolü ihlali neden olabilir. Örneğin, birden çok belirtilen yazma `content-length`.
> - Gövde biçimi bozulmasına neden olabilir. Örneğin, bir HTML altbilgi CSS dosyaya yazma.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) üstbilgileri gönderilen ve/veya gövdesi yazılmış varsa göstermek için yararlı bir ipucu olur.

## <a name="ordering"></a>Sıralama

Ara yazılım bileşenlerinin içinde eklendiğinden sipariş `Configure` , bunlar çağrılır isteklerinde sırası ve yanıtı için ters sırada yöntemi tanımlar. Bu sıralama, güvenlik, performans ve işlevselliği için önemlidir.

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

Böylece istekleri işlemek ve kalan bileşenleri geçmeden kısa devre oluşturur statik dosya ara yazılımlarını erken ardışık düzen adı verilir. Statik dosya ara yazılımlarını sağlar **hiçbir** yetkilendirme denetimleri. Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir. Bkz: [statik dosyaları ile çalışma](xref:fundamentals/static-files) statik dosyaları güvenli bir yaklaşım için.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


İstek statik dosya ara yazılımı tarafından işlenmemiş varsa, bu kimlik Ara geçirildiğinde (`app.UseAuthentication`), kimlik doğrulaması gerçekleştirir. Kimliği doğrulanmamış istekler kısa devre oluşturur değil. İstek kimliğini doğrular rağmen yalnızca bir özel Razor sayfasını veya denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

İstek statik dosya ara yazılımı tarafından işlenmemiş varsa, bu kimlik Ara geçirildiğinde (`app.UseIdentity`), kimlik doğrulaması gerçekleştirir. Kimliği doğrulanmamış istekler kısa devre oluşturur değil. İstek kimliğini doğrular rağmen yalnızca belirli denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.

-----------

Aşağıdaki örnek, burada statik dosyalar için istek yanıt sıkıştırma Ara önce statik dosya ara yazılımı tarafından işlenen sıralama bir ara yazılımı gösterir. Statik dosyalar bu ara yazılım sıralama ile sıkıştırılmaz. MVC yanıtlarının [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sıkıştırılabilir.

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

HTTP kullanarak ardışık düzen yapılandırma `Use`, `Run`, ve `Map`. `Use` Yöntemi kısa devre oluşturur ardışık düzen (diğer bir deyişle, arama, bir `next` isteği temsilci). `Run`bir kural ve bazı ara yazılımı bileşenleri getirebilir `Run[Middleware]` ardışık düzen sonunda çalışacak yöntemleri.

`Map*`Uzantılar, ardışık düzen dallanma için bir kural kullanılır. [Harita](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) istek ardışık düzenini belirtilen istek yolu eşleşmeleri üzerinde göre dallandırır. İstek yolu belirtilen yolun ile başlarsa, şube yürütülür.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:

| İstek | Yanıt |
| --- | --- |
| localhost:1234 | Merhaba harita olmayan temsilci gelen.  |
| localhost:1234/map1 | Harita Test 1 |
| localhost:1234/map2 | Harita Test 2 |
| localhost:1234/map3 | Merhaba harita olmayan temsilci gelen.  |

Zaman `Map` olan kullanıldığında, eşleşen yolu segment(s) çıkarılır `HttpRequest.Path` ve için eklenen `HttpRequest.PathBase` her istek için.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) istek ardışık düzenini belirtilen koşulun sonucuna göre dallandırır. Herhangi bir koşul türü `Func<HttpContext, bool>` istekleri dalı ardışık eşlemek için kullanılır. Aşağıdaki örnekte, bir koşul bir sorgu dizesi değişkeni varolup olmadığını algılamak için kullanılan `branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:

| İstek | Yanıt |
| --- | --- |
| localhost:1234 | Merhaba harita olmayan temsilci gelen.  |
| localhost:1234/?branch=master | Kullanılan şube Yöneticisi =|

`Map`iç içe, örneğin destekler:

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

`Map`Ayrıca birden çok parçalı bir kerede örneğin eşleştirebilirsiniz:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Yerleşik Ara

ASP.NET Core aşağıdaki ara yazılımı bileşenleri ile birlikte gelir:

| Ara yazılım | Açıklama |
| ----- | ------- |
| [Kimlik Doğrulaması](xref:security/authentication/identity) | Kimlik doğrulama desteği sağlar. |
| [CORS](xref:security/cors) | Çıkış noktaları arası kaynak paylaşımını yapılandırır. |
| [Yanıtları Önbelleğe Alma](xref:performance/caching/middleware) | Yanıt önbelleğe alma işlemi için destek sağlar. |
| [Yanıt sıkıştırma](xref:performance/response-compression) | Yanıtları sıkıştırma için destek sağlar. |
| [Yönlendirme](xref:fundamentals/routing) | Tanımlar ve istek yolları kısıtlar. |
| [Oturum](xref:fundamentals/app-state) | Kullanıcı oturumlarını yönetmek için destek sağlar. |
| [Statik dosyalar](xref:fundamentals/static-files) | Statik dosya ve Dizin tarama hizmet vermek için destek sağlar. |
| [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting) | URL yeniden yazma işlemi ve istekleri yönlendirme için destek sağlar. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Yazma Ara

Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile gösteriliyor. Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

Not: Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır. Bkz: [ Genelleştirme ve Yerelleştirme](xref:fundamentals/localization) ASP.NET Core'nın yerleşik yerelleştirme desteği.

Kültürün, örneğin geçirerek ara yazılım sınayabilirsiniz `http://localhost:7997/?culture=no`.

Aşağıdaki kod bir sınıfa ara yazılım temsilci taşır:

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

Ara yazılım aracılığıyla aşağıdaki uzantısı yöntemi gösterir [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

Aşağıdaki kod Ara çağırır `Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Ara yazılım izlemelidir [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/) bağımlılıklarını kendi oluşturucusuna gösterme tarafından. Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*. Bkz: *istek başına bağımlılıkları* üstündeyse ara yazılım istek içinde Hizmetleri paylaşmasına gerekir.

Ara yazılımı bileşenleri bağımlılıklarını bağımlılık ekleme Oluşturucu parametreleri üzerinden gelen çözebilirsiniz. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)Ayrıca ek parametreler doğrudan kabul edebilir.

### <a name="per-request-dependencies"></a>İstek başına bağımlılıkları

Ara yazılım değil istek başına, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım Oluşturucu tarafından kullanılan yaşam süresi olmayan paylaşılan hizmetler diğer bağımlılık tarafından eklenen türleriyle her isteği sırasında. Gereken paylaşıyorsanız bir *kapsamlı* , Ara ve diğer türleri arasında hizmet, bu hizmetlere ekleme `Invoke` yöntemin imzası. `Invoke` Yöntemi tarafından bağımlılık ekleme doldurulur ek parametreleri kabul edebilir. Örneğin:

```c#
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

## <a name="resources"></a>Kaynaklar

* [Bu belge kullanılan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [Ara yazılım için geçirme HTTP modülleri](../migration/http-modules.md)
* [Uygulama Başlatma](startup.md)
* [İstek Özellikleri](request-features.md)
