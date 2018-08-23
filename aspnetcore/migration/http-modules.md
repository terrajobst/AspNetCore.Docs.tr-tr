---
title: ASP.NET Core Ara yazılımıyla HTTP işleyicilerini ve modülleri geçirme
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 9dd28b86966912cce87166feb37e65adf3dd6dcb
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902677"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>ASP.NET Core Ara yazılımıyla HTTP işleyicilerini ve modülleri geçirme

Tarafından [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

Bu makale mevcut ASP.NET geçirme [HTTP modüller ve işleyiciler system.webserver gelen](/iis/configuration/system.webserver/) ASP.NET core'a [ara yazılım](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Modüller ve işleyiciler revisited

ASP.NET Core ara yazılımı için devam etmeden önce şimdi ilk HTTP modüller ve işleyiciler nasıl özeti:

![Modül işleyicisi](http-modules/_static/moduleshandlers.png)

**İşleyicileri şunlardır:**

   * Uygulayan sınıflar [IHttpHandler](/dotnet/api/system.web.ihttphandler)

   * Gibi belirli dosya adı veya uzantısı, istekleri işlemek için kullanılan *.report*

   * [Yapılandırılmış](/iis/configuration/system.webserver/handlers/) içinde *Web.config*

**Modüller şunlardır:**

   * Uygulayan sınıflar [IHttpModule](/dotnet/api/system.web.ihttpmodule)

   * Her istek için çağrılır

   * Şunları (bir isteğin diğer işlemleri durdurmak) kısa devre oluşturur

   * HTTP yanıtı ekleyin veya kendi oluşturmak için

   * [Yapılandırılmış](/iis/configuration/system.webserver/modules/) içinde *Web.config*

**Hangi modüllerin gelen istekleri işleme sırası tarafından belirlenir:**

   1. [Uygulama yaşam döngüsü](https://msdn.microsoft.com/library/ms227673.aspx), ASP.NET tarafından tetiklenen bir serisi olayları olduğu: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)vb. Her modülü, bir veya daha fazla olay işleyicisi oluşturabilirsiniz.

   2. Aynı olay, siparişin, bunlar yapılandırılmış için *Web.config*.

Modüller yanı sıra, yaşam döngüsü olayları için işleyiciler ekleyebilirsiniz, *Global.asax.cs* dosya. Bu işleyiciler, yapılandırılmış modülleri işleyiciler sonra çalıştırın.

## <a name="from-handlers-and-modules-to-middleware"></a>İşleyicileri ve modüllerinden Ara yazılıma

**Ara yazılım HTTP modüller ve işleyiciler basit şunlardır:**

   * Modüller, işleyiciler *Global.asax.cs*, *Web.config* (hariç IIS yapılandırması) ve uygulama yaşam döngüsü kayboldu

   * Ara yazılım tarafından devralınırsa hem modüller ve işleyiciler rollerini alınmış

   * Ara yazılım, kod kullanarak yapılandırılmış yerine *Web.config*

   * [İşlem hattı dallanma](xref:fundamentals/middleware/index#use-run-and-map) yalnızca URL de istek üst bilgileri, sorgu dizeleri, vb. üzerinde dayalı belirli ara yazılımı için istekleri gönderdiğiniz sağlar.

**Ara yazılım modüllerini çok benzer:**

   * Her istek için asıl çağrılır

   * Bir istek tarafından iki duruma [isteğin sonraki Ara yazılıma geçmiyor.](#http-modules-shortcircuiting-middleware)

   * Kendi HTTP yanıtı oluşturmak için

**Ara yazılım ve modülleri farklı bir sırada işlenir:**

   * Ara yazılım sırası içinde eklenir istek ardışık düzende modülleri sırasını çoğunlukla tabanlı çalıştırırken sırasını temel [uygulama yaşam döngüsü](https://msdn.microsoft.com/library/ms227673.aspx) olayları

   * Modüller sırasını istekleri ve yanıtları için aynı olsa da Ara yazılım yanıtları için istekleri, tersine sırasıdır

   * Bkz: [IApplicationBuilder ile bir ara yazılım ardışık düzenini oluşturun](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![Ara yazılım](http-modules/_static/middleware.png)

Yukarıdaki görüntüde, kimlik doğrulaması ara yazılım istek nasıl short-circuited unutmayın.

## <a name="migrating-module-code-to-middleware"></a>Ara yazılıma modülü kod geçirme

Var olan bir HTTP modülü, aşağıdakine benzer görünecektir:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Gösterildiği [ara yazılım](xref:fundamentals/middleware/index) sayfasında, bir ASP.NET Core ara yazılımı olduğunu gösteren bir sınıf bir `Invoke` yöntemi alma bir `HttpContext` ve döndüren bir `Task`. Yeni Ara yazılımınızı şöyle görünür:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Yukarıdaki ara yazılım şablonu bölümünden alındığı [yazma ara yazılımı](xref:fundamentals/middleware/index#write-middleware).

*MyMiddlewareExtensions* yardımcı bir sınıf içinde ara yazılımını yapılandırma kolaylaştırır, `Startup` sınıfı. `UseMyMiddleware` Yöntemi, ara yazılım sınıfı istek ardışık düzene ekler. Ara yazılım tarafından gerekli hizmetleri Ara oluşturucuda eklenen.

<a name="http-modules-shortcircuiting-middleware"></a>

Örneğin kullanıcının yetkili olmayan bir istek modülünüzde sonlandırabilir:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Bir ara yazılım değil çağırarak bu işleme `Invoke` ardışık düzende sonraki ara yazılımı üzerinde. Yanıt yolu geri ardışık düzeninden yaptığında, yine de önceki middlewares çağrılacak çünkü bu tam olarak istek sonlandırmaz aklınızda bulundurun.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Modülün işlevi yeni Ara yazılımınızı geçirdiğinizde, kodunuzu için derle değil bulabilirsiniz `HttpContext` sınıf, ASP.NET Core önemli ölçüde değişti. [Daha sonra](#migrating-to-the-new-httpcontext), yeni ASP.NET temel HttpContext geçirmeyi öğreneceksiniz.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>İstek ardışık düzenini uygulamasına geçirme modülü ekleme

HTTP modüllerinden, istek kullanarak işlem hattı genellikle eklenir *Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Bu dönüştürme [yeni Ara yazılımınızı ekleme](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) istek işlem hattı için `Startup` sınıfı:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Bir modül olarak işlenen bir olayın noktanın tam yeni Ara yazılımınızı eklediğiniz yerde işlem hattındaki bağlıdır (`BeginRequest`, `EndRequest`, vs.) ve modül listenizde, siparişi *Web.config*.

Daha önce belirtildiği gibi ASP.NET Core hiçbir uygulama yaşam döngüsü yoktur ve modülleri tarafından kullanılan sırası yanıtlarını ara yazılımı tarafından işlenme sırasını farklıdır. Bu daha zor sıralama kullanacağınıza karar vermeniz.

Sıralama bir soruna dönüşmesi durumunda, bağımsız olarak sıralanabilir birden fazla ara yazılım bileşenlerine modülünüzde bölebilirsiniz.

## <a name="migrating-handler-code-to-middleware"></a>Ara yazılıma geçirilmesi işleyici kodu

Bir HTTP işleyicisini şuna benzer:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

ASP.NET Core projenizde, bu şuna benzer bir ara yazılım için çevirir:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Bu ara yazılım modüllerini karşılık gelen bir ara yazılım çok benzer. Yalnızca gerçek fark burada hiçbir yoktur `_next.Invoke(context)`. Olacaktır işleyici çağırmak için hiçbir sonraki ara yazılım istek ardışık düzenini sonunda olduğundan, mantıklıdır.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>İstek ardışık düzenini uygulamasına geçirme işleyici ekleme

Bir HTTP işleyicisini yapılandırma yapılır *Web.config* ve şuna benzer:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Bu istek işlem hattı, yeni bir işleyici Ara yazılımınızı ekleyerek dönüştüremedi, `Startup` sınıfı, ara yazılım modüllerini dönüştürülen benzer. Bu yaklaşım, tüm istekler için yeni bir işleyici Ara yazılımınızı gönderir sorunudur. Ancak, yalnızca ara yazılımınızı ulaşmak için istekleri belirli bir uzantıya sahip istersiniz. Bu, HTTP işleyicisi ile olan aynı işlevleri verirsiniz.

İstekleri belirli bir uzantıya sahip bir işlem hattı oluşturmak için bir çözüm ise kullanarak `MapWhen` genişletme yöntemi. Aynı bunu `Configure` diğer ara yazılımdan eklediğiniz yöntemi:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` Bu parametreleri alır:

1. Alan bir lambda `HttpContext` ve döndürür `true` istek dalını giderseniz. Bu, yalnızca kendi uzantı üzerinde ancak istek üstbilgilerini, sorgu dizesi parametrelerini temel istekleri dallandırabilirsiniz anlamına gelir.

2. Alan bir lambda bir `IApplicationBuilder` ve dal için tüm ara yazılımı ekler. Başka bir deyişle, ek bir ara yazılım dala önünde işleyici Ara yazılımınızı ekleyebilirsiniz.

Tüm istekleri dal çağrılacak önce ara yazılım ardışık düzenine eklenen; Dal üzerinde herhangi bir etkisi olacaktır.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Yükleme seçenekleri desenini kullanarak, ara yazılım seçenekleri

Bazı modüller ve işleyiciler depolanan yapılandırma seçeneğiniz *Web.config*. ASP.NET Core yerine yeni bir yapılandırma modeli ancak kullanılır *Web.config*.

Yeni [yapılandırma sistemi](xref:fundamentals/configuration/index) bunu çözmek için bu seçeneği sunar:

* Ara yazılım seçeneklere gösterildiği gibi doğrudan ekleme [sonraki bölümde](#loading-middleware-options-through-direct-injection).

* Kullanım [seçenekleri deseni](xref:fundamentals/configuration/options):

1. Örneğin, ara yazılım seçenekleri tutmak için bir sınıf oluşturun:

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. Seçenek değerleri Store

   Yapılandırma sistemi seçenek değerleri istediğiniz herhangi bir yerde depolamanıza olanak sağlar. Ancak, en siteleri kullanım *appsettings.json*, biz bu yaklaşımı benimsemeye:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection* bir bölüm adı'aşağıda verilmiştir. Seçenekleri sınıfınızın adıyla aynı olması gerekmez.

3. Seçenek değerleri seçenekleri sınıfıyla ilişkilendirme

    Seçenek türünün ilişkilendirmek için ASP.NET Core'nın bağımlılık ekleme framework seçenekleri deseni kullanır (gibi `MyMiddlewareOptions`) ile bir `MyMiddlewareOptions` gerçek seçenekleri nesnesi.

    Güncelleştirme, `Startup` sınıfı:

   1. Kullanıyorsanız *appsettings.json*, yapılandırma Oluşturucu'da ekleme `Startup` Oluşturucusu:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. Seçenekleri Hizmeti'ni yapılandırın:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. Seçeneklerinizi, seçenek sınıfı ile ilişkilendirin:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. Seçenekler, ara yazılım Oluşturucu içine yerleştirir. Bu, seçenekleri bir denetleyici içine ekleme için benzer.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   [UseMiddleware](#http-modules-usemiddleware) , ara yazılım ekler genişletme yöntemi `IApplicationBuilder` bağımlılık ekleme üstlenir.

   Bu sınırlı değildir `IOptions` nesneleri. Ara yazılımınızı gerektiren herhangi bir nesne, bu şekilde yerleştirilebilir.

## <a name="loading-middleware-options-through-direct-injection"></a>Ara yazılım seçenekleri aracılığıyla doğrudan ekleme yükleniyor

Seçenekleri deseni gevşek seçenekleri değerleri ve tüketicilerini eşlemeyi oluşturan avantajına sahiptir. Bir seçenek sınıfı gerçek seçenekleri değerleri ile ilişkilendirdiğiniz sonra başka bir sınıf seçenekleri bağımlılık ekleme çerçevesi aracılığıyla erişebilir. Geçici bir çözüm seçenekleri değerleri geçirmek için gerek yoktur.

Aynı ara yazılım farklı seçeneklerle iki kez kullanmak istiyorsanız, bunu ancak ayırır. Kullanılan farklı dallara farklı rollere izin verme örneğin bir yetkilendirme ara yazılımı. İki farklı seçenekler nesneleri bir seçenek sınıfı ile ilişkilendiremezsiniz.

Çözüm seçenekleri nesneleri gerçek seçenekleri değerleriyle almaktır, `Startup` sınıfı ve bunları doğrudan Ara yazılımınızı her bir örneğini geçirin.

1. İkinci bir anahtar ekler *appsettings.json*

   İkinci bir set seçenekleri eklemek için *appsettings.json* dosya, yeni bir anahtar benzersiz şekilde tanımlamak için kullanın:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. Seçenekleri değerleri almak ve bunları ara yazılımıyla geçirin. `Use...` (Bu, ara yazılım ardışık düzene ekler) genişletme yöntemi olduğunu seçeneği değerleri geçirmek için mantıksal bir yer: 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. Seçenekler parametresi gerçekleştirilecek Ara yazılımlarını etkinleştir. Bir aşırı yüklemesini sağlamak `Use...` uzantı metodu (Seçenekler parametresi alır ve buna ileten `UseMiddleware`). Zaman `UseMiddleware` çağrılır ara yazılım nesnesi örneğini oluşturduğunda parametrelerle, bu parametreleri, ara yazılım Oluşturucu için geçirir.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   Bu seçenekler nesnesinde nasıl sarmalar Not bir `OptionsWrapper` nesne. Bu uygulayan `IOptions`ara yazılım Oluşturucu tarafından beklendiği gibi.

## <a name="migrating-to-the-new-httpcontext"></a>İçin yeni HttpContext geçirme

Daha önce gördüğünüzle, `Invoke` Ara yazılımınızı yönteminde türünde bir parametre alan `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` ASP.NET Core önemli ölçüde değişti. Bu bölümde, en sık kullanılan özelliklerini çevrilecek gösterilmektedir [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) yeni `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Benzersiz istek kimliği (System.Web.HttpContext karşılık gelen)**

Her istek için benzersiz bir kimlik sağlar. Günlükleri dahil etmek çok kullanışlıdır.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** ve **HttpContext.Request.RawUrl** için çevir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Yalnızca içerik alt tür ise form değerlerini okumasını *x-www-form-urlencoded işlemek* veya *form verisinin*.

**HttpContext.Request.InputStream** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Bu kod yalnızca bir işlem hattı, sonunda bir işleyici türü Ara kullanın.
>
>İstek başına yalnızca bir kez yukarıda da gösterildiği gibi ham gövde okuyabilirsiniz. İlk salt okunur, boş bir gövdeyi okuyacaksa sonra gövdesi okumaya çalışırken bir ara yazılım.
>
>Bu, bir arabellek yapıldığı için bir form daha önce gösterildiği gibi okuma için geçerli değildir.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** ve **HttpContext.Response.StatusDescription** için çevir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** ve **HttpContext.Response.ContentType** için çevir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** üzerinde kendi ayrıca için çevirir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** çevrilir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Dosya tanıtıcısıyla ele [burada](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Yanıt üst bilgileri gönderme gerçeğiyle karmaşık yanıt gövdesi için herhangi bir şey yazıldıktan sonra ayarını yaparsanız, bunlar değil gönderilir.

Sağ yanıt başlatır için yazmadan önce çağrılacak bir geri çağırma yöntemi ayarlamak için kullanılan çözümüdür. Bu en iyi başlangıcında gerçekleştirilir `Invoke` Ara yazılımınızı yöntemi. Buna, yanıt üstbilgilerini ayarlar bu geri çağırma yöntemi var.

Aşağıdaki kod olarak adlandırılan bir geri çağırma yöntemi ayarlar `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` Geri çağırma yöntemi şuna benzeyecektir:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Tanımlama bilgilerini seyahat tarayıcıya bir *Set-Cookie* yanıtı üstbilgisi. Sonuç olarak, tanımlama bilgileri gönderme kullanılanlarla aynı geri çağırma yanıt üst bilgilerini göndermek için gerektirir:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies` Geri çağırma yöntemi, aşağıdaki gibi görünür:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP işleyicilerini ve HTTP modüllerini genel bakış](/iis/configuration/system.webserver/)
* [Yapılandırma](xref:fundamentals/configuration/index)
* [Uygulama Başlatma](xref:fundamentals/startup)
* [Ara Yazılım](xref:fundamentals/middleware/index)
