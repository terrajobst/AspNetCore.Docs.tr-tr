---
title: "Geçirme HTTP işleyicileri ve ASP.NET Core ara yazılım modülleri"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: 8aac6c649b22dc8f6cfc916aa78d56efad7821a0
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>Geçirme HTTP işleyicileri ve ASP.NET Core ara yazılım modülleri 

Tarafından [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

Bu makalede mevcut ASP.NET geçirmek nasıl gösterilmektedir [HTTP modülleri ve system.webserver işleyicilerini](https://docs.microsoft.com/iis/configuration/system.webserver/) ASP.NET Core için [Ara](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Modüller ve tekrar ziyaret işleyicileri

ASP.NET Core ara yazılımı için devam etmeden önce şimdi ilk HTTP modülleri ve işleyicileri nasıl çalıştığını olduðunu:

![Modül işleyicisi](http-modules/_static/moduleshandlers.png)

**İşleyicileri şunlardır:**

   * Sınıfları uygulayan [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * Gibi belirli dosya adı veya uzantısı ile isteklerini işlemek için kullanılan *.report*

   * [Yapılandırılmış](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) içinde *Web.config*

**Modülleri şunlardır:**

   * Sınıfları uygulayan [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * Her istek için çağrılır

   * Kısa devre oluşturur (bir isteğin diğer işlemleri durdurmak) için

   * HTTP yanıtı eklemek veya kendi oluşturmak için

   * [Yapılandırılmış](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) içinde *Web.config*

**Modülleri gelen istekleri işleme sırası tarafından belirlenir:**

   1. [Uygulama yaşam döngüsü](https://msdn.microsoft.com/library/ms227673.aspx), ASP.NET tarafından tetiklenen serisi olayları olduğu: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), vb. Her modül işleyicisi için bir veya daha fazla olay oluşturabilirsiniz.

   2. Aynı olay, bunlar yapılandırılmış sipariş için *Web.config*.

Modülleri yanı sıra, yaşam döngüsü olayları için işleyiciler ekleyebilirsiniz, *Global.asax.cs* dosya. Bu işleyiciler, yapılandırılmış modüllerdeki işleyicileri sonra çalıştırın.

## <a name="from-handlers-and-modules-to-middleware"></a>İşleyicileri ve ara yazılım modülleri

**Ara yazılım HTTP modülleri ve işleyicileri basit şunlardır:**

   * Modüller, işleyiciler *Global.asax.cs*, *Web.config* (dışında IIS yapılandırmasının) ve uygulama yaşam döngüsü kayboldu

   * Modüller ve işleyicileri rollerini ara yazılım tarafından ele alınmış

   * Ara yazılım, kod kullanarak yapılandırılır yerine içinde *Web.config*

   * [Ardışık Düzen dallanma](xref:fundamentals/middleware/index#middleware-run-map-use) yalnızca URL'yi de istek üstbilgileri, sorgu dizeleri, vb. göre belirli ara yazılımı için istekleri gönderdiğiniz sağlar.

**Ara yazılım modülleri için çok benzer:**

   * Her istek için ilkesine çağrılır

   * Bir istek tarafından kısa devre oluşturur yapabileceksiniz [isteği için bir sonraki ara yazılım geçirme değil](#http-modules-shortcircuiting-middleware)

   * Kendi HTTP yanıtı oluşturmak için

**Ara yazılım ve modülleri farklı bir sırada işlenir:**

   * Ara yazılım sırasını içinde eklenir istek ardışık düzenine modülleri sırasını çoğunlukla tabanlı çalıştırırken sipariş temel [uygulama yaşam döngüsü](https://msdn.microsoft.com/library/ms227673.aspx) olayları

   * Modülleri sırasını istekleri ve yanıtları için aynı olsa da Ara yazılım yanıtlar için istekleri, ters sırasıdır

   * Bkz: [IApplicationBuilder ile Ara yazılım ardışık düzenini oluşturma](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)

![Ara yazılım](http-modules/_static/middleware.png)

Yukarıdaki resimde kimlik doğrulaması ara yazılım istek nasıl short-circuited unutmayın.

## <a name="migrating-module-code-to-middleware"></a>Ara yazılım Geçirme Modülü kodu

Var olan bir HTTP Modül aşağıdakine benzer görünecektir:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Gösterildiği gibi [ara yazılımı](xref:fundamentals/middleware/index) sayfasında, bir ASP.NET Core Ara yazılımıdır gösteren bir sınıf bir `Invoke` yöntemi alma bir `HttpContext` ve döndüren bir `Task`. Yeni Ara yazılımınızı şuna benzeyecektir:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Önceki ara yazılım şablon bölümünden alındığı [ara yazılımı yazma](xref:fundamentals/middleware/index#middleware-writing-middleware).

*MyMiddlewareExtensions* yardımcı sınıfı, Ara yapılandırmak kolaylaştırır, `Startup` sınıfı. `UseMyMiddleware` Yöntemi ara yazılım sınıfınız istek ardışık düzenine ekler. Ara yazılım tarafından gerekli hizmetleri Ara oluşturucuda ucunuz.

<a name="http-modules-shortcircuiting-middleware"></a>

Örneğin kullanıcının yetkili olmayan bir istek modülünüzün sonlandırabilir:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Bir ara yazılım değil çağırarak bu işleme `Invoke` ardışık düzende sonraki ara yazılım üzerinde. Ardışık düzeni üzerinden geri yolu yanıt yaptığında, önceki middlewares hala çağrılacak olduğundan bu tam istek sonlandırmak değil olduğunu göz önünde bulundurun.

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Yeni, Ara, modülün işlevselliği geçirdiğinizde, kodunuzu çünkü derleyin değil bulabilirsiniz `HttpContext` sınıfı ASP.NET Core önemli ölçüde değişti. [Daha sonra](#migrating-to-the-new-httpcontext), yeni ASP.NET Core HttpContext geçirmek nasıl görürsünüz.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>İstek ardışık düzenini geçirme modülü ekleme

HTTP modülleri, istek ardışık düzen kullanarak genellikle eklenir *Web.config*:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Bu dönüştürme [, yeni Ara ekleme](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) için istek ardışık düzeninde, `Startup` sınıfı:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Bir modül olarak işlenen olay Ekle burada yeni Ara yazılımınızı ardışık düzeninde tam nokta bağlıdır (`BeginRequest`, `EndRequest`, vs.) ve kendi sıra modülleri, listesinde *Web.config*.

Daha önce belirtildiği gibi ASP.NET Core hiçbir uygulama yaşam döngüsü yoktur ve modülleri tarafından kullanılan sırayı yanıtları ara yazılım tarafından işlenir sipariş farklıdır. Bu sipariş Kararınızı daha zor hale getirebilir.

Sıralama bir sorun olursa, bağımsız olarak sıralanabilir birden fazla ara yazılım bileşenlerine modülünüzün bölebilir.

## <a name="migrating-handler-code-to-middleware"></a>Ara yazılım geçirme işleyici kodu

Bir HTTP işleyicisini şuna benzer:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

ASP.NET Core projenizde, bu şuna benzer bir ara yazılımı için çevirir:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Bu ara yazılım modülleri için karşılık gelen ara çok benzer. Yalnızca gerçek fark burada hiçbir çağrısı yok `_next.Invoke(context)`. Olur; böylece işleyici çağırmak için hiçbir sonraki ara yazılım istek ardışık düzen sonunda olduğundan, mantıklıdır.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>İstek ardışık düzenini geçirme işleyici ekleme

Bir HTTP işleyicisini yapılandırma yapılır *Web.config* ve şuna benzer:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Bu istek ardışık düzeninde, yeni işleyici Ara ekleyerek dönüştüremedi, `Startup` sınıfı, ara yazılımı modüllerden dönüştürülen benzer. Bu yaklaşım, tüm istekleri için yeni bir işleyici Ara yazılımınızı gönderebilir sorundur. Ancak, yalnızca, Ara ulaşması isteklerini belirli bir uzantıya sahip ister. HTTP işleyicisi ile sahip olduğunuz aynı işlevselliği verirsiniz.

Bir çözümdür isteklerini belirli bir uzantıya sahip işlem hattı oluşturmak için kullanarak `MapWhen` genişletme yöntemi. Aynı bunu `Configure` diğer ara yazılımdan eklediğiniz yöntemi:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen`Bu parametreleri alır:

1. Geçen lambda `HttpContext` ve döndürür `true` istek şube görünmeliyse. Bu, yalnızca kendi uzantısı, aynı zamanda istek üstbilgileri, sorgu dizesi parametreleri vb. bağlı olarak istekleri dal anlamına gelir.

2. Geçen lambda bir `IApplicationBuilder` ve dal için tüm ara yazılımı ekler. Başka bir deyişle, ek ara yazılım, işleyici Ara önünde dala ekleyebilirsiniz.

Dal tüm isteklerinde çağrılacak önce ara yazılım ardışık düzenine eklenen; dal, bunlar üzerinde hiçbir etkisi sahip olur.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Ara yazılım seçenekleri seçenekleri desenini kullanarak yükleme

Bazı modüller ve işleyicileri depolanmış yapılandırma seçeneğiniz *Web.config*. Ancak, ASP.NET Core yeni bir yapılandırma modeli yerine kullanılan *Web.config*.

Yeni [yapılandırma sistemi](xref:fundamentals/configuration/index) bunu çözmek için bu seçeneği sunar:

* Ara yazılım seçeneklere gösterildiği gibi doğrudan Ekle [sonraki bölümde](#loading-middleware-options-through-direct-injection).

* Kullanım [seçenekleri düzeni](xref:fundamentals/configuration/options):

1.  Örneğin, ara yazılım seçenekleri tutmak için bir sınıf oluşturun:

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  Seçenek değerleri depolama

    Yapılandırma sistemi seçenek değerleri istediğiniz herhangi bir yere depolamanıza olanak sağlar. Ancak, en siteleri kullanım *appsettings.json*, biz bu yaklaşımı benimsemeye:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection* bir bölüm adı aşağıdadır. Seçenek sınıfı adı ile aynı olması gerekmez.

3. Seçenek değerleri seçenekleri sınıfıyla ilişkilendirme

    Seçenekleri türü ilişkilendirmek için ASP.NET Core'nın bağımlılık ekleme framework seçenekleri desen kullanır (gibi `MyMiddlewareOptions`) ile bir `MyMiddlewareOptions` , gerçek seçenekleri sahip bir nesne.

    Güncelleştirme, `Startup` sınıfı:

    1.  Kullanıyorsanız, *appsettings.json*, yapılandırma Oluşturucusu'nda eklemek `Startup` Oluşturucusu:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  Seçenekler hizmetini yapılandırın:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  Seçeneklerinizi seçenekleri sınıfınız ile ilişkilendirin:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  Ara yazılım Oluşturucu seçeneklere yerleştirir. Bu, bir denetleyici seçeneklere injecting için benzer.

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  [UseMiddleware](#http-modules-usemiddleware) , ara yazılımı ekler genişletme yöntemi `IApplicationBuilder` mvc'deki bağımlılık ekleme.

  Bu sınırlı değildir `IOptions` nesneleri. Ara yazılımınızı gerektiren herhangi bir nesne bu şekilde yerleştirilebilir.

## <a name="loading-middleware-options-through-direct-injection"></a>Ara yazılım seçenekleri doğrudan ekleme işlemi aracılığıyla yükleniyor

Seçenekler düzeni seçenekleri değerler ve bunların tüketicileri arasında Kuplaj gevşek oluşturur avantajına sahiptir. Bir seçenek sınıfı gerçek seçenekleri değerlerle ilişkilendirdiğiniz sonra başka bir sınıf seçeneklerine bağımlılık ekleme çerçevesi aracılığıyla erişim elde edebilirsiniz. Geçici bir çözüm seçenekleri değerleri geçirmek için gerek yoktur.

Farklı seçeneklerle aynı ara yazılım, iki kez kullanmak istiyorsanız, bunu ancak böler. Farklı roller sağlayan farklı dallarda kullanılan örneğin bir yetkilendirme ara yazılımı. İki farklı seçenekler nesneleri bir seçenek sınıfı ile ilişkilendiremezsiniz.

Çözüm seçenekleri nesneleri gerçek seçenekleri değerleri elde etmektir, `Startup` sınıfı ve bu, Ara her örneği için doğrudan geçirin.

1.  İkinci anahtar eklemek *appsettings.json*

    İkinci bir set seçenekleri eklemek için *appsettings.json* dosya, yeni bir anahtar benzersiz şekilde tanımlamak için kullanın:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  Seçenek değerleri almak ve bunları ara geçirin. `Use...` , Ara yazılım ardışık düzene ekler) genişletme yöntemi (yerdir seçeneği değerleri geçirmek için bir mantıksal: 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  Seçenekler parametresi yapılacak Ara yazılımlarını etkinleştir. Bir aşırı yüklemesini sağlayan `Use...` genişletme yöntemi (Seçenekler parametresi alır ve buna ileten `UseMiddleware`). Zaman `UseMiddleware` çağrılır Ara nesne başlattığında parametrelerle, parametreleri, ara yazılım Oluşturucu geçirir.

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    Bu seçenekler nesnesinde nasıl sarmalar Not bir `OptionsWrapper` nesnesi. Bu uygulayan `IOptions`, ara yazılım Oluşturucu tarafından beklendiği gibi.

## <a name="migrating-to-the-new-httpcontext"></a>Yeni HttpContext geçirme

Daha önce gördüğünüzle, `Invoke` , Ara yönteminde türünde bir parametre alan `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext`ASP.NET Core önemli ölçüde değişti. Bu bölümde, en sık kullanılan özelliklerini çevirmek gösterilmiştir [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) yeni `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Benzersiz istek kimliği (System.Web.HttpContext karşılık gelen)**

Her istek için benzersiz bir kimlik verir. Günlüklerinize dahil etmek çok kullanışlıdır.

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** ve **HttpContext.Request.RawUrl** için çevir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> İçerik alt türü yalnızca ise form değerleri Okuma *x-www-form-urlencoded* veya *form verileri*.

**HttpContext.Request.InputStream** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Bu kod yalnızca bir ardışık düzen sonunda bir işleyici türü Ara kullanın.
>
>İstek başına yalnızca bir kez yukarıda gösterildiği gibi ham gövde okuyabilir. İlk okuma boş bir gövdeyi okuyacaksa sonra gövdesi okumaya çalışırken bir ara yazılım.
>
>Bu, bir arabelleğinden yapıldığından form daha önce gösterildiği gibi okuma için geçerli değil.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** ve **HttpContext.Response.StatusDescription** için çevir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** ve **HttpContext.Response.ContentType** için çevir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** üzerinde kendi ayrıca için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** için çevirir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Bir dosyayı yedeklemeniz hizmet veren ele alınmıştır [burada](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Yanıt Üstbilgileri gönderme gerçeğiyle karmaşık her şeyi yanıt gövdesi yazıldıktan sonra ayarlarsanız, bunlar değil gönderilir.

Sağ yanıt başlatır yazmadan önce çağrılacak bir geri çağırma yöntemini ayarlamak için kullanılan çözümüdür. Bu en iyi başlangıcında gerçekleştirilir `Invoke` Ara yazılımınızı yöntemi. Yanıt Üstbilgileri ayarlar bu geri çağırma yöntemidir.

Aşağıdaki kod adlı bir geri çağırma yöntemi ayarlar `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` Geri çağırma yöntemi şuna benzeyebilir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Tanımlama bilgilerini seyahat tarayıcıya bir *Set-Cookie* yanıtı üstbilgisi. Sonuç olarak, tanımlama bilgileri gönderme kullanılanlarla aynı geri çağırma yanıt üstbilgileri göndermek için gerektirir:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies` Geri çağırma yöntemi uygulamamız şu şekilde görünecektir:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP işleyicileri ve HTTP modülleri genel bakış](/iis/configuration/system.webserver/)
* [Yapılandırma](xref:fundamentals/configuration/index)
* [Uygulama Başlatma](xref:fundamentals/startup)
* [Ara Yazılım](xref:fundamentals/middleware/index)
