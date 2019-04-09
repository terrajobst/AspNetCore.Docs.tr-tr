---
title: ASP.NET core'da günlüğe kaydetme
author: tdykstra
description: ASP.NET core'da günlüğe kaydetme çerçevesi hakkında bilgi edinin. Yerleşik günlük sağlayıcıları bulmak ve popüler üçüncü taraf sağlayıcılar hakkında daha fazla bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 065b2016d3a2dcc2243ec6869e027c5fabe4dad8
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068410"
---
# <a name="logging-in-aspnet-core"></a>ASP.NET core'da günlüğe kaydetme

Tarafından [Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core çeşitli günlük yerleşik ve üçüncü taraf sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler. Bu makalede, yerleşik sağlayıcılar ile günlüğe kaydetme API'si kullanmayı gösterir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Sağlayıcıları Ekle

Oturum açma sağlayıcısı görüntüler veya günlüklerinde depolar. Örneğin, konsolu sağlayıcısı günlükleri konsolda görüntüler ve Azure Application Insights sağlayıcısı bunları Azure Application Insights içinde depolar. Günlükleri, birden çok sağlayıcı ekleyerek, birden çok hedefe gönderilebilir.

::: moniker range=">= aspnetcore-2.0"

Bir sağlayıcı eklemek için sağlayıcının çağrı `Add{provider name}` uzantı yönteminde *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

Varsayılan proje şablonu çağrılarının <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, aşağıdaki günlük kaydı sağlayıcıları ekler:

* Konsol
* Hata ayıklama
* EventSource (ASP.NET Core 2.2 içinde başlayarak)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Kullanırsanız `CreateDefaultBuilder`, varsayılan sağlayıcıları kendi seçtiklerinizle değiştirin. Çağrı <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, istediğiniz sağlayıcıları ekleyin.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Bir sağlayıcı kullanmak için kendi NuGet paketini yükleyin ve bir örneği üzerinde sağlayıcının uzantı metodu çağırma <xref:Microsoft.Extensions.Logging.ILoggerFactory>:

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) sağlar `ILoggerFactory` örneği. `AddConsole` Ve `AddDebug` genişletme yöntemleri tanımlanmış [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) ve [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paketleri. Her bir genişletme yöntemi çağıran `ILoggerFactory.AddProvider` sağlayıcının bir örneğini geçirerek yöntemi.

> [!NOTE]
> [Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) günlük sağlayıcıları ekler `Startup.Configure` yöntemi. Daha önce yürüten koddan günlük çıktısını almak için günlüğe kaydetme hizmeti sağlayıcıları Ekle `Startup` sınıf oluşturucusu.

::: moniker-end

Daha fazla bilgi edinin [yerleşik günlük sağlayıcıları](#built-in-logging-providers) ve [üçüncü taraf günlük sağlayıcıları](#third-party-logging-providers) makalenin ilerleyen bölümlerinde.

## <a name="create-logs"></a>Günlükleri oluşturma

Alma bir <xref:Microsoft.Extensions.Logging.ILogger%601> DI nesne.

::: moniker range=">= aspnetcore-2.0"

Aşağıdaki denetleyici örneği oluşturur `Information` ve `Warning` günlükleri. *Kategori* olduğu `TodoApiSample.Controllers.TodoController` (tam sınıf adını `TodoController` örnek uygulamada):

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Aşağıdaki Razor sayfaları örnek günlükleriyle oluşturur `Information` olarak *düzeyi* ve `TodoApiSample.Pages.AboutModel` olarak *kategori*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Yukarıdaki örnekte günlükleriyle oluşturur `Information` ve `Warning` olarak *düzeyi* ve `TodoController` olarak sınıf *kategori*. 

::: moniker-end

Günlük *düzeyi* günlüğe kaydedilen olayın önem derecesini gösterir. Günlük *kategori* her bir günlük ilişkilendirilmiş bir dizedir. `ILogger<T>` Örneği oluşturur türünde tam olarak nitelenmiş ada sahip günlükleri `T` kategori olarak. [Düzeyleri](#log-level) ve [kategorileri](#log-category) bu makalenin ilerleyen bölümlerinde daha ayrıntılı açıklanmıştır. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Başlangıç günlükleri oluşturma

Günlükleri yazmak için `Startup` sınıfı, içeren bir `ILogger` parametresinde Oluşturucu imzası:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a>Günlükleri oluşturma

Günlükleri yazmak için `Program` sınıfı, Al bir `ILogger` örneğinden dı:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Hiçbir zaman uyumsuz Günlükçü yöntemi

Günlük hızlı şekilde zaman uyumsuz kodun performans maliyeti karşılıyor olmadığından emin olmanız gerekir. Günlük veri deponuz yavaşsa, kendisine doğrudan yazmayın. Günlük iletilerini başlangıçta hızlı bir depoya yazmayı düşünebilirsiniz, sonra bunları daha sonra yavaş deposuna taşıyın. Örneğin, SQL Server oturum açtığınızdan, doğrudan Bunu yapmak istemediğiniz bir `Log` yöntemi, bu yana `Log` yöntemleri zaman uyumludur. Bunun yerine, zaman uyumlu bir bellek içi kuyruğuna günlük iletilerini ve SQL Server veri gönderme, zaman uyumsuz işi yapmak için sıradaki iletilerin çekme arka plan çalışanı vardır.

## <a name="configuration"></a>Yapılandırma

Sağlayıcı Yapılandırması günlüğe kaydetme, bir veya daha fazla yapılandırma sağlayıcıları tarafından sağlanır:

* Dosya biçimleri (INI, JSON ve XML).
* Komut satırı bağımsız değişkenleri.
* Ortam değişkenleri.
* Bellek içi .NET nesneleri.
* Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolama.
* Gibi bir şifrelenmiş kullanıcı depolamak [Azure anahtar kasası](xref:security/key-vault-configuration).
* Özel sağlayıcılar (veya oluşturulan yüklü).

Örneğin, günlük kaydı yapılandırması sık tarafından sağlanan `Logging` uygulama ayarları dosyaları bölümü. Aşağıdaki örnek, tipik bir içeriğini gösterir *appsettings. Development.JSON* dosyası:

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
      "IncludeScopes": true
    }
  }
}
```

`Logging` Özelliği olabilir `LogLevel` ve günlük sağlayıcısı özellikleri (konsol gösterilmiştir).

`LogLevel` Özelliği altında `Logging` en düşük belirtir [düzeyi](#log-level) için seçili kategorilerdeki oturum. Örnekte, `System` ve `Microsoft` kategorileri oturum sırasında `Information` düzeyi ve diğer tüm oturum sırasında `Debug` düzeyi.

Diğer özellikleri altında `Logging` günlük sağlayıcılarını belirtin. Örnek için konsol sağlayıcısıdır. Bir sağlayıcı destekliyorsa [oturum kapsamları](#log-scopes), `IncludeScopes` ayarların etkinleştirildiğinden olup olmadığını gösterir. Sağlayıcı özelliği (gibi `Console` örnekte) ayrıca bir `LogLevel` özelliği. `LogLevel` bir sağlayıcı altında bu sağlayıcı için oturum düzeyleri belirtir.

Düzeyleri belirtilirse `Logging.{providername}.LogLevel`, herhangi bir şey kümesinde geçersiz kılma `Logging.LogLevel`.

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

Uygulama yapılandırma sağlayıcıları hakkında daha fazla bilgi için bkz: <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Örnek günlük çıktısı

Komut satırından uygulama çalıştırıldığında önceki bölümde gösterilen örnek kod ile günlükleri konsolda görünür. Konsol çıktının bir örneği aşağıda verilmiştir:

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

Önceki günlükleri konumundaki örnek uygulamaya bir HTTP Get isteği yapılarak oluşturulan `http://localhost:5000/api/todo/0`.

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

Tarafından oluşturulan günlükleri `ILogger` çağrıları önceki bölümde gösterilen "TodoApi.Controllers.TodoController" ile başlar. "Microsoft" Kategoriler ile başlayan ASP.NET Core framework koddan günlüklerdir. ASP.NET Core ve uygulama kodu aynı günlüğe kaydetme API'si ve sağlayıcıları kullanmaktadır.

Bu makalenin geri kalanında, bazı ayrıntılar ve günlüğe kaydetme seçeneklerini açıklar.

## <a name="nuget-packages"></a>NuGet paketleri

`ILogger` Ve `ILoggerFactory` arabirimdir içinde [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ve bunlar için varsayılan uygulamalarıdır içinde [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Günlük kategorisi

Olduğunda bir `ILogger` nesnesi oluşturulur, bir *kategori* için belirtilir. Bu örneği tarafından oluşturulan her günlük iletisi kategori dahildir `ILogger`. Kategori herhangi bir dize olabilir, ancak kuralı, "TodoApi.Controllers.TodoController" gibi bir sınıf adı kullanmaktır.

Kullanım `ILogger<T>` almak için bir `ILogger` tam olarak nitelenmiş tür adını kullanan örnek `T` kategori olarak:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Kategori açıkça belirtmek için çağrı `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` çağırmakla eşdeğerdir `CreateLogger` tam olarak nitelenmiş tür adını `T`.

## <a name="log-level"></a>Günlük düzeyi

Her günlük belirtir bir <xref:Microsoft.Extensions.Logging.LogLevel> değeri. Önem derecesi veya önem günlük düzeyini belirtir. Örneğin, yazabilirsiniz bir `Information` normalde bir yöntem sona erdiğinde ve günlük `Warning` oturum bir yöntem döndürüldüğünde bir *404 Bulunamadı* durum kodu.

Aşağıdaki kod oluşturur `Information` ve `Warning` günlükleri:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Önceki kodda, ilk parametredir [oturum öğesini belirten Olay No.](#log-event-id). İkinci parametre, kalan yöntem parametreleri tarafından sağlanan bağımsız değişken değerleri yer tutucuları olan bir ileti şablonudur. Yöntem parametreleri açıklandığı [ileti şablon bölümü](#log-message-template) bu makalenin ilerleyen bölümlerinde.

Günlük düzeyi yöntem adı'içeren yöntemleri (örneğin, `LogInformation` ve `LogWarning`) olan [için ILogger genişletme yöntemleri](xref:Microsoft.Extensions.Logging.LoggerExtensions). Bu yöntemleri çağırmak bir `Log` gereken yöntemini bir `LogLevel` parametresi. Çağırabilirsiniz `Log` biri bu genişletme yöntemleri, ancak söz dizimi yerine doğrudan yöntemi nispeten karmaşık. Daha fazla bilgi için <xref:Microsoft.Extensions.Logging.ILogger> ve [Günlükçü uzantılarını kaynak kodu](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

ASP.NET Core burada en düşük en üst derecesi sıralı aşağıdaki günlük düzeyleri tanımlar.

* İzleme = 0

  Yalnızca hata ayıklama için genellikle değerli bilgiler. Bu iletiler, uygulama hassas verileri içerebilir ve bir üretim ortamında bu nedenle etkin olmamalıdır. *Varsayılan olarak devre dışıdır.*

* Hata ayıklama = 1

  Bilgi için geliştirme ve hata ayıklama için yararlı olabilir. Örnek: `Entering method Configure with flag set to true.` Etkinleştirme `Debug` düzeyi yalnızca, yüksek hacimli günlükleri nedeniyle sorun giderme sırasında üretimde günlüğe kaydeder.

* Bilgi = 2

  Uygulama'nın genel akışı izlemek için. Bu günlükler genellikle uzun vadeli bir değer var. Örnek: `Request received for path /api/todo`

* Uyarı = 3

  Olağan dışı ya da beklenmeyen olaylarını için uygulama akışı. Bunlar, hatalar veya durdurmak uygulamayı neden olmaz ancak incelenmesi gerekebilecek diğer koşulları olabilir. İşlenmiş istisnaları kullanmak için ortak bir yerde `Warning` günlük düzeyi. Örnek: `FileNotFoundException for file quotes.txt.`

* Hata = 4

  Hatalar ve özel durumlar için işlenemez. Bu iletiler, geçerli etkinliği ya da (örneğin, geçerli HTTP isteği) işlemi bir hata, uygulama genelinde hata gösterir. Örnek günlük iletisi: `Cannot insert record due to duplicate key violation.`

* Kritik = 5

  Hemen ilgilenilmesi gereken hataları. Örnekler: veri kaybı senaryolarına, disk alanı yetersiz.

Ne kadar günlük çıktısını bir belirli depolama ortamına yazılır denetlemek için günlük düzeyi kullanın veya penceresini görüntüleyin. Örneğin:

* Üretim ortamında, gönderme `Trace` aracılığıyla `Information` toplu veri deposuna düzeyi. Gönderme `Warning` aracılığıyla `Critical` için bir değer veri deposu.
* Geliştirme sırasında Gönder `Warning` aracılığıyla `Critical` konsola ekleyin `Trace` aracılığıyla `Information` sorunlarını giderirken.

[Günlük filtreleme](#log-filtering) bu makalenin devamındaki bölümüne bir sağlayıcı işleme hangi günlük düzeyleri denetlemek nasıl açıklar.

ASP.NET Core framework olayları için günlükler yazar. Günlük örnekleri, bu makalede daha önce günlüklere hariç tutuldu. `Information` düzeyi, dolayısıyla `Debug` veya `Trace` düzeyi günlükleri oluşturulmuş. İşte bir örnek konsol günlüklerinin göstermek için örnek uygulamayı çalıştırarak üretilmiş `Debug` günlükleri:

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

Her günlük belirtebilirsiniz bir *öğesini belirten Olay No*. Örnek uygulamayı yerel olarak tanımlanan kullanarak bunu yapar `LoggingEvents` sınıfı:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Olay Kimliği bir etkinlik kümesi ilişkilendirir. Örneğin, bir sayfada öğelerinin listesini görüntülemek için ilgili tüm günlükler 1001 olabilir.

Oturum açma sağlayıcısı, bir Kimliği alanında günlük iletisi ya da hiç olay kimliği depolayabilir. Hata ayıklama sağlayıcısı olay kimlikleri göstermez. Konsolu sağlayıcısı olay kimlikleri, köşeli ayraç içinde kategori sonra gösterir:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Günlük ileti şablonu

Her günlük iletisi şablonu belirtir. İleti şablonunu argüman sağlanmıyorsa yer tutucuları içerebilir. Sayılar değil yer tutucu adları kullanın.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Hangi parametrelerin değerlerini sağlamak için kullanılan yer tutucuları, bunların adları sırasını belirler. Aşağıdaki kodda, parametre adları ileti şablonunu sırayla dışında olduğuna dikkat edin:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Bu kod, parametre değerleriniz ile sırada bir günlük iletisi oluşturur:

```
Parameter values: parm1, parm2
```

Günlüğe kaydetme çerçevesi, bu şekilde çalışır, böylece günlük sağlayıcıları uygulayabilirsiniz [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Bağımsız değişkenleri kendilerini yalnızca biçimlendirilmiş ileti şablonunu, günlüğe kaydetme sistemi geçirilir. Günlüğe kaydetme sağlayıcıları alanları olarak parametre değerlerini depolamak bu bilgileri sağlar. Örneğin, Günlükçü yöntemi çağrıları görünüm şöyle düşünün:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Azure tablo depolaması için günlükleri gönderiyorsanız, her Azure Tablo varlığı olabilir `ID` ve `RequestTime` sorgu günlüğü verilerini kolaylaştıran özellikler. Bir sorgu içinde belirli bir tüm günlükler bulabilirsiniz `RequestTime` olmadan ayrıştırma, kısa mesaj zaman aşımı aralığı.

## <a name="logging-exceptions"></a>Özel durumları

Günlükçü yöntemleri aşağıdaki örnekteki gibi bir özel durum geçirmenize olanak tanıyan aşırı yüklemeleri vardır:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Farklı sağlayıcıları, özel durum bilgilerini farklı yollarla işler. Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktının bir örneği aşağıda verilmiştir.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Günlük Filtresi

::: moniker range=">= aspnetcore-2.0"

En düşük günlük düzeyi veya tüm sağlayıcıları ya da tüm kategorileri özel sağlayıcı ve kategori için belirtebilirsiniz. Bunlar görüntülenen veya depolanan olmayan şekilde en düşük düzeyin herhangi bir günlük bu sağlayıcısına geçirilen değildir.

Tüm günlükler bastırmak için belirtin `LogLevel.None` en düşük günlük düzeyi olarak. Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Yapılandırma filtresi kuralları oluşturabilir

Proje şablonu kod çağrıları `CreateDefaultBuilder` konsol ve hata ayıklama sağlayıcıları için günlük kaydı ayarlamak için. `CreateDefaultBuilder` Yöntemi ayrıca ayarlar günlüğünü yapılandırmasında aramak için bir `Logging` bölümünde, aşağıdaki gibi kod kullanarak:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

Yapılandırma verileri sağlayıcısı ve kategorisi, aşağıdaki örnekte olduğu gibi en az günlük düzeyleri belirtir:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Bu JSON altı bir filtre kuralları oluşturur: bir hata ayıklama sağlayıcısı, dört Konsolu sağlayıcısı için ve biri tüm sağlayıcılar için. Tek bir kural her sağlayıcısı için seçilen zaman bir `ILogger` nesnesi oluşturulur.

### <a name="filter-rules-in-code"></a>Kod içinde filtre kuralları

Aşağıdaki örnek, kod içinde filtre kuralları kaydedebilir gösterilmektedir:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

İkinci `AddFilter` , tür adını kullanarak hata ayıklama sağlayıcıyı belirtir. İlk `AddFilter` bir sağlayıcı türü belirtmeyen çünkü tüm sağlayıcılar için geçerlidir.

### <a name="how-filtering-rules-are-applied"></a>Nasıl filtreleme kuralları uygulanır

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

Olduğunda bir `ILogger` nesnesi oluşturulur, `ILoggerFactory` nesne bu Günlükçü için uygulanacak sağlayıcı başına tek bir kural seçer. Tüm iletileri tarafından yazılmış bir `ILogger` örneği seçili kuralları göre filtrelenir. Her bir sağlayıcı ve kategori çifti için olası en belirgin kural kullanılabilir kurallardan seçilir.

Aşağıdaki algoritmadan her sağlayıcısı için kullanılan zaman bir `ILogger` için belirli bir kategori oluşturulur:

* Sağlayıcı veya diğer adıyla eşleşen tüm kuralları'nı seçin. Eşleşme bulunursa, tüm kuralları ile boş bir sağlayıcı seçin.
* Önceki adımda sonuçtan seçme kuralları ile en uzun kategori ön ek eşleştirme. Eşleşme bulunursa, bir kategori belirtmeyin tüm kuralları'nı seçin.
* Birden çok kural seçtiyseniz ele **son** biri.
* Hiçbir kural seçtiyseniz, kullanın `MinimumLevel`.

İle önceki kurallar listesinde, oluşturduğunuz düşünün bir `ILogger` nesne kategorisi "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" için:

* Hata ayıklama sağlayıcısı için 1, 6 ve 8 kuralları geçerlidir. Kural 8 en belirgin olduğundan, seçili durumdaki.
* Konsolu sağlayıcısı için 3, 4, 5 ve 6 kuralları geçerlidir. Kural 3 en belirgin değil.

Ortaya çıkan `ILogger` örneği gönderir günlüklerini `Trace` düzeyi ve yukarıdaki hata ayıklama sağlayıcısı. Günlükleri `Debug` düzeyi ve konsol sağlayıcısına yukarıdaki gönderilir.

### <a name="provider-aliases"></a>Sağlayıcı diğer adları

Her bir sağlayıcı tanımlar bir *diğer* yapılandırmasında tam nitelikli tür adı yerine kullanılabilir.  Yerleşik sağlayıcılar için aşağıdaki diğer adlar kullanın:

* Konsol
* Hata ayıklama
* EventLog
* AzureAppServices
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>Varsayılan en düşük düzeyi

Yalnızca yapılandırma veya kod hiçbir kural belirtilen sağlayıcı ve kategori için uygularsanız, etkinleşir en az bir düzeyi ayarı yoktur. Aşağıdaki örnek, en düşük düzeyi gösterilmektedir:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

En düşük düzey açıkça ayarlamazsanız, varsayılan değer: `Information`, anlamına `Trace` ve `Debug` günlükleri yok sayılır.

### <a name="filter-functions"></a>Filtre işlevleri

Tüm sağlayıcıları ve yapılandırma veya kod tarafından atanmış kural kategorisi için bir filtre işlevi çağrılır. İşlev kodu, sağlayıcı türü, kategori ve günlük düzeyi erişimi vardır. Örneğin:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Bazı günlük sağlayıcıları, ne zaman günlükleri bir depolama ortamına veya göz ardı belirtin günlük düzeyi ve kategoriye göre olanak tanır.

`AddConsole` Ve `AddDebug` genişletme yöntemleri filtreleme ölçütleri kabul eden aşırı yükler sağlar. Aşağıdaki örnek kod konsol sağlayıcı günlüklere yok saymak neden `Warning` düzey sırasında hata ayıklama sağlayıcısı framework oluşturduğu günlükleri yok sayar.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` Yöntemi olan alan bir aşırı yüklemesini bir `EventLogSettings` filtreleme bir işlevde içerebilen örneği kendi `Filter` özelliği. Kendi günlük düzeyi ve diğer parametrelere dayanır beri TraceSource sağlayıcısı bu aşırı yüklemeler hiçbirini sağlamaz `SourceSwitch` ve `TraceListener` kullanır.

Filtreleme kuralları ile kayıtlı tüm sağlayıcılar için ayarlanacak bir `ILoggerFactory` kullanın, örnek `WithFilter` genişletme yöntemi. Aşağıdaki örnek, uygulama kodu tarafından oluşturulan günlükleri için hata ayıklama düzeyinde açılırken uyarıları framework günlükleri (kategorisi "Microsoft" veya "Sistem" ile başlayan) sınırlar.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Yazılmakta olan herhangi bir günlük önlemek için şunu belirtin `LogLevel.None` en düşük günlük düzeyi olarak. Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).

`WithFilter` Genişletme yöntemi tarafından sağlanır [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet paketi. Yeni bir yöntem döndürür `ILoggerFactory` tüm Günlükçü sağlayıcıları için geçirilen günlük iletileri filtreler örneği ile kayıtlı. Diğer etkilemez `ILoggerFactory` örnekleri, özgün dahil olmak üzere `ILoggerFactory` örneği.

::: moniker-end

## <a name="system-categories-and-levels"></a>Sistem kategorileri ve düzeyleri

Hangi günlüklerin bunları beklediğiniz hakkında notlar ile ASP.NET Core ve Entity Framework Core tarafından kullanılan bazı kategorileri şunlardır:

| Kategori                            | Notlar |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | ASP.NET Core genel tanılama. |
| Microsoft.AspNetCore.DataProtection | Hangi anahtarları kabul bulundu kullanılan ve. |
| Microsoft.AspNetCore.HostFiltering  | İzin verilen bir ana bilgisayar. |
| Microsoft.AspNetCore.Hosting        | Tamamlanması için geçen süreyi HTTP istekleri ve ne zaman başladıklarını. Hangi barındırma başlangıç derlemeler yüklü. |
| Microsoft.AspNetCore.Mvc            | MVC ve Razor tanılama. Model bağlama, filtre yürütme, görünüm derlemesi eylem seçimi. |
| Microsoft.AspNetCore.Routing        | Bilgi eşleşen rota. |
| Microsoft.AspNetCore.Server         | Bağlantı başlatma, durdurma ve canlı tutma yanıtlar. HTTPS sertifika bilgileri. |
| Microsoft.AspNetCore.StaticFiles    | Dosyaları sunulur. |
| Microsoft.EntityFrameworkCore       | Genel Entity Framework Core tanılama. Veritabanı geçişlerini etkinlik ve değişiklik algılama yapılandırması. |

## <a name="log-scopes"></a>Günlük kapsamları

 A *kapsam* mantıksal işlem kümesi gruplayabilirsiniz. Bu gruplandırma, aynı veri kümesinin bir parçası oluşturulan her bir günlüğe eklemek için kullanılabilir. Örneğin, bir işlem işleme bir parçası olarak oluşturulan her günlük işlem kimliği içerebilir

Bir kapsam bir `IDisposable` tarafından döndürülen tür <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> yöntemi ve bırakılana kadar bağlanabilmelerini. Günlükçü çağrılarında sarmalama tarafından bir kapsamı kullanan bir `using` engelle:

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Aşağıdaki kod, konsolu sağlayıcısı için kapsamları etkinleştirir:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.
>
> Yapılandırması hakkında daha fazla bilgi için bkz: [yapılandırma](#configuration) bölümü.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

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

* [Konsol](#console-provider)
* [Hata ayıklama](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)

Seçenekler [Azure'da oturum](#logging-in-azure) bu makalenin sonraki bölümlerinde ele alınmaktadır.

Stdout günlüğü hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> ve <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

### <a name="console-provider"></a>Konsolu sağlayıcısı

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcı paketi konsola günlük çıktısını gönderir. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[AddConsole aşırı](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) en düşük günlük düzeyi, bir filtre işlevi ve kapsamları desteklenip desteklenmediğini belirten bir Boole değeri geçirdiğiniz sağlar. Başka bir seçenek de geçirmektir bir `IConfiguration` kapsamları desteği ve günlüğe kaydetme düzeylerini belirleyebilirsiniz nesnesidir.

Konsolu sağlayıcısı performansı üzerinde önemli bir etkisi vardır ve genellikle üretim amaçlı kullanım için uygun değildir.

Visual Studio'da yeni bir proje oluşturduğunuzda `AddConsole` yöntemi aşağıdaki gibi görünür:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Bu kod başvurduğu `Logging` bölümünü *appSettings.json* dosyası:

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

Hata ayıklama düzeyinde açıklandığı şekilde oturum sınırı framework günlükleri uyarılar için uygulama izin verirken gösterilen ayarları [günlük filtreleme](#log-filtering) bölümü. Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).

::: moniker-end

Konsol çıkış günlüğü görmek için proje klasöründeki bir komut istemi açın ve aşağıdaki komutu çalıştırın:

```console
dotnet run
```

### <a name="debug-provider"></a>Sağlayıcı hata ayıklama

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcı paketi kullanarak günlük çıktı Yazar [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) sınıfı (`Debug.WriteLine` yöntem çağrılarını).

Linux üzerinde bu sağlayıcı günlüklerine Yazar */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

[AddDebug aşırı](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) en düşük günlük düzeyi veya bir filtre işlevi geçirdiğiniz sağlar.

::: moniker-end

### <a name="eventsource-provider"></a>EventSource sağlayıcısı

ASP.NET Core 1.1.0 hedefleyen uygulamalar için veya sonraki bir sürümü [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcı paketi, olay izleme uygulayabilirsiniz. Windows üzerinde kullandığı [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Platformlar arası sağlayıcısıdır, ancak henüz Linux veya macOS için toplama ve görüntüleme araçları hiçbir olay yok.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

Toplamak ve günlükleri görüntülemek için en iyi yolu kullanmaktır [PerfView yardımcı programı](https://github.com/Microsoft/perfview). ETW günlükleri görüntülemek için diğer araçları vardır, ancak PerfView ASP.NET tarafından yayılan ETW olayları ile çalışmak için en iyi deneyimi sağlar.

Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView yapılandırmak için dize Ekle `*Microsoft-Extensions-Logging` için **ek sağlayıcılar** listesi. (Yıldız işareti dizenin başlangıcında kaçırmayın.)

![Perfview ek sağlayıcılar](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Windows olay günlüğü sağlayıcısı

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcı paketi, Windows olay günlüğüne günlük çıktısını gönderir.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

[AddEventLog aşırı](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) geçirdiğiniz let `EventLogSettings` veya en düşük günlük düzeyi.

::: moniker-end

### <a name="tracesource-provider"></a>TraceSource sağlayıcısı

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcı paketi kullanan <xref:System.Diagnostics.TraceSource> kitaplıkları ve sağlayıcıları.

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

[AddTraceSource aşırı](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) kaynak anahtarı ve bir izleme dinleyicisi geçirdiğiniz sağlar.

Bu sağlayıcıyı kullanmak için .NET Framework (yerine üzerinde .NET Core) çalıştırmak bir uygulama vardır. Sağlayıcı için çeşitli iletiler yönlendirebilirsiniz [dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners), gibi <xref:System.Diagnostics.TextWriterTraceListener> örnek uygulamada kullanılan.

::: moniker range="< aspnetcore-2.0"

Aşağıdaki örnek yapılandırır bir `TraceSource` günlükleri sağlayıcısı `Warning` ve konsol penceresinde daha yüksek ileti.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a>Azure'da günlüğe kaydetme

Azure'da günlüğe kaydetme hakkında daha fazla bilgi için aşağıdaki bölümlere bakın:

* [Azure App Service sağlayıcısı](#azure-app-service-provider)
* [Azure günlük akışı](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [Azure Application Insights izleme günlüğü](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a>Azure App Service sağlayıcısı

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcı paketi metin dosyalarını bir Azure App Service uygulamanın dosya sistemi ve çok günlükler Yazar [blob depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) bir Azure depolama hesabındaki. Sağlayıcı paketi, .NET Core 1.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.

::: moniker range=">= aspnetcore-2.0"

.NET Core'u hedefleyen, aşağıdaki noktalara dikkat edin:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* ASP.NET Core sağlayıcısı paketinde [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Sağlayıcı paketi bulunup bulunmadığına [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Sağlayıcıyı kullanmak için paketi yükleyin.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* Açıkça Remove() çağırmayın <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>. Azure App Service'te uygulama dağıtıldığında sağlayıcısı otomatik olarak uygulama için kullanılabilir hale getirilir.

.NET Framework'ü hedefleyen veya başvuru `Microsoft.AspNetCore.App` metapackage, sağlayıcı paketini projeye ekleyin. Çağırma `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

Bir <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> geçirdiğiniz sağlar aşırı <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. Ayarlar nesnesini günlük çıktı şablonu, blob adı ve dosya boyutu sınırını gibi varsayılan ayarları geçersiz kılabilirsiniz. (*Çıkış şablon* ile sağlanan ek olarak, tüm günlükleri uygulanan bir ileti şablonu bir `ILogger` yöntem çağrısı.)

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Sağlayıcı ayarlarını yapılandırmak için kullanmayı <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> ve <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

Bir App Service uygulamasına dağıtmak, uygulama ayarlarında yapılırken [tanılama günlükleri](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) bölümünü **App Service** Azure Portalı'nın sayfasında. Bu ayarlar güncelleştirildiğinde, değişiklikleri hemen yeniden başlatma veya yeniden dağıtma işlemi uygulamanın gerek olmadan etkinleşir.

![Azure günlük ayarları](index/_static/azure-logging-settings.png)

Günlük dosyaları için varsayılan konum alıyor *D:\\giriş\\LogFiles\\uygulama* klasörü ve varsayılan dosya adı olan *tanılama yyyymmdd.txt*. Varsayılan dosya boyutu sınırını 10 MB'tır ve korunan dosyaları varsayılan en yüksek sayısı 2'dir. Varsayılan blob adı *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.

Sağlayıcı, yalnızca proje Azure ortamında çalışan olduğunda çalışır. Projeyi yerel olarak çalıştırdığınızda herhangi bir etkisi&mdash;yerel dosyaları ya da yerel geliştirme deposu bloblar için yazma değil.

### <a name="azure-log-streaming"></a>Azure günlük akışı

Azure günlük akışını gerçek zamanlı günlüğünü etkinliği görüntülemenize olanak tanır:

* Uygulama sunucusu
* Web sunucusu
* Başarısız istek izleme

Azure günlük akışını yapılandırmak için:

* Gidin **tanılama günlükleri** sayfasından uygulamanızın portal sayfası.
* Ayarlama **uygulama günlüğü (dosya sistemi)** için **üzerinde**.

![Azure portalı tanılama günlükleri sayfası](index/_static/azure-diagnostic-logs.png)

Gidin **günlük akışını** uygulama iletilerini görüntülemek için sayfa. Tarafından uygulama üzerinden oturum açmadıysanız `ILogger` arabirimi.

![Azure portal uygulaması günlük akışı](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Azure Application Insights izleme günlüğü

Application Insights SDK'sı, toplamak ve ASP.NET Core günlük kaydı altyapısı tarafından oluşturulan günlükleri bildirin. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Application Insights'a genel bakış](/azure/application-insights/app-insights-overview)
* [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core)
* [Application Insights bağdaştırıcılarını günlüğü](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).
* [Application Insights ILogger uygulama örnekleri](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a>Üçüncü taraf günlük sağlayıcıları

ASP.NET Core ile çalışan üçüncü taraf günlük altyapılarına:

* [elmah.io](https://elmah.io/) ([GitHub deposunu](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposunu](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([GitHub deposunu](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub deposunu](https://github.com/catalingavan/KissLog-net))
* [Loggr](http://loggr.net/) ([GitHub deposunu](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([GitHub deposunu](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([GitHub deposunu](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub deposunu](https://github.com/serilog/serilog-extensions-logging))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github deposunu](https://github.com/googleapis/google-cloud-dotnet))

Bazı üçüncü taraf çerçeveleri gerçekleştirebilirsiniz [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Bir üçüncü taraf framework kullanarak, yerleşik sağlayıcılardan birini kullanmaya benzer:

1. Bir NuGet paketini projenize ekleyin.
1. Çağrısı bir `ILoggerFactory`.

Daha fazla bilgi için her sağlayıcının belgelerine bakın. Üçüncü taraf günlük sağlayıcıları, Microsoft tarafından desteklenmez.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/logging/loggermessage>
