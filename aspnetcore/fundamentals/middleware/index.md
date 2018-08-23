---
title: ASP.NET Core ara yazılımı
author: rick-anderson
description: ASP.NET Core ara yazılım ve istek ardışık düzenini hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 9ba77561ab4f6a8668c480d6e81f2ce7e0193c73
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870953"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core ara yazılımı

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

Ara yazılım isteklerini ve yanıtlarını işlemek için bir uygulama ardışık birleştirilmiş bir yazılımdır. Her bileşen için:

* İstek ardışık düzende sonraki bileşene geçmek bu seçeneği seçer.
* İş, önce ve ardışık düzende sonraki bileşene çağrıldıktan sonra gerçekleştirebilirsiniz.

İstek Temsilciler, istek ardışık düzenini oluşturmak için kullanılır. İstek temsilcileri her HTTP isteği işler.

Temsilcileri kullanarak yapılandırılmış olan istek <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, ve <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> genişletme yöntemleri. Tek tek istekler temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntem belirtilen satır içi olabilir veya yeniden kullanılabilir bir sınıf içinde tanımlanabilir. Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılım*ayrıca adlı *ara yazılımı bileşenleri*. Her ara yazılım bileşeni istek ardışık düzende, ardışık düzende sonraki bileşene çağırma veya işlem hattı kısa devre sorumludur.

<xref:migration/http-modules> İstek hatlarında ASP.NET Core ve ASP.NET arasındaki farkı açıklar 4.x ve daha fazla ara yazılım örnekleri sağlar.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma

İstek Temsilciler, birbiri ardına adlı bir dizi ASP.NET Core istek ardışık düzenini oluşur. Kavram Aşağıdaki diyagramda gösterilmiştir. Yürütme iş parçacığını siyah okları izler.

![İstek işleme düzeni ulaşan, işlem üç middlewares ve uygulamadan ayrılmasını yanıt bir istek gösteriliyor. Her bir ara yazılım, mantığını çalışır ve izin isteği next() deyimindeki sonraki ara yazılımı için uygulamalı. Üçüncü bir ara yazılım isteği işler sonra ters sırada kendi next() deyimleri istemciye yanıt olarak uygulama çıkmadan önce sonra ek işleme için önceki iki middlewares üzerinden geri istek geçirir.](index/_static/request-delegate-pipeline.png)

Her temsilci önce ve sonra İleri temsilci işlemleri gerçekleştirebilir. Bir istek çağrılır sonraki temsilcisine geçirmemesi bir temsilci da karar verebilirsiniz *istek ardışık düzenini kısa devre*. Gereksiz iş önlediği için kısa devre genellikle tercih edilir. Örneğin, statik dosya ara yazılımı statik bir dosya için bir istek dönün ve kalan ardışık düzenini kısa devre oluşturur. Bunlar işlem hattının sonraki aşamasında oluşan özel durumları yakalayabilirsiniz özel durum işleme temsilciler kanal içinde çağrılır.

Tüm istekleri işleyen bir tek istek temsilci basit olası ASP.NET Core uygulaması ayarlar. Bu durumda, bir gerçek istek ardışık düzeni dahil değildir. Bunun yerine, tek bir anonim işlev, her bir HTTP isteğine yanıt olarak adlandırılır.

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

İlk <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> temsilci işlem hattı sonlandırır.

Birden çok istek temsilciler birlikte zincirleme <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. `next` Parametresi ardışık düzende sonraki temsilciyi temsil eder. İşlem hattı tarafından kısa devre oluşturur *değil* çağırma *sonraki* parametresi. Aşağıdaki örnekte de gösterildiği gibi öncesinde ve sonrasında sonraki temsilci, genellikle eylemleri gerçekleştirebilirsiniz:

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> Remove() çağırmayın `next.Invoke` istemciye yanıt gönderildikten sonra. Değişikliklerini <xref:Microsoft.AspNetCore.Http.HttpResponse> yanıt başlatıldıktan sonra bir özel durum. Örneğin, üst bilgileri ve durum kodu ayarlama gibi değişiklikler, bir özel durum. Yanıt gövdesi için çağırdıktan sonra Yazma `next`:
>
> * Protokol ihlali neden olabilir. Örneğin, belirtilen birden fazla yazma `Content-Length`.
> * Gövde biçimi bozuk. Örneğin, bir CSS dosyası için bir HTML altbilgi yazma.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> üstbilgileri gönderildikten veya için gövde yazılmadan belirtmek için kullanışlı bir ipucudur.

## <a name="order"></a>Sırası

Ara yazılım bileşenleri içinde eklenen sırasını `Startup.Configure` yöntemi, istekler ve yanıt için ters sırada ara yazılımı bileşenleri çağrılır sırasını tanımlar. Güvenlik, performans ve işlev için sırasını kritiktir.

Aşağıdaki `Configure` yöntemi aşağıdaki ara yazılım bileşenlerini ekler:

1. Özel durum/hata işleme
2. Statický souborový server
3. Kimlik doğrulaması
4. MVC

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

Yukarıdaki kodda <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ardışık düzenine eklenen ilk ara yazılım bileşeni. Bu nedenle, özel durum işleyicisi Ara sonraki çağrılarında oluşan özel durumları yakalar.

İstekleri işlemek ve kalan bileşenleri olmadan iki statik dosya ara yazılımı erken işlem hattında çağrılır. Statik dosya ara yazılım sağlar **hiçbir** yetkilendirme denetimleri. Tüm dosyaları sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir. Statik dosyaların güvenliğini sağlamak bir yaklaşım için bkz <xref:fundamentals/static-files>.

::: moniker range=">= aspnetcore-2.0"

Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik doğrulaması Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), kimlik doğrulaması gerçekleştirir. Kimlik doğrulaması, kimliği doğrulanmamış istekler kısa devre oluşturur değil. Kimlik doğrulaması ara yazılım kimlik doğrulaması istekleri olsa da, yalnızca belirli bir Razor sayfası veya MVC denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), kimlik doğrulaması gerçekleştirir. Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil. İstek kimlik doğrular olsa da, yalnızca belirli bir denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.

::: moniker-end

Aşağıdaki örnek, statik dosyaların nerede yanıt sıkıştırma ara yazılımı önce statik dosya ara yazılımı tarafından işlenen bir ara yazılım sırasını gösterir. Bu ara yazılım siparişle sıkıştırılmış statik dosyaları değildir. MVC yanıtlarından <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> birleştirilebilir.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a>Harita kullanın ve çalıştırma

HTTP kullanarak işlem hattını yapılandırmak `Use`, `Run`, ve `Map`. `Use` Yöntemi iki işlem hattı (diğer bir deyişle, çağırma değil, bir `next` istek temsilci). `Run` bir kuralı ve bazı ara yazılımı bileşenleri getirebilir `Run[Middleware]` ardışık düzen sonunda çalışan yöntemleri.

<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> Uzantılar, işlem hattı dallanma için bir kural kullanılır. `Map*` dalları istek ardışık düzenini belirtilen istek yolu eşleşmeleri üzerinde temel. İstek yolu belirtilen yol ile başlarsa, dalı çalıştırılır.

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

Aşağıdaki tablo istekleri ve gelen yanıtları gösterir `http://localhost:1234` önceki kod kullanarak.

| İstek             | Yanıt                     |
| ------------------- | ---------------------------- |
| 1234      | Harita olmayan temsilci gelen Merhaba. |
| 1234 / map1 | Test 1 eşleme                   |
| 1234 / map2 | Harita Test 2                   |
| 1234 / map3 | Harita olmayan temsilci gelen Merhaba. |

Zaman `Map` olan kullanıldığında, eşleşen yolu segment(s) çıkarılır `HttpRequest.Path` ve için eklenen `HttpRequest.PathBase` her istek için.

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) dalları istek ardışık düzenini belirli bir koşul sonucuna göre. Herhangi bir koşul türü `Func<HttpContext, bool>` istekleri işlem hattının yeni bir dala eşlemek için kullanılabilir. Aşağıdaki örnekte, bir koşul bir sorgu dizesi değişkeni varolup olmadığını algılamak için kullanılan `branch`:

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

Aşağıdaki tablo istekleri ve gelen yanıtları gösterir `http://localhost:1234` önceki kod kullanarak.

| İstek                       | Yanıt                     |
| ----------------------------- | ---------------------------- |
| 1234                | Harita olmayan temsilci gelen Merhaba. |
| 1234 /? dal ana = | Dal kullanılan ana =         |

`Map` Örneğin, içe destekler:

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

`Map` Ayrıca birden fazla bölüm aynı anda eşleştirebilirsiniz:

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a>Yerleşik ara yazılım

ASP.NET Core aşağıdaki ara yazılımı bileşenleri ile birlikte gelir. *Sipariş* sütun Ara yerleştirme istek ardışık düzenini ve ara yazılım hangi koşullar altında ilgili notlar istek sonlandırmak ve diğer ara yazılımdan, bir isteğin işlenmesini önlemek sağlar.

| Ara yazılım | Açıklama | Sırası |
| ---------- | ----------- | ----- |
| [Kimlik Doğrulaması](xref:security/authentication/identity) | Kimlik doğrulama desteği sağlar. | Önce `HttpContext.User` gereklidir. Terminal OAuth geri çağırmalar için. |
| [CORS](xref:security/cors) | Çıkış noktaları arası kaynak paylaşımını yapılandırır. | CORS kullanan bileşenleri önce. |
| [Tanılama](xref:fundamentals/error-handling) | Tanılama yapılandırır. | Bileşenlerinden önce bu hataları oluşturur. |
| [İletilen üstbilgileri](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Geçerli istek üzerine proxy üstbilgileri iletir. | Önce güncelleştirilmiş alanları kullanma bileşenleri (örnekler: Düzen, konak, istemci IP'si yöntemi). |
| [HTTP yöntemini geçersiz kılma](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | Bu yöntemi geçersiz kılmak gelen bir POST isteği sağlar. | Bileşenlerinden önce güncelleştirilen yöntemi kullanır. |
| [HTTPS yeniden yönlendirmesi](xref:security/enforcing-ssl#require-https) | Tüm HTTP isteklerini (ASP.NET Core 2.1 veya üzeri) HTTPS'ye yönlendiriyor. | Bileşenlerinden önce bu URL'yi kullanır. |
| [HTTP katı aktarım güvenliği (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Bir özel yanıt üst bilgisi (ASP.NET Core 2.1 veya üzeri) ekleyen güvenlik geliştirmesi ara yazılımı. | Yanıtları gönderilmeden önce ve sonra değiştirme isteklerini (örneğin, iletilen üstbilgileri, URL yeniden yazma) bileşenleri. |
| [MVC](xref:mvc/overview) | MVC/Razor sayfaları (ASP.NET Core 2.0 veya sonraki bir sürümü) ile istekleri işler. | İstek bir Terminal varsa bir rotayla eşleşen. |
| [OWIN](xref:fundamentals/owin) | OWIN tabanlı uygulamalar, sunucuları ve ara yazılım ile birlikte çalışma. | Terminal OWIN ara yazılımı tam isteği işler. |
| [Yanıtları Önbelleğe Alma](xref:performance/caching/middleware) | Yanıtları önbelleğe alma işlemi için destek sağlar. | Önbelleğe alma gerektiren bileşenler önce. |
| [Yanıt sıkıştırma](xref:performance/response-compression) | Destek için yanıtları sıkıştırma sağlar. | Sıkıştırma iste bileşenlerinden önce. |
| [İstek yerelleştirme](xref:fundamentals/localization) | Yerelleştirme desteği sağlar. | Yerelleştirme önemli bileşenlerinden önce. |
| [Yönlendirme](xref:fundamentals/routing) | Tanımlar ve istek yolları kısıtlar. | Yollar eşleştirmek için terminal. |
| [Oturum](xref:fundamentals/app-state) | Kullanıcı oturumlarını yönetmek için destek sağlar. | Bileşenlerinden önce oturumu gerektirir. |
| [Statik dosyalar](xref:fundamentals/static-files) | Statik dosya ve Dizin tarama hizmet vermek için destek sağlar. | İstek bir Terminal varsa, bir dosya ile eşleşir. |
| [URL yeniden yazma](xref:fundamentals/url-rewriting) | URL yeniden yazma ve istekleri yönlendirme için destek sağlar. | Bileşenlerinden önce bu URL'yi kullanır. |
| [WebSockets](xref:fundamentals/websockets) | WebSockets Protokolü sağlar. | WebSocket isteklerini kabul etmek için gerekli bileşenleri önce. |

## <a name="write-middleware"></a>Ara yazılım yazma

Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile kullanıma sunulan. Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır. Bkz: ASP.NET Core'nın yerleşik yerelleştirme desteğini <xref:fundamentals/localization>.

Ara yazılım kültür, örneğin geçirerek sınayabilirsiniz `http://localhost:7997/?culture=no`.

Aşağıdaki kod, bir sınıf için ara yazılım temsilci taşır:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

Ara yazılım `Task` yöntemin adı olmalıdır `Invoke`. ASP.NET Core 2.0 veya sonraki sürümlerde, ad ya da olabilir `Invoke` veya `InvokeAsync`.

::: moniker-end

Aşağıdaki uzantı yöntemi ara yazılımı üzerinden kullanıma sunan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

Aşağıdaki kod ara yazılımı gelen çağrıları `Startup.Configure`:

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

Ara yazılım izlemelidir [açık bağımlılıkları İlkesi](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) tarafından kendi oluşturucusuna bağımlılıkları gösterme. Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*. Bkz: [istek başına bağımlılıkları](#per-request-dependencies) istek içinde ara yazılım ile Hizmetleri paylaşan gerekiyorsa bölümü.

Ara yazılım bileşenleri, bunların bağımlılıklarını giderebilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) Oluşturucu parametresi üzerinden. [UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) ek parametreler doğrudan da kabul edebilir.

### <a name="per-request-dependencies"></a>İstek başına bağımlılıkları

Ara yazılım değil istek, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım oluşturucular tarafından kullanılan etkin kalma süresi olmayan paylaşılan hizmetler diğer bağımlılık eklenen türleriyle her isteği sırasında. Paylaşmanız gerekir durumunda bir *kapsamlı* hizmet Ara yazılımınızı ve diğer türleri arasında bu hizmetlere ekleme `Invoke` yöntemin imzası. `Invoke` Yöntemi tarafından DI doldurulur ek parametreleri kabul edebilir:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
