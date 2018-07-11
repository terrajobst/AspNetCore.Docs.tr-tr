---
title: ASP.NET core'da günlüğe kaydetme
author: ardalis
description: ASP.NET core'da günlüğe kaydetme çerçevesi hakkında bilgi edinin. Yerleşik günlük sağlayıcıları bulmak ve popüler üçüncü taraf sağlayıcılar hakkında daha fazla bilgi edinin.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: dde01129bb7ea29544c4c416dfe9b5522a738d01
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938491"
---
# <a name="logging-in-aspnet-core"></a>ASP.NET core'da günlüğe kaydetme

Tarafından [Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core çeşitli günlük sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler. Bir veya daha fazla hedefe günlükleri gönderme yerleşik sağlayıcılar sağlar ve bir üçüncü taraf günlüğe kaydetme çerçevesi içinde takabilirsiniz. Bu makalede, yerleşik günlüğe kaydetme API'si ve sağlayıcıları kodunuzda nasıl kullanılacağı gösterilmektedir.

::: moniker range=">= aspnetcore-2.0"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

IIS ile barındırırken stdout günlüğe kaydetme hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>. Azure App Service stdout günlüğe kaydetme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

## <a name="how-to-create-logs"></a>Günlükleri oluşturma

Günlükleri oluşturmak için uygulama bir [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) nesnesinden [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Ardından bu Günlükçü nesnede günlük yöntemlerini çağırın:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Bu örnek ile günlükleri oluşturur `TodoController` olarak sınıf *kategori*. Kategorileri açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#log-category).

Günlük zaman uyumsuz kullanma maliyeti karşılıyor olmadığını kadar hızlı olması gerektiğinden, ASP.NET Core Günlükçü yöntemleri zaman uyumsuz sağlamaz. Burada, geçerli olmayan bir durumda kullanıyorsanız oturum şekilde değiştirmeyi göz önünde bulundurun. Data store yavaşsa, günlük iletilerini önce hızlı bir depoya yazmak ve sonra bunları daha sonra yavaş deposuna taşıyın. Örneğin, okuma ve başka bir işlem tarafından yavaş depolama için kalıcı bir ileti kuyruğu oturum açın.

## <a name="how-to-add-providers"></a>Sağlayıcıları ekleme

::: moniker range=">= aspnetcore-2.0"

Oturum açma sağlayıcısı ile oluşturduğunuz iletileri alan bir `ILogger` nesnesini görüntüler ve bunları depolar. Örneğin, konsolu sağlayıcısı iletileri konsolda görüntüler ve Azure App Service sağlayıcısı Azure blob depolama alanında depolayabilirsiniz.

Bir sağlayıcı kullanmak için sağlayıcının çağrı `Add<ProviderName>` uzantı yönteminde *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Varsayılan proje şablonu ile günlük kaydını etkinleştirir [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) yöntemi:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Oturum açma sağlayıcısı ile oluşturduğunuz iletileri alan bir `ILogger` nesnesini görüntüler ve bunları depolar. Örneğin, konsolu sağlayıcısı iletileri konsolda görüntüler ve Azure App Service sağlayıcısı Azure blob depolama alanında depolayabilirsiniz.

Bir sağlayıcı kullanmak için kendi NuGet paketini yükleyin ve bir örneği üzerinde sağlayıcının uzantı metodu çağırma [Iloggerfactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory)aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection) sağlar (dı) `ILoggerFactory` örneği. `AddConsole` Ve `AddDebug` genişletme yöntemleri tanımlanmış [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) ve [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paketleri. Her bir genişletme yöntemi çağıran `ILoggerFactory.AddProvider` sağlayıcının bir örneğini geçirerek yöntemi. 

> [!NOTE]
> [Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) günlük sağlayıcıları ekler `Startup.Configure` yöntemi. Daha önce yürüten koddan günlük çıkış elde etmek istiyorsanız, günlüğe kaydetme hizmeti sağlayıcıları Ekle `Startup` sınıf oluşturucusu.

::: moniker-end

Her hakkında bilgiler bulacaksınız [yerleşik günlük sağlayıcısını](#built-in-logging-providers) ve bağlantılar [üçüncü taraf günlük sağlayıcıları](#third-party-logging-providers) makalenin ilerleyen bölümlerinde.

## <a name="settings-file-configuration"></a>Dosya yapılandırma ayarları

Yukarıdaki örneklerde her [sağlayıcıları ekleme](#how-to-add-providers) bölümü yükler günlük sağlayıcısı yapılandırmasından `Logging` uygulama ayarları dosyaları bölümünü. Aşağıdaki örnek, tipik bir içeriğini gösterir *appsettings. Development.JSON* dosyası:

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

`LogLevel` anahtarları günlük adlarını temsil eder. `Default` Anahtarı açıkça listelenen günlükler için geçerlidir. Değeri temsil [günlük düzeyi](#log-level) verilen günlüğe uygulanır. Günlük anahtarları küme `IncludeScopes` (`Console` örnekte), belirtin [oturum kapsamları](#log-scopes) belirtilen günlük için etkinleştirilir.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

`LogLevel` anahtarları günlük adlarını temsil eder. `Default` Anahtarı açıkça listelenen günlükler için geçerlidir. Değeri temsil [günlük düzeyi](#log-level) verilen günlüğe uygulanır.

::: moniker-end

## <a name="sample-logging-output"></a>Örnek günlük çıktısı

Önceki bölümde gösterilen örnek kod ile komut satırından çalıştırdığınızda, konsol günlüklerine görürsünüz. Konsol çıktının bir örneği aşağıda verilmiştir:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

Bu günlükler giderek oluşturulan `http://localhost:5000/api/todo/0`, her ikisi de yürütülmesini tetikler `ILogger` çağrıları önceki bölümde gösterilen.

Visual Studio'da örnek uygulamayı çalıştırdığınızda hata ayıklama penceresinde göründükleri gibi aynı günlüklerinin bir örnek aşağıdadır:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Tarafından oluşturulan günlükleri `ILogger` çağrıları önceki bölümde gösterilen "TodoApi.Controllers.TodoController" ile başlar. "Microsoft" Kategoriler ile başlayan ASP.NET Core günlüklerdir. ASP.NET Core kendisi ve uygulama kodunuz aynı günlüğe kaydetme API'si ve aynı günlük sağlayıcıları kullanmaktadır.

Bu makalenin geri kalanında, bazı ayrıntılar ve günlüğe kaydetme seçeneklerini açıklar.

## <a name="nuget-packages"></a>NuGet paketleri

`ILogger` Ve `ILoggerFactory` arabirimdir içinde [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ve bunlar için varsayılan uygulamalarıdır içinde [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Günlük kategorisi

A *kategori* oluşturduğunuz her bir günlük dahildir. Kategori oluşturduğunuzda, belirttiğiniz bir `ILogger` nesne. Kategori herhangi bir dize olabilir, ancak günlükler yazıldığı sınıfın tam adını kullanmak için bir kuraldır. Örneğin: "TodoApi.Controllers.TodoController".

Kategori bir dize olarak belirtebilir veya kategori türünden türetilen bir genişletme yöntemi kullanın. Bir dize olarak kategorisini belirtmek için çağrı `CreateLogger` üzerinde bir `ILoggerFactory` , aşağıda gösterildiği gibi örnek.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

Çoğu zaman, kullanımı daha kolay olacak `ILogger<T>`, aşağıdaki örnekte olduğu gibi.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Bu çağırmakla eşdeğerdir `CreateLogger` tam olarak nitelenmiş tür adını `T`.

## <a name="log-level"></a>Günlük düzeyi

Her bir günlük yazma, belirttiğiniz kendi [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). Günlük düzeyini önem derecesi veya önem derecesini gösterir. Örneğin yazabilirsiniz bir `Information` oturum normalde, bir yöntem sona erdiğinde bir `Warning` bir yöntem dönüş kodu 404 hatası döndürdüğünde ve bir günlük `Error` günlüğe beklenmeyen bir özel durum yakalayın.

Aşağıdaki kod örneğinde, yöntemlerin adlarını (örneğin, `LogWarning`) günlük düzeyini belirtin. İlk parametre [oturum öğesini belirten Olay No.](#log-event-id). İkinci parametre bir [ileti şablonunu](#log-message-template) kalan yöntem parametreleri tarafından sağlanan bağımsız değişken değerleri yer tutucuları olan. Yöntem parametreleri, bu makalenin sonraki bölümlerinde daha ayrıntılı açıklanmıştır.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Yöntem adında düzeyi günlük yöntemler [için ILogger genişletme yöntemleri](/dotnet/api/microsoft.extensions.logging.loggerextensions). Arka planda bu yöntemleri çağırmak bir `Log` gereken yöntemini bir `LogLevel` parametresi. Çağırabilirsiniz `Log` biri bu genişletme yöntemleri, ancak söz dizimi yerine doğrudan yöntemi nispeten karmaşık. Daha fazla bilgi için [ILogger arabirimi](/dotnet/api/microsoft.extensions.logging.ilogger) ve [Günlükçü uzantılarını kaynak kodu](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core, aşağıdaki tanımlar [günlük düzeyleri](/dotnet/api/microsoft.extensions.logging.loglevel), burada en çok yüksek derecesi sıralı.

* İzleme = 0

  Yalnızca bir geliştirici için değerli bilgiler için bir sorun hata ayıklama. Bu iletiler, uygulama hassas verileri içerebilir ve bir üretim ortamında bu nedenle etkin olmamalıdır. *Varsayılan olarak devre dışıdır.* Örnek: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Hata ayıklama = 1

  Bilgi için geliştirme ve hata ayıklama sırasında kısa vadeli fayda vardır. Örnek: `Entering method Configure with flag set to true.` genellikle etkinleştirme mıydı `Debug` düzeyi yüksek hacimli günlükleri nedeniyle giderirken sürece üretimde günlüğe kaydeder.

* Bilgi = 2

  Uygulama'nın genel akışı izlemek için. Bu günlükler genellikle uzun vadeli bir değer var. Örnek: `Request received for path /api/todo`

* Uyarı = 3

  Uygulama akışındaki olağan dışı ya da beklenmeyen olaylar için. Bunlar, hata veya uygulamanın durdurmasına neden olmaz, ancak araştırılması gereken, diğer koşulları olabilir. İşlenmiş istisnaları kullanmak için ortak bir yerde `Warning` günlük düzeyi. Örnek: `FileNotFoundException for file quotes.txt.`

* Hata = 4

  Hatalar ve özel durumlar için işlenemez. Bu iletiler, geçerli etkinliği ya da (örneğin, geçerli HTTP isteği) işlemi bir hata, birçok farklı uygulama başarısızlığı gösterir. Örnek günlük iletisi: `Cannot insert record due to duplicate key violation.`

* Kritik = 5

  Hemen ilgilenilmesi gereken hataları. Örnekler: veri kaybı senaryolarına, disk alanı yetersiz.

Günlük düzeyi ne kadar günlük çıktısını bir belirli depolama ortamına yazılır denetlemek veya penceresini görüntülemek için kullanabilirsiniz. Örneğin, üretim ortamında, tüm günlükleri isteyebileceğiniz `Information` düzeyi ve toplu veri deposu ve tüm günlüklerin Git alt `Warning` düzeyi ve daha yüksek bir değer veri deposu için Git. Geliştirme sırasında normalde günlüklerini gönderebilir `Warning` veya konsola daha yüksek önem derecesi. Sorun giderme gerektiğinde ekleyebilirsiniz `Debug` düzeyi. [Günlük filtreleme](#log-filtering) bu makalenin devamındaki bölümüne bir sağlayıcı işleme hangi günlük düzeyleri denetlemek nasıl açıklar.

ASP.NET Core framework Yazar `Debug` düzey framework olayları için günlükleri. Günlük örnekleri, bu makalede daha önce günlüklere hariç tutuldu. `Information` düzeyi, dolayısıyla `Debug` düzeyi günlükleri gösterilir. İşte bir örnek konsol günlüklerinin göstermek üzere yapılandırılmış örnek uygulamayı çalıştırırsanız `Debug` ve konsolu sağlayıcısı için daha yüksek günlükleri.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>Günlük Olay Kimliği

Her bir günlük yazma belirtebileceğiniz bir *öğesini belirten Olay No.*. Örnek uygulamayı yerel olarak tanımlanan bir kullanarak bunu yapar `LoggingEvents` sınıfı:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Olay Kimliği birbiriyle günlüğe kaydedilen olayları kümesini ilişkilendirmek için kullanabileceğiniz bir tamsayı değerdir. Örneğin, bir öğe, alışveriş sepetine eklemek için bir günlüğe olay kimliği 1000 olabilir ve bir satın alma siparişinin tamamlanması için bir günlük olay kimliği 1001 olabilir.

Günlüğünü Çıkışta, olay kimliği bir alanda depolanmış veya olabilir sağlayıcısına bağlı olarak bir SMS mesajı dahil. Olay kimliklerini hata ayıklama sağlayıcısı göstermez ancak Konsolu sağlayıcısı bunları ayraçlar içine sonra kategori gösterir:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Günlük ileti şablonu

Bir günlük iletisine yazma her zaman bir ileti şablonu sağlar. İleti şablonunu bir dize olabilir veya adlandırılmış yer tutucu değerleri hangi bağımsız değişken yerleştirilir içerebilir. Şablon bir biçim dizesi değil ve yok numaralı yer tutucuları adlandırılmalıdır.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Hangi parametrelerin değerlerini sağlamak için kullanılan yer tutucuları, bunların adları sırasını belirler. Aşağıdaki koda sahipseniz:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Elde edilen günlük iletisi şu şekilde görünür:

```
Parameter values: parm1, parm2
```

Günlüğe kaydetme çerçevesi uygulamak günlük sağlayıcıları için mümkün kılmak için bu şekilde biçimlendirme ileti [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Biçimlendirilmiş ileti şablonu yalnızca günlüğe kaydetme sistem değişkenleri geçirildiğinden günlük sağlayıcıları alanlara ileti şablonunu ek olarak parametre değerleri depolayabilir. Azure tablo depolaması için çıktı günlüğünüzün yönlendiren ve Günlükçü yöntem çağrınız şöyle görünür değilse:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Her Azure tablo varlığın `ID` ve `RequestTime` sorgu günlüğü verilerini kolaylaştıran özellikler. Belirli bir içindeki tüm günlükler bulabilirsiniz `RequestTime` kısa mesaj zaman aşımı ayrıştırma gerek kalmadan aralığı.

## <a name="logging-exceptions"></a>Özel durumları

Günlükçü yöntemleri aşağıdaki örnekteki gibi bir özel durum geçirmenize olanak tanıyan aşırı yüklemeleri vardır:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Farklı sağlayıcıları, özel durum bilgilerini farklı yollarla işler. Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktının bir örneği aşağıda verilmiştir.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Günlük Filtresi

::: moniker range=">= aspnetcore-2.0"

En düşük günlük düzeyi veya tüm sağlayıcıları ya da tüm kategorileri özel sağlayıcı ve kategori için belirtebilirsiniz. Bunlar görüntülenen veya depolanan olmayan şekilde en düşük düzeyin herhangi bir günlük bu sağlayıcısına geçirilen değildir. 

Tüm günlükler gizlemek istiyorsanız, belirtebilirsiniz `LogLevel.None` en düşük günlük düzeyi olarak. Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).

**Yapılandırma filtresi kuralları oluşturabilir**

Çağıran kod proje şablonları oluşturma `CreateDefaultBuilder` konsol ve hata ayıklama sağlayıcıları için günlük kaydı ayarlamak için. `CreateDefaultBuilder` Yöntemi ayrıca ayarlar günlüğünü yapılandırmasında aramak için bir `Logging` bölümünde, aşağıdaki gibi kod kullanarak:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Yapılandırma verileri sağlayıcısı ve kategorisi, aşağıdaki örnekte olduğu gibi en az günlük düzeyleri belirtir:

[!code-json[](index/sample2/appsettings.json)]

Bu JSON, bir hata ayıklama sağlayıcısı, konsolu sağlayıcısı için dört ve tüm sağlayıcılar için geçerli bir altı filtre kuralları oluşturur. Bu kurallar daha sonra nasıl yalnızca biri için her bir sağlayıcı seçilir görürsünüz, bir `ILogger` nesnesi oluşturulur.

**Kod içinde filtre kuralları**

Aşağıdaki örnekte gösterildiği gibi kodda filtre kuralları kaydedebilir:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

İkinci `AddFilter` , tür adını kullanarak hata ayıklama sağlayıcıyı belirtir. İlk `AddFilter` bir sağlayıcı türü belirtmeyen çünkü tüm sağlayıcılar için geçerlidir.

**Nasıl filtreleme kuralları uygulanır**

Yapılandırma verilerini ve `AddFilter` Yukarıdaki örneklerde gösterilen kod aşağıdaki tabloda gösterilen kuralları oluşturun. İlk altı yapılandırma örnekten gelmesi ve son iki kod örneğindeki gelir.

| Sayı | Sağlayıcı      | Şununla kategori...          | En düşük günlük düzeyi |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1.      | Hata ayıklama         | Tüm kategoriler                          | Bilgiler       |
| 2      | Konsol       | Microsoft.AspNetCore.Mvc.Razor.Internal | Uyarı           |
| 3      | Konsol       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Hata ayıklama             |
| 4      | Konsol       | Microsoft.AspNetCore.Mvc.Razor          | Hata             |
| 5      | Konsol       | Tüm kategoriler                          | Bilgiler       |
| 6      | Tüm sağlayıcılar | Tüm kategoriler                          | Hata ayıklama             |
| 7      | Tüm sağlayıcılar | Sistem                                  | Hata ayıklama             |
| 8      | Hata ayıklama         | Microsoft                               | İzleme             |

Oluştururken bir `ILogger` , günlükleri yazmak için nesne `ILoggerFactory` nesne bu Günlükçü için uygulanacak sağlayıcı başına tek bir kural seçer. Tüm iletileri tarafından yazılan `ILogger` nesne seçili kuralları göre filtrelenir. Her bir sağlayıcı ve kategori çifti için olası en belirgin kural kullanılabilir kurallardan seçilir.

Aşağıdaki algoritmadan her sağlayıcısı için kullanılan zaman bir `ILogger` için belirli bir kategori oluşturulur:

* Sağlayıcı veya diğer adıyla eşleşen tüm kuralları'nı seçin. Bulunursa, tüm kuralları ile boş bir sağlayıcı seçin.
* Önceki adımda sonuçtan seçme kuralları ile en uzun kategori ön ek eşleştirme. Bulunursa, bir kategori belirtmeyin tüm kuralları'nı seçin.
* Birden çok kural seçtiyseniz ele **son** biri.
* Hiçbir kural seçtiyseniz, kullanın `MinimumLevel`.

Örneğin, önceki kuralların listesini varsa ve oluşturduğunuz düşünün bir `ILogger` nesne kategorisi "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" için:

* Hata ayıklama sağlayıcısı için 1, 6 ve 8 kuralları geçerlidir. Kural 8 en belirgin olduğundan, seçili durumdaki.
* Konsolu sağlayıcısı için 3, 4, 5 ve 6 kuralları geçerlidir. Kural 3 en belirgin değil.

Günlükleri ile oluşturduğunuzda bir `ILogger` "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" kategorisi için günlükleri `Trace` düzeyi ve hata ayıklama sağlayıcısı ve günlükleri için yukarıdaki geçer `Debug` düzeyi ve üzeri konsol sağlayıcıya geçer.

**Sağlayıcı diğer adları**

Tür adı, yapılandırmada bir sağlayıcısını belirtmek için kullanabilirsiniz, ancak her sağlayıcı daha kısa bir tanımlar *diğer* kullanımı daha kolay olmasıdır. Yerleşik sağlayıcılar için aşağıdaki diğer adlar kullanın:

- Konsol
- Hata ayıklama
- Olay günlüğü
- AzureAppServices
- TraceSource
- EventSource

**Varsayılan en düşük düzeyi**

Yalnızca yapılandırma veya kod hiçbir kural belirtilen sağlayıcı ve kategori için uygularsanız, etkinleşir en az bir düzeyi ayarı yoktur. Aşağıdaki örnek, en düşük düzeyi gösterilmektedir:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

En düşük düzey açıkça ayarlamazsanız, varsayılan değer: `Information`, anlamına `Trace` ve `Debug` günlükleri yok sayılır.

**Filtre işlevleri**

Filtreleme kurallarını uygulamak için bir filtre işlevi kod yazabilirsiniz. Tüm sağlayıcıları ve yapılandırma veya kod tarafından atanmış kural kategorisi için bir filtre işlevi çağrılır. İşlev kodu, sağlayıcı türü, kategori ve günlük düzeyi bir ileti günlüğe olup olmadığına karar vermek için erişimi vardır. Örneğin:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Bazı günlük sağlayıcıları, ne zaman günlükleri bir depolama ortamına veya göz ardı belirtin günlük düzeyi ve kategoriye göre olanak tanır.

`AddConsole` Ve `AddDebug` genişletme yöntemleri, filtre ölçütlerinde geçirmenize olanak tanıyan aşırı yükler sağlar. Aşağıdaki örnek kod konsol sağlayıcı günlüklere yok saymak neden `Warning` düzey sırasında hata ayıklama sağlayıcısı framework oluşturduğu günlükleri yok sayar.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Yöntemi olan alan bir aşırı yüklemesini bir `EventLogSettings` filtreleme bir işlevde içerebilen örneği kendi `Filter` özelliği. Kendi günlük düzeyi ve diğer parametrelere dayanır beri TraceSource sağlayıcısı bu aşırı yüklemeler hiçbirini sağlamaz `SourceSwitch` ve `TraceListener` kullanır.

Filtreleme kuralları ile kayıtlı tüm sağlayıcılar için ayarlayabileceğiniz bir `ILoggerFactory` kullanarak örneği `WithFilter` genişletme yöntemi. Aşağıdaki örnekte, uygulamada oturum hata ayıklama düzeyinde izin verirken framework günlüklerine uyarıları (kategorisi "Microsoft" veya "Sistem" ile başlayan) sınırlar.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Belirtebileceğiniz filtreleme için belirli bir kategori yazılan tüm günlükler önlemek için kullanmak istiyorsanız, `LogLevel.None` olarak bu kategori için en düşük günlük düzeyi. Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).

`WithFilter` Genişletme yöntemi tarafından sağlanır [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet paketi. Yeni bir yöntem döndürür `ILoggerFactory` tüm Günlükçü sağlayıcıları için geçirilen günlük iletileri filtreler örneği ile kayıtlı. Diğer etkilemez `ILoggerFactory` örnekleri, özgün dahil olmak üzere `ILoggerFactory` örneği.

::: moniker-end

## <a name="log-scopes"></a>Günlük kapsamları

Bir dizi içinde mantıksal işlemlerini gruplandırabilirsiniz bir *kapsam* bu kümesinin bir parçası oluşturulan her oturum için aynı verileri iliştirmek için. Örneğin, işlem kimliği eklemek için bir işlem işleme bir parçası olarak oluşturulan her günlük isteyebilirsiniz.

Bir kapsam bir `IDisposable` tarafından döndürülen tür [ILogger.BeginScope&lt;TState&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) yöntemi ve bırakılana kadar bağlanabilmelerini. Bir kapsam, Günlükçü çağrıları sarmalama tarafından kullandığınız bir `using` burada gösterildiği gibi engelleme:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Aşağıdaki kod, konsolu sağlayıcısı için kapsamları etkinleştirir:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.
>
> `IncludeScopes` aracılığıyla yapılandırılabilir *appsettings* yapılandırma dosyaları. Daha fazla bilgi için [ayarları dosya Yapılandırması](#settings-file-configuration) bölümü.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Her günlük iletisi kapsamlı bilgiler içerir:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Yerleşik günlük sağlayıcıları

ASP.NET Core aşağıdaki sağlayıcıları birlikte gelir:

* [Console](#console-provider)
* [Hata ayıklama](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [Azure uygulama hizmeti](#azure-app-service-provider)

### <a name="console-provider"></a>Konsolu sağlayıcısı

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcı paketi konsola günlük çıktısını gönderir. 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

[AddConsole aşırı](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) geçirdiğiniz olanak veren bir en düşük günlük düzeyi, bir filtre işlevi ve kapsamları desteklenip desteklenmediğini belirten bir Boole değeri. Başka bir seçenek de geçirmektir bir `IConfiguration` kapsamları desteği ve günlüğe kaydetme düzeylerini belirleyebilirsiniz nesnesidir. 

Üretim amaçlı kullanım için konsolu sağlayıcısı düşünüyorsanız, performansı üzerinde önemli bir etkisi olduğunu unutmayın.

Visual Studio'da yeni bir proje oluşturduğunuzda `AddConsole` yöntemi aşağıdaki gibi görünür:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Bu kod başvurduğu `Logging` bölümünü *appSettings.json* dosyası:

[!code-json[](index/sample//appsettings.json)]

Hata ayıklama düzeyinde açıklandığı şekilde oturum sınırı framework günlükleri uyarılar için uygulama izin verirken gösterilen ayarları [günlük filtreleme](#log-filtering) bölümü. Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).

::: moniker-end

### <a name="debug-provider"></a>Sağlayıcı hata ayıklama

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcı paketi kullanarak günlük çıktı Yazar [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) sınıfı (`Debug.WriteLine` yöntem çağrılarını).

Linux üzerinde bu sağlayıcı günlüklerine Yazar */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

[AddDebug aşırı](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) en düşük günlük düzeyi veya bir filtre işlevi geçirdiğiniz sağlar.

::: moniker-end

### <a name="eventsource-provider"></a>EventSource sağlayıcısı

ASP.NET Core 1.1.0 hedefleyen uygulamalar için veya daha yüksek [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcı paketi, olay izleme uygulayabilirsiniz. Windows üzerinde kullandığı [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Platformlar arası sağlayıcısıdır, ancak henüz Linux veya macOS için toplama ve görüntüleme araçları hiçbir olay yok. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

Toplamak ve günlükleri görüntülemek için en iyi yolu kullanmaktır [PerfView yardımcı programı](https://github.com/Microsoft/perfview). ETW günlükleri görüntülemek için diğer araçları vardır, ancak PerfView ASP.NET tarafından yayılan ETW olayları ile çalışmak için en iyi deneyimi sağlar. 

Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView yapılandırmak için dize Ekle `*Microsoft-Extensions-Logging` için **ek sağlayıcılar** listesi. (Yıldız işareti dizenin başlangıcında kaçırmayın.)

![Perfview ek sağlayıcılar](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Windows olay günlüğü sağlayıcısı

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcı paketi, Windows olay günlüğüne günlük çıktısını gönderir.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog aşırı](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) geçirdiğiniz let `EventLogSettings` veya en düşük günlük düzeyi.

::: moniker-end

### <a name="tracesource-provider"></a>TraceSource sağlayıcısı

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcı paketi kullanan [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) kitaplıkları ve sağlayıcıları.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

[AddTraceSource aşırı](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) kaynak anahtarı ve bir izleme dinleyicisi geçirdiğiniz sağlar.

Bu sağlayıcıyı kullanmak için .NET Framework (yerine üzerinde .NET Core) çalıştırmak bir uygulama sahiptir. Çeşitli iletileri yönlendirmek sağlayıcısı sağlar [dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners), gibi [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) kullanılan örnek uygulama.

Aşağıdaki örnek yapılandırır bir `TraceSource` günlükleri sağlayıcısı `Warning` ve konsol penceresinde daha yüksek ileti.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a>Azure App Service sağlayıcısı

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcı paketi metin dosyalarını bir Azure App Service uygulamanın dosya sistemi ve çok günlükler Yazar [blob depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) bir Azure depolama hesabındaki. Sağlayıcı, ASP.NET Core 1.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.

::: moniker range=">= aspnetcore-2.0"

.NET Core'u hedefleyen, yoksa sağlayıcı paketi yükleyin veya açıkça çağırmak [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics). Azure App Service'te uygulama dağıtıldığında sağlayıcı uygulamaya otomatik olarak kullanılabilir.

.NET Framework'ü hedefleyen, sağlayıcı paketini projeye ekleyin ve çağırma `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

Bir [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) geçirdiğiniz sağlar aşırı [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) ile geçersiz kılma günlük çıktı şablonu, blob adı ve dosya gibi varsayılan ayarları boyut sınırını aştı. (*Çıkış şablon* çağırdığınızda sağlayan bir üzerine tüm günlükler için uygulanan bir ileti şablonu bir `ILogger` yöntemi.)

::: moniker-end

Bir App Service uygulamasına dağıtmak, uygulama ayarlarında yapılırken [tanılama günlükleri](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) bölümünü **App Service** Azure Portalı'nın sayfasında. Bu ayarlar güncelleştirildiğinde, değişiklikleri hemen yeniden başlatma veya yeniden dağıtma işlemi uygulamanın gerek olmadan etkinleşir.

![Azure günlük ayarları](index/_static/azure-logging-settings.png)

Günlük dosyaları için varsayılan konum alıyor *D:\\giriş\\LogFiles\\uygulama* klasörü ve varsayılan dosya adı olan *tanılama yyyymmdd.txt*. Varsayılan dosya boyutu sınırını 10 MB'tır ve korunan dosyaları varsayılan en yüksek sayısı 2'dir. Varsayılan blob adı *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Varsayılan davranış hakkında daha fazla bilgi için bkz. [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).

Sağlayıcı, yalnızca proje Azure ortamında çalışan olduğunda çalışır. Projeyi yerel olarak çalıştırdığınızda herhangi bir etkisi&mdash;yerel dosyaları ya da yerel geliştirme deposu bloblar için yazma değil.

## <a name="third-party-logging-providers"></a>Üçüncü taraf günlük sağlayıcıları

ASP.NET Core ile çalışan üçüncü taraf günlük altyapılarına:

* [elmah.io](https://elmah.io/) ([GitHub deposunu](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposunu](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([GitHub deposunu](https://github.com/mperdeck/jsnlog))
* [Loggr](http://loggr.net/) ([GitHub deposunu](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([GitHub deposunu](https://github.com/NLog/NLog.Extensions.Logging))
* [Serilog](https://serilog.net/) ([GitHub deposunu](https://github.com/serilog/serilog-extensions-logging))

Bazı üçüncü taraf çerçeveleri gerçekleştirebilirsiniz [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Bir üçüncü taraf framework kullanarak, yerleşik sağlayıcılardan birini kullanmaya benzer:

1. Bir NuGet paketini projenize ekleyin.
1. Uzantı metodu çağırma `ILoggerFactory`.

Daha fazla bilgi için her framework'ün belgelerine bakın.

## <a name="azure-log-streaming"></a>Azure günlük akışı

Azure günlük akışını gerçek zamanlı günlüğünü etkinliği görüntülemenize olanak sağlar: 

* Uygulama sunucusu
* Web sunucusu
* Başarısız istek izleme

Azure günlük akışını yapılandırmak için:

* Gidin **tanılama günlükleri** sayfasından uygulamanızın portal sayfası
* Ayarlama **uygulama günlüğü (dosya sistemi)** açık.

![Azure portalı tanılama günlükleri sayfası](index/_static/azure-diagnostic-logs.png)

Gidin **günlük akışını** uygulama iletilerini görüntülemek için sayfa. Uygulama oturum açmadıysanız `ILogger` arabirimi.

![Azure portal uygulaması günlük akışı](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a>Azure Application Insights izleme günlüğü

[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK, ASP.NET Core günlük kaydı altyapısı aracılığıyla oluşturulan günlüklerinden izleme telemetrisi toplama özelliğine sahip. Daha fazla bilgi için [Applicationınsights/Microsoft-aspnetcore Wiki: günlük](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).

## <a name="additional-resources"></a>Ek kaynaklar

[LoggerMessage ile yüksek performans günlüğü](xref:fundamentals/logging/loggermessage)
