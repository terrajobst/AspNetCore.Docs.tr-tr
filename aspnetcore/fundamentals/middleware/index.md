---
title: ASP.NET Core ara yazılımı
author: rick-anderson
description: ASP.NET Core ara yazılım ve istek ardışık düzenini hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: bac121441d6856ca79affe1a3130e5cbc76debd9
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665395"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core ara yazılımı

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

Ara yazılım isteklerini ve yanıtlarını işlemek için bir uygulama ardışık birleştirilmiş bir yazılımdır. Her bileşen için:

* İstek ardışık düzende sonraki bileşene geçmek bu seçeneği seçer.
* İşlem hattı, iş önce ve sonraki bileşen sonra gerçekleştirebilirsiniz.

İstek Temsilciler, istek ardışık düzenini oluşturmak için kullanılır. İstek temsilcileri her HTTP isteği işler.

Temsilcileri kullanarak yapılandırılmış olan istek <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, ve <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> genişletme yöntemleri. Tek tek istekler temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntem belirtilen satır içi olabilir veya yeniden kullanılabilir bir sınıf içinde tanımlanabilir. Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılım*ayrıca adlı *ara yazılımı bileşenleri*. Her ara yazılım bileşeni istek ardışık düzende, ardışık düzende sonraki bileşene çağırma veya işlem hattı kısa devre sorumludur. Olduğunda bir ara yazılım short-circuits, çağrıldığı bir *terminal ara yazılım* isteği işlemesini daha fazla ara yazılımı önlediği için.

<xref:migration/http-modules> İstek hatlarında ASP.NET Core ve ASP.NET arasındaki farkı açıklar 4.x ve daha fazla ara yazılım örnekleri sağlar.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma

İstek Temsilciler, birbiri ardına adlı bir dizi ASP.NET Core istek ardışık düzenini oluşur. Kavram Aşağıdaki diyagramda gösterilmiştir. Yürütme iş parçacığını siyah okları izler.

![İstek işleme düzeni ulaşan, işlem üç middlewares ve uygulamadan ayrılmasını yanıt bir istek gösteriliyor. Her bir ara yazılım, mantığını çalışır ve izin isteği next() deyimindeki sonraki ara yazılımı için uygulamalı. Üçüncü bir ara yazılım isteği işler sonra ters sırada kendi next() deyimleri istemciye yanıt olarak uygulama çıkmadan önce sonra ek işleme için önceki iki middlewares üzerinden geri istek geçirir.](index/_static/request-delegate-pipeline.png)

Her temsilci önce ve sonra İleri temsilci işlemleri gerçekleştirebilir. Bunlar işlem hattının sonraki aşamasında oluşan özel durumları yakalayabilirsiniz özel durum işleme temsilciler kanal içinde çağrılmalıdır.

Tüm istekleri işleyen bir tek istek temsilci basit olası ASP.NET Core uygulaması ayarlar. Bu durumda, bir gerçek istek ardışık düzeni dahil değildir. Bunun yerine, tek bir anonim işlev, her bir HTTP isteğine yanıt olarak adlandırılır.

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

İlk <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> temsilci işlem hattı sonlandırır.

Birden çok istek temsilciler birlikte zincirleme <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. `next` Parametresi ardışık düzende sonraki temsilciyi temsil eder. İşlem hattı tarafından kısa devre oluşturur *değil* çağırma *sonraki* parametresi. Aşağıdaki örnekte de gösterildiği gibi öncesinde ve sonrasında sonraki temsilci, genellikle eylemleri gerçekleştirebilirsiniz:

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

Bir temsilci sonraki temsilci atamak için bir istek başarısız olduğunda çağrılır *istek ardışık düzenini kısa devre*. Gereksiz iş önlediği için kısa devre genellikle tercih edilir. Örneğin, [statik dosya ara yazılımlarını](xref:fundamentals/static-files) olarak davranıp bir *terminal ara yazılım* statik bir dosya için bir istek işleme ve kalan ardışık düzenini kısa devre. Ara yazılım ardışık düzene daha fazla işleme sonlandıran bir ara yazılım koddan sonra hala işlemeden önce eklenen kendi `next.Invoke` deyimleri. Bununla birlikte, zaten gönderilen yanıt yazma girişiminde bulunulurken hakkında aşağıdaki uyarıyı görürsünüz.

> [!WARNING]
> Remove() çağırmayın `next.Invoke` istemciye yanıt gönderildikten sonra. Değişikliklerini <xref:Microsoft.AspNetCore.Http.HttpResponse> yanıt başlatıldıktan sonra bir özel durum. Örneğin, üst bilgileri ve durum kodu ayarlama gibi değişiklikler, bir özel durum. Yanıt gövdesi için çağırdıktan sonra Yazma `next`:
>
> * Protokol ihlali neden olabilir. Örneğin, belirtilen birden fazla yazma `Content-Length`.
> * Gövde biçimi bozuk. Örneğin, bir CSS dosyası için bir HTML altbilgi yazma.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> üstbilgileri gönderildikten veya için gövde yazılmadan belirtmek için kullanışlı bir ipucudur.

## <a name="order"></a>Sipariş verme

Ara yazılım bileşenleri içinde eklenen sırasını `Startup.Configure` yöntemi, istekler ve yanıt için ters sırada ara yazılımı bileşenleri çağrılır sırasını tanımlar. Güvenlik, performans ve işlev için sırasını kritiktir.

Aşağıdaki `Startup.Configure` yöntemi yaygın uygulama senaryoları için ara yazılım bileşenlerini ekler:

::: moniker range=">= aspnetcore-2.0"

1. Özel durum/hata işleme
1. HTTP taşıma katı güvenlik protokolü
1. HTTPS yeniden yönlendirmesi
1. Statický souborový server
1. Tanımlama bilgisi ilkesi zorlama
1. Kimlik doğrulaması
1. Oturum
1. MVC

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
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. Özel durum/hata işleme
1. Statik dosyalar
1. Kimlik doğrulaması
1. Oturum
1. MVC

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

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

Yukarıdaki örnek kodda, her bir ara yazılım genişletme yöntemi üzerinde kullanıma sunulan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> aracılığıyla <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> ad alanı.

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ardışık düzene ilk ara yazılım bileşeni eklenir. Bu nedenle, özel durum işleyicisi Ara sonraki çağrılarında oluşan özel durumları yakalar.

Böylece isteklerini işlemek ve kalan bileşenleri olmadan iki statik dosya ara yazılımlarını erken işlem hattında çağrılır. Statik dosya ara yazılımlarını sağlar **hiçbir** yetkilendirme denetimleri. Tüm dosyaları sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir. Statik dosyaların güvenliğini sağlamak bir yaklaşım için bkz <xref:fundamentals/static-files>.

::: moniker range=">= aspnetcore-2.0"

Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik doğrulaması Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), kimlik doğrulaması gerçekleştirir. Kimlik doğrulaması, kimliği doğrulanmamış istekler kısa devre oluşturur değil. Kimlik doğrulaması ara yazılım kimlik doğrulaması istekleri olsa da, yalnızca belirli bir Razor sayfası veya MVC denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), kimlik doğrulaması gerçekleştirir. Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil. İstek kimlik doğrular olsa da, yalnızca belirli bir denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.

::: moniker-end

Aşağıdaki örnek, statik dosyaların nerede statik dosya ara yazılımlarını önce yanıt sıkıştırma ara yazılımı tarafından işlenen bir ara yazılım sırasını gösterir. Bu ara yazılım siparişle sıkıştırılmış statik dosyaları değildir. MVC yanıtlarından <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> birleştirilebilir.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
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

ASP.NET Core aşağıdaki ara yazılımı bileşenleri ile birlikte gelir. *Sipariş* sütun sağlar ara yazılım yerleştirme istek işleme ardışık düzeninde ve ara yazılım hangi koşullar altında ilgili notlar isteği işlemeyi sonlandır. Bir ara yazılım ardışık düzen işleme isteği short-circuits ve istek önlediği daha da aşağı akış ara yazılım adlı bir *terminal ara yazılım*. Kısa devre daha fazla bilgi için bkz: [IApplicationBuilder ile bir ara yazılım ardışık düzenini oluşturma](#create-a-middleware-pipeline-with-iapplicationbuilder) bölümü.

| Ara yazılım | Açıklama | Sipariş verme |
| ---------- | ----------- | ----- |
| [Kimlik Doğrulaması](xref:security/authentication/identity) | Kimlik doğrulama desteği sağlar. | Önce `HttpContext.User` gereklidir. Terminal OAuth geri çağırmalar için. |
| [Tanımlama bilgisi ilkesi](xref:security/gdpr) | Onay kullanıcıların kişisel bilgilerini depolamak için ve izler, tanımlama bilgisi alanları için en düşük standartlara zorlayan `secure` ve `SameSite`. | Önce bir ara yazılım, tanımlama bilgileri verir. Örnekler: Kimlik doğrulaması, oturum, MVC (TempData). |
| [CORS](xref:security/cors) | Çıkış noktaları arası kaynak paylaşımını yapılandırır. | CORS kullanan bileşenleri önce. |
| [Özel Durum İşleme](xref:fundamentals/error-handling) | Özel durumları işler. | Bileşenlerinden önce bu hataları oluşturur. |
| [İletilen üstbilgileri](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Geçerli istek üzerine proxy üstbilgileri iletir. | Bileşenlerinden önce güncelleştirilmiş alanları kullanır. Örnekler: düzeni, ana bilgisayar, istemci IP'si yöntemi. |
| [Sistem durumu denetimi](xref:host-and-deploy/health-checks) | ASP.NET Core uygulaması ve veritabanı kullanılabilirlik denetimi gibi bağımlılıklarını durumunu denetler. | İstek bir sistem durumu onay uç noktası eşleşiyorsa terminal. |
| [HTTP yöntemini geçersiz kılma](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | Bu yöntemi geçersiz kılmak gelen bir POST isteği sağlar. | Bileşenlerinden önce güncelleştirilen yöntemi kullanır. |
| [HTTPS yeniden yönlendirmesi](xref:security/enforcing-ssl#require-https) | Tüm HTTP isteklerini (ASP.NET Core 2.1 veya üzeri) HTTPS'ye yönlendiriyor. | Bileşenlerinden önce bu URL'yi kullanır. |
| [HTTP katı aktarım güvenliği (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Bir özel yanıt üst bilgisi (ASP.NET Core 2.1 veya üzeri) ekleyen güvenlik geliştirmesi ara yazılımı. | Yanıtları gönderilmeden önce ve sonra değiştirme isteklerini bileşenleri. Örnekler: Üst bilgiler, iletilen URL yeniden yazma. |
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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
