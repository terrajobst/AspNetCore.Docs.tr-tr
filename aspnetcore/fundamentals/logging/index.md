---
title: ASP.NET Core oturum açma
author: tdykstra
description: ASP.NET Core 'de günlük çerçevesini öğrenin. Yerleşik günlük sağlayıcılarını bulun ve popüler üçüncü taraf sağlayıcılar hakkında daha fazla bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/logging/index
ms.openlocfilehash: cdb5cad2302cc8ec02107021005b8590efce7533
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308213"
---
# <a name="logging-in-aspnet-core"></a>ASP.NET Core oturum açma

[Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra) tarafından

ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler. Bu makalede, yerleşik sağlayıcılarla günlüğe kaydetme API 'sinin nasıl kullanılacağı gösterilmektedir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Sağlayıcı Ekle

Günlük sağlayıcısı günlükleri görüntüler veya depolar. Örneğin, konsol sağlayıcısı günlükleri konsolunda görüntüler ve Azure Application Insights sağlayıcısı bunları Azure Application Insights depolar. Birden çok sağlayıcı eklenerek Günlükler birden çok hedefe gönderilebilir.

::: moniker range=">= aspnetcore-2.0"

Bir sağlayıcı eklemek için sağlayıcının `Add{provider name}` uzantı yöntemini *program.cs*içinde çağırın:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

Yukarıdaki kod, ve `Microsoft.Extensions.Logging` `Microsoft.Extensions.Configuration`için başvuruları gerektirir.

Varsayılan proje şablonu, aşağıdaki <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>günlük sağlayıcılarını ekleyen çağırır:

* Konsol
* Hata ayıklama
* EventSource (ASP.NET Core 2,2 ' den başlayarak)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

`CreateDefaultBuilder`Kullanıyorsanız, varsayılan sağlayıcıları kendi seçimlerinizle değiştirebilirsiniz. Öğesini <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>çağırın ve istediğiniz sağlayıcıları ekleyin.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Bir sağlayıcıyı kullanmak için, NuGet paketini yükleyip sağlayıcının uzantı yöntemini bir örneğine <xref:Microsoft.Extensions.Logging.ILoggerFactory>çağırın:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [bağımlılığı ekleme (dı)](xref:fundamentals/dependency-injection) `ILoggerFactory` örneği sağlar. Ve genişletme yöntemleri [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) ve [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paketlerinde tanımlanmıştır. `AddDebug` `AddConsole` Her genişletme yöntemi, sağlayıcının `ILoggerFactory.AddProvider` bir örneğini geçirerek yöntemini çağırır.

> [!NOTE]
> [Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) , `Startup.Configure` yöntemine günlük sağlayıcıları ekler. Daha önce yürütülen koddan günlük çıktısını almak için, `Startup` sınıf oluşturucusunda günlük sağlayıcılarını ekleyin.

::: moniker-end

Makalenin ilerleyen kısımlarında [yerleşik günlük sağlayıcıları](#built-in-logging-providers) ve [üçüncü taraf günlüğü sağlayıcıları](#third-party-logging-providers) hakkında daha fazla bilgi edinin.

## <a name="create-logs"></a>Günlükleri oluştur

Dı 'dan <xref:Microsoft.Extensions.Logging.ILogger%601> bir nesne al.

::: moniker range=">= aspnetcore-2.0"

Aşağıdaki denetleyici örneği ve `Information` `Warning` günlüklerini oluşturur. `TodoController` *Kategori* `TodoApiSample.Controllers.TodoController` (örnek uygulamadaki tam sınıf adı):

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Aşağıdaki Razor Pages örnek `Information` , *düzeyi* ve `TodoApiSample.Pages.AboutModel` *Kategori*olarak içeren Günlükler oluşturur:

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Yukarıdaki örnek, ve ile, `Information` *kategorisi*olarak `Warning` ve *düzeyi* `TodoController` olan Günlükler oluşturur. 

::: moniker-end

Günlük *düzeyi* günlüğe kaydedilen etkinliğin önem derecesini gösterir. Günlük *kategorisi* , her günlük ile ilişkili bir dizedir. Örnek, kategori olarak türünde `T` tam adı olan Günlükler oluşturur. `ILogger<T>` [Düzeyler](#log-level) ve [Kategoriler](#log-category) Bu makalenin ilerleyen kısımlarında daha ayrıntılı olarak açıklanmıştır. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Başlangıçta günlük oluşturma

`Startup` Sınıf içindeki günlükleri yazmak için, Oluşturucu imzasına bir `ILogger` parametre ekleyin:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a>Programda günlük oluşturma

`Program` Sınıf içindeki günlükleri yazmak için, dı 'den bir `ILogger` örnek alın:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Zaman uyumsuz günlükçü yöntemi yok

Günlüğe kaydetme, zaman uyumsuz kodun performans maliyetine değer olmaması kadar hızlı olmalıdır. Günlüğe kaydetme veri depoluizin yavaşsa, doğrudan buna yazmayın. Başlangıç olarak günlük iletilerini hızlı bir mağazaya yazmayı ve sonra yavaş depoya daha sonra taşımayı düşünün. Örneğin, SQL Server için oturum açıyorsanız `Log` `Log` Yöntemler zaman uyumlu olduğundan, bunu doğrudan bir yöntemde yapmak istemezsiniz. Bunun yerine, günlük iletilerini bir bellek içi kuyruğa eşzamanlı olarak ekleyin ve bir arka plan çalışanı, SQL Server veri gönderme zaman uyumsuz çalışmasını sağlamak için iletileri kuyruktan çekin.

## <a name="configuration"></a>Yapılandırma

Günlüğe kaydetme sağlayıcısı yapılandırması bir veya daha fazla yapılandırma sağlayıcısı tarafından sağlanır:

* Dosya biçimleri (ıNı, JSON ve XML).
* Komut satırı bağımsız değişkenleri.
* Ortam değişkenleri.
* Bellek içi .NET nesneleri.
* Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolaması.
* [Azure Key Vault](xref:security/key-vault-configuration)gibi şifreli bir kullanıcı deposu.
* Özel sağlayıcılar (yüklü veya oluşturulmuş).

Örneğin, günlük yapılandırma genellikle uygulama ayarları dosyalarının `Logging` bölümü tarafından sağlanır. Aşağıdaki örnek tipik bir appSettings 'in içeriğini gösterir *. Development. JSON* dosyası:

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

Özelliği `Logging` , ve günlük `LogLevel` sağlayıcısı özelliklerine sahip olabilir (konsol gösterilir).

Altındaki `LogLevel` özelliği`Logging` , Seçili kategoriler için günlüğe kaydedilecek minimum [düzeyi](#log-level) belirtir. Örnekte `System` ve `Microsoft` kategoriler `Information` düzeyinde günlüğe kaydedilir ve diğer tüm diğerleri `Debug` düzeyinde günlüğe kaydedilir.

Günlük sağlayıcılarını belirt `Logging` altındaki diğer özellikler. Örnek, konsol sağlayıcısına yöneliktir. Bir sağlayıcı, [günlük kapsamlarını](#log-scopes)destekliyorsa, `IncludeScopes` etkinleştirilip etkinleştirilmeyeceğini belirtir. Bir sağlayıcı özelliği (örnekte olduğu `Console` gibi), bir `LogLevel` özellik de belirtebilir. `LogLevel`sağlayıcı altında, bu sağlayıcının günlüğe kaydedilecek düzeyleri belirtir.

' De `Logging.{providername}.LogLevel`düzeyler belirtilirse, içinde `Logging.LogLevel`ayarlanan her şeyi geçersiz kılar.

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

`LogLevel`Anahtarlar günlük adlarını temsil eder. `Default` Anahtar açıkça listelenmemiş Günlükler için geçerlidir. Değer, belirtilen günlüğe uygulanan [günlük düzeyini](#log-level) temsil eder.

::: moniker-end

Yapılandırma sağlayıcılarını uygulama hakkında daha fazla bilgi için <xref:fundamentals/configuration/index>bkz.

## <a name="sample-logging-output"></a>Örnek günlüğe kaydetme çıkışı

Yukarıdaki bölümde gösterilen örnek kodla, uygulama komut satırından çalıştırıldığında Günlükler konsolunda görüntülenir. Konsol çıkışının bir örneği aşağıda verilmiştir:

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

Yukarıdaki Günlükler, üzerinde `http://localhost:5000/api/todo/0`örnek uygulamaya bir http get isteği yapılarak oluşturulmuştur.

Visual Studio 'da örnek uygulamayı çalıştırdığınızda hata ayıklama penceresinde göründükleri günlüklere yönelik bir örnek aşağıda verilmiştir:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Yukarıdaki bölümde gösterilen `ILogger` çağrılar tarafından oluşturulan Günlükler "TodoApi. Controllers. TodoController" ile başlar. "Microsoft" kategorileri ile başlayan Günlükler ASP.NET Core Framework kodundan alınır. ASP.NET Core ve uygulama kodu aynı günlük API 'sini ve sağlayıcılarını kullanıyor.

Bu makalenin geri kalanında günlüğe kaydetme için bazı ayrıntılar ve seçenekler açıklanmaktadır.

## <a name="nuget-packages"></a>NuGet paketleri

Ve arabirimleri Microsoft [. Extensions. Logging. soyutlamalar](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)içinde ve bu uygulamalar için varsayılan uygulamalar [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/)içinde bulunur. `ILoggerFactory` `ILogger`

## <a name="log-category"></a>Günlük kategorisi

Bir `ILogger` nesne oluşturulduğunda, için bir *Kategori* belirtilir. Bu kategori, bu örneği `ILogger`tarafından oluşturulan her günlük iletisine dahildir. Kategori herhangi bir dize olabilir, ancak kural, "TodoApi. Controllers. TodoController" gibi sınıf adını kullanmaktır.

Kategori `ILogger<T>` olarak`T` tam nitelikli `ILogger` tür adını kullanan bir örnek almak için kullanın:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Kategoriyi açıkça belirtmek için şunu çağırın `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>`, tam nitelikli tür `CreateLogger` `T`adı ile çağırma ile eşdeğerdir.

## <a name="log-level"></a>Günlük düzeyi

Her günlük bir <xref:Microsoft.Extensions.Logging.LogLevel> değer belirtir. Günlük düzeyi önem derecesini veya önemini gösterir. Örneğin, bir yöntem normal olarak sona `Information` erdiğinde `Warning` ve bir yöntem *404* bulunmayan bir durum kodu döndürdüğünde bir günlük yazabilirsiniz.

Aşağıdaki kod oluşturur `Information` ve `Warning` günlükleri:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Önceki kodda, ilk parametredir [oturum öğesini belirten Olay No.](#log-event-id) İkinci parametre, kalan Yöntem parametreleri tarafından belirtilen bağımsız değişken değerleri için yer tutucuları olan bir ileti şablonudur. Yöntem parametreleri bu makalenin ilerleyen kısımlarında bulunan [ileti şablonu bölümünde](#log-message-template) açıklanmaktadır.

Yöntem adında düzeyi (örneğin, `LogInformation` ve `LogWarning`) içeren günlük yöntemleri, [ILogger için uzantı yöntemleridir](xref:Microsoft.Extensions.Logging.LoggerExtensions). Bu yöntemler bir `LogLevel` parametre `Log` alan bir yöntemi çağırır. `Log` Yöntemi bu uzantı yöntemlerinden biri yerine doğrudan çağırabilirsiniz, ancak söz dizimi görece karmaşıktır. Daha fazla bilgi için bkz <xref:Microsoft.Extensions.Logging.ILogger> . ve [günlükçü uzantıları kaynak kodu](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

ASP.NET Core, en küçükten en yüksek öneme doğru sıralanan aşağıdaki günlük düzeylerini tanımlar.

* İzleme = 0

  Genellikle yalnızca hata ayıklama için değerli bilgiler için. Bu iletiler hassas uygulama verileri içerebilir, bu nedenle bir üretim ortamında etkinleştirilmemelidir. *Varsayılan olarak devre dışıdır.*

* Hata Ayıkla = 1

  Geliştirme ve hata ayıklama konusunda yararlı olabilecek bilgiler için. Örnek: `Entering method Configure with flag set to true.`En `Debug` yüksek günlük hacimden dolayı yalnızca sorun giderirken üretimde düzey günlüklerini etkinleştirin.

* Bilgi = 2

  Uygulamanın genel akışını izlemek için. Bu günlüklerde genellikle uzun süreli bir değer vardır. Örnek: `Request received for path /api/todo`

* Uyarı = 3

  Uygulama akışında anormal veya beklenmedik olaylar için. Bunlar, uygulamanın durmasına neden olmayan ancak araştırılması gerekebilecek hataları veya diğer koşulları içerebilir. İşlenmiş özel durumlar, `Warning` günlük düzeyini kullanmak için yaygın bir yerdir. Örnek: `FileNotFoundException for file quotes.txt.`

* Hata = 4

  İşlenemeyen hatalar ve özel durumlar için. Bu iletiler, uygulama genelinde bir hata değil geçerli etkinlikte veya işlemde (geçerli HTTP isteği gibi) bir hata olduğunu gösterir. Örnek günlük iletisi:`Cannot insert record due to duplicate key violation.`

* Kritik = 5

  Anında ilgilenilmesi gereken hatalarda. Örnekler: veri kaybı senaryoları, disk alanı yetersiz.

Belirli bir depolama ortamında veya görüntüleme penceresinde ne kadar günlük çıkışının yazıldığını denetlemek için günlük düzeyini kullanın. Örneğin:

* Üretimde, birim veri `Trace` deposuna `Information` geçiş düzeyi olarak gönderin. ' İ bir değer veri deposuna gönderin `Warning`. `Critical`
* Geliştirme sırasında konsola gönderin `Warning` `Critical` ve sorun giderme sırasında aracılığıyla `Information` ekleyin `Trace` .

Bu makalede daha sonra bulunan [günlük filtreleme](#log-filtering) bölümünde, bir sağlayıcının hangi günlük düzeylerinin işlediğini nasıl denetleneceği açıklanmaktadır.

ASP.NET Core çerçeve olayları için günlükleri yazar. Bu makalenin önceki kısımlarında yer alınan günlük örnekleri, günlüklerin `Information` altında tutulur, bu `Debug` nedenle `Trace` hiçbir veya düzeyi günlük oluşturulmaz. Günlükleri göstermek `Debug` için yapılandırılmış örnek uygulama çalıştırılarak oluşturulan konsol günlüklerinin bir örneği aşağıda verilmiştir:

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

## <a name="log-event-id"></a>Günlüğe olay KIMLIĞI

Her günlük belirtebilirsiniz bir *öğesini belirten Olay No*. Örnek uygulama bunu yerel olarak tanımlanmış `LoggingEvents` bir sınıf kullanarak yapar:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Olay KIMLIĞI bir olay kümesini ilişkilendirir. Örneğin, bir sayfadaki öğelerin listesini görüntülemek için ilgili tüm Günlükler 1001 olabilir.

Günlüğe kaydetme sağlayıcısı, olay KIMLIĞINI günlüğe kaydetme iletisindeki kimlik alanında veya hiç değil, bir kimlik alanında saklayabilir. Hata ayıklama sağlayıcısı olay kimliklerini göstermiyor. Konsol sağlayıcısı, etkinlik kimliklerini kategoriden sonra parantez içinde gösterir:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Günlük iletisi şablonu

Her günlük bir ileti şablonunu belirtir. İleti şablonu, bağımsız değişkenlerin sağlandığı yer tutucuları içerebilir. Sayılar değil, yer tutucular için adları kullanın.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Adlarının, değerlerinin sağlanması için hangi parametrelerin kullanılacağını belirleyen yer tutucular sırası. Aşağıdaki kodda, parametre adlarının ileti şablonunda sıra dışı olduğuna dikkat edin:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Bu kod, sırasıyla parametre değerleriyle bir günlük iletisi oluşturur:

```
Parameter values: parm1, parm2
```

Günlüğe kaydetme altyapısı bu şekilde çalışarak, günlük sağlayıcılarının [yapılandırılmış günlüğe yazma olarak da bilinen anlamsal günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulayabilmesini sağlayabilir. Bağımsız değişkenler yalnızca biçimli ileti şablonuna değil, günlük sistemine geçirilir. Bu bilgiler, günlük sağlayıcılarının parametre değerlerini alan olarak depolamasına olanak sağlar. Örneğin, günlükçü yönteminin şuna benzer şekilde göründüğünü varsayın:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Günlükleri Azure Tablo depolama alanına gönderiyorsanız, her bir Azure Tablo varlığı, günlük verilerinde sorguları basitleştiren `ID` ve `RequestTime` özelliklere sahip olabilir. Bir sorgu, belirli `RequestTime` bir aralıktaki tüm günlükleri, kısa mesajdan zaman aşımını ayrıştırmadan bulabilir.

## <a name="logging-exceptions"></a>Günlüğe kaydetme özel durumları

Günlükçü yöntemlerinin, aşağıdaki örnekte olduğu gibi bir özel durum iletmenizi sağlayan aşırı yüklemeleri vardır:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Özel durum bilgileri farklı yollarla farklı sağlayıcılarda işler. Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktısına bir örnek aşağıda verilmiştir.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Günlük filtreleme

::: moniker range=">= aspnetcore-2.0"

Belirli bir sağlayıcı ve kategori için en az bir günlük düzeyi veya tüm sağlayıcılar ya da tüm kategoriler için belirtebilirsiniz. Minimum düzeyin altındaki tüm Günlükler bu sağlayıcıya aktarılmaz, bu nedenle görüntülenmez veya depolanmaz.

Tüm günlükleri gizlemek için en düşük `LogLevel.None` günlük düzeyi olarak belirtin. Öğesinin `LogLevel.None` tamsayı değeri 6 ' dır `LogLevel.Critical` (5) daha yüksektir.

### <a name="create-filter-rules-in-configuration"></a>Yapılandırmada filtre kuralları oluşturma

Proje şablonu kodu, konsol `CreateDefaultBuilder` ve hata ayıklama sağlayıcılarının günlüğünü ayarlamak için çağırır. Yöntemi, aşağıdaki gibi bir kod kullanarak bir `Logging` bölümde yapılandırmayı aramak için günlüğe kaydetmeyi de ayarlar: `CreateDefaultBuilder`

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17)]

Yapılandırma verileri aşağıdaki örnekte olduğu gibi sağlayıcıya ve kategoriye göre en düşük günlük düzeylerini belirtir:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Bu JSON altı filtre kuralı oluşturur: biri hata ayıklama sağlayıcısı, konsol sağlayıcısı için dört ve diğeri tüm sağlayıcılar için. Bir `ILogger` nesne oluşturulduğunda her sağlayıcı için tek bir kural seçilir.

### <a name="filter-rules-in-code"></a>Koddaki filtre kuralları

Aşağıdaki örnek, koddaki filtre kurallarının nasıl kaydedileceği gösterilmektedir:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

İkincisi `AddFilter` , hata ayıklama sağlayıcısını tür adını kullanarak belirtir. Birincisi `AddFilter` bir sağlayıcı türü belirtmediğinden, tüm sağlayıcılar için geçerlidir.

### <a name="how-filtering-rules-are-applied"></a>Filtreleme kuralları nasıl uygulanır

Yapılandırma verileri ve `AddFilter` önceki örneklerde gösterilen kod, aşağıdaki tabloda gösterilen kuralları oluşturur. İlk altı yapılandırma örneğinde ve son iki ise kod örneğinde gelir.

| Sayı | Sağlayıcı      | Şununla başlayan Kategoriler...          | En düşük günlük düzeyi |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1\.      | Hata ayıklama         | Tüm Kategoriler                          | Bilgiler       |
| 2      | Konsol       | Microsoft.AspNetCore.Mvc.Razor.Internal | Uyarı           |
| 3      | Konsol       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Hata ayıklama             |
| 4      | Konsol       | Microsoft.AspNetCore.Mvc.Razor          | Hata             |
| 5      | Konsol       | Tüm Kategoriler                          | Bilgiler       |
| 6      | Tüm sağlayıcılar | Tüm Kategoriler                          | Hata ayıklama             |
| 7      | Tüm sağlayıcılar | Sistem                                  | Hata ayıklama             |
| 8      | Hata ayıklama         | Microsoft                               | İzlemesinin             |

Bir `ILogger` nesne oluşturulduğunda `ILoggerFactory` , nesne bu günlükçü için uygulanacak her sağlayıcı için tek bir kural seçer. Bir `ILogger` örnek tarafından yazılan tüm iletiler, seçilen kurallara göre filtrelenmiştir. Her sağlayıcı ve kategori çifti için mümkün olan en özel kural kullanılabilir kurallardan seçilir.

Belirli bir kategori için oluşturulan her sağlayıcı `ILogger` için aşağıdaki algoritma kullanılır:

* Sağlayıcı veya diğer adıyla eşleşen tüm kuralları seçin. Hiçbir eşleşme bulunmazsa, boş bir sağlayıcıya sahip tüm kurallar ' ı seçin.
* Önceki adımın sonucunda, en uzun eşleşen kategori ön ekine sahip kurallar ' ı seçin. Eşleşme bulunmazsa, kategori belirtmeyen tüm kuralları seçin.
* Birden çok kural seçilirse, **son** olanı götürün.
* Hiçbir kural seçilmezse, kullanın `MinimumLevel`.

Yukarıdaki kurallar listesinde, "Microsoft. aspnetcore. Mvc `ILogger` . Razor. RazorViewEngine" kategorisi için bir nesne oluşturduğunuzu varsayalım:

* Hata ayıklama sağlayıcısı, kurallar 1, 6 ve 8 için geçerlidir. Kural 8 ' i en özeldir, yani seçili olanı seçilidir.
* Konsol sağlayıcısı için, kurallar 3, 4, 5 ve 6 geçerlidir. Kural 3 en özeldir.

Elde edilen `ILogger` örnek, hata ayıklama `Trace` sağlayıcısına düzeyi ve üzeri Günlükler gönderir. `Debug` Düzeyi ve üzeri Günlükler konsol sağlayıcısına gönderilir.

### <a name="provider-aliases"></a>Sağlayıcı diğer adları

Her sağlayıcı, tam nitelikli tür adı yerine yapılandırmada kullanılabilecek bir *diğer ad* tanımlar.  Yerleşik sağlayıcılar için aşağıdaki diğer adları kullanın:

* Konsol
* Hata ayıklama
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>Varsayılan en düşük düzey

Yalnızca belirli bir sağlayıcı ve kategori için yapılandırma veya koddan kural uygulanmaz geçerli olan en düşük düzey ayar vardır. Aşağıdaki örnekte, en düşük düzeyin nasıl ayarlanacağı gösterilmektedir:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

En düşük düzeyi açıkça ayarlamazsanız, varsayılan değer olur `Information`ve `Debug` bu `Trace` , günlüklerin yok sayılacağı anlamına gelir.

### <a name="filter-functions"></a>Filtre işlevleri

Configuration veya Code tarafından kendisine atanmış kuralları olmayan tüm sağlayıcılar ve kategoriler için bir filtre işlevi çağırılır. İşlevindeki kodun sağlayıcı türü, kategorisi ve günlük düzeyine erişimi vardır. Örneğin:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Bazı günlük sağlayıcıları, günlüklerin bir depolama ortamına ne zaman yazılacağını veya günlük düzeyi ve kategorisine göre yoksayılacağını belirtmenize olanak tanır.

`AddConsole` Ve`AddDebug` genişletme yöntemleri, filtreleme ölçütlerini kabul eden aşırı yüklemeler sağlar. Aşağıdaki örnek kod, hata ayıklama sağlayıcısı Framework 'ün oluşturduğu günlükleri yok `Warning` sayırken, konsol sağlayıcısının düzeyi aşağıdaki günlükleri yoksaymasına neden olur.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Yönteminde bir `EventLogSettings` örnek alan bir aşırı yükleme vardır ve bu, `Filter` özelliğinde bir filtreleme işlevi içerebilir. `AddEventLog` Kendi günlük düzeyi ve diğer parametreleri temel aldığı ve `SourceSwitch` `TraceListener` kullandığı için, TraceSource sağlayıcısı bu aşırı yüklemelerin hiçbirini sağlamıyor.

Bir `ILoggerFactory` örnekle kayıtlı tüm sağlayıcıların filtreleme kurallarını ayarlamak için, `WithFilter` genişletme yöntemini kullanın. Aşağıdaki örnek, çerçeve günlüklerini kısıtlar (kategori, uygulama kodu tarafından oluşturulan Günlükler için hata ayıklama düzeyinde günlüğe yazılırken uyarılar için "Microsoft" veya "sistem" ile başlar).

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Tüm günlüklerin yazılmasını engellemek için, en düşük günlük `LogLevel.None` düzeyi olarak belirtin. Öğesinin `LogLevel.None` tamsayı değeri 6 ' dır `LogLevel.Critical` (5) daha yüksektir.

Genişletme yöntemi [Microsoft. Extensions. Logging. Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet paketi tarafından sağlanır. `WithFilter` Yöntemi, ile kaydedilen tüm `ILoggerFactory` günlükçü sağlayıcılarına geçirilen günlük iletilerini filtreleyecek yeni bir örnek döndürür. `ILoggerFactory` Özgün`ILoggerFactory` örnek dahil diğer örnekleri etkilemez.

::: moniker-end

## <a name="system-categories-and-levels"></a>Sistem kategorileri ve Düzeyler

ASP.NET Core ve Entity Framework Core tarafından kullanılan bazı kategoriler şunlardır ve bunlardan beklenen Günlükler hakkında notlar bulunur:

| Kategori                            | Notlar |
| ----------------------------------- | ----- |
| Microsoft. AspNetCore                | Genel ASP.NET Core tanılama. |
| Microsoft. AspNetCore. DataProtection | Hangi anahtarların kabul edildiği, bulunduğu ve kullanıldığı. |
| Microsoft. AspNetCore. HostFiltering  | İzin verilen konaklar. |
| Microsoft. AspNetCore. Hosting        | HTTP isteklerinin tamamlanması için geçen süre ve ne zaman başladıkları. Hangi barındırma başlangıç derlemeleri yüklendi. |
| Microsoft. AspNetCore. Mvc            | MVC ve Razor tanılama. Model bağlama, filtre yürütme, derlemeyi görüntüleme, eylem seçimi. |
| Microsoft. AspNetCore. Routing        | Eşleşen bilgileri yönlendirin. |
| Microsoft. AspNetCore. Server         | Bağlantı başlatın, durdurun ve canlı yanıtları koruyun. HTTPS sertifika bilgileri. |
| Microsoft. AspNetCore. StaticFiles    | Sunulan dosyalar. |
| Microsoft. EntityFrameworkCore       | Genel Entity Framework Core tanılama. Veritabanı etkinliği ve yapılandırması, değişiklik algılama, geçişler. |

## <a name="log-scopes"></a>Günlük kapsamları

 *Kapsam* bir mantıksal işlemler kümesini gruplandırabilir. Bu gruplandırma, kümenin bir parçası olarak oluşturulan her günlüğe aynı verileri eklemek için kullanılabilir. Örneğin, bir işlemin işlenmesi kapsamında oluşturulan her günlük işlem KIMLIĞI içerebilir.

Kapsam, `IDisposable` <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> yöntemi tarafından döndürülen ve atılana kadar sürer olan bir türdür. Bir `using` blok içinde günlükçü çağrılarını sarmalayarak kapsam kullanın:

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Aşağıdaki kod konsol sağlayıcısı için kapsamları etkinleştirilir:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Kapsam tabanlı günlüğe kaydetmeyi etkinleştirmek için konsolgünlükçüseçeneğininyapılandırılmasıgerekir.`IncludeScopes`
>
> Yapılandırma hakkında bilgi için [yapılandırma](#configuration) bölümüne bakın.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Kapsam tabanlı günlüğe kaydetmeyi etkinleştirmek için konsolgünlükçüseçeneğininyapılandırılmasıgerekir.`IncludeScopes`

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

## <a name="built-in-logging-providers"></a>Yerleşik günlük oluşturma sağlayıcıları

ASP.NET Core aşağıdaki sağlayıcıları sevk eder:

* [Console](#console-provider)
* [Hata ayıklama](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

Stdout ve ASP.NET Core modüllü hata ayıklama günlüğü hakkında bilgi için bkz <xref:test/troubleshoot-azure-iis> . ve. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>

### <a name="console-provider"></a>Konsol sağlayıcısı

[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcı paketi, günlük çıktısını konsola gönderir. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[Addconsole aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) , en az bir günlük düzeyi, bir filtre işlevi ve kapsamların desteklenip desteklenmediğini belirten bir Boole değeri geçirmenize olanak sağlar. Diğer bir seçenek de `IConfiguration` bir nesneyi geçirmektir ve bu da kapsamları destekler ve günlüğe kaydetme düzeylerini belirtebilir.

Konsol sağlayıcısı seçenekleri için bkz <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions>.

Konsol sağlayıcısının performansı önemli ölçüde etkiler ve genellikle üretimde kullanım için uygun değildir.

Visual Studio 'da yeni bir proje oluşturduğunuzda, `AddConsole` Yöntem şöyle görünür:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Bu kod, `Logging` *appSettings. JSON* dosyasının bölümüne başvurur:

[!code-json[](index/samples/1.x/TodoApiSample/appsettings.json)]

Gösterilen ayarlar, [günlük filtreleme](#log-filtering) bölümünde açıklandığı gibi uygulamanın hata ayıklama düzeyinde oturum açmasına izin verirken çerçeveyi uyarılarla günlüğe kaydeder. Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).

::: moniker-end

Konsol günlüğü çıkışını görmek için proje klasöründe bir komut istemi açın ve aşağıdaki komutu çalıştırın:

```console
dotnet run
```

### <a name="debug-provider"></a>Hata ayıklama sağlayıcısı

[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcı paketi, [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) sınıfını (`Debug.WriteLine` Yöntem çağrıları) kullanarak günlük çıktısını yazar.

Linux 'ta, bu sağlayıcı günlükleri */var/log/Message*dosyasına yazar.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

[Adddebug aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) , en az bir günlük düzeyinde veya bir filtre işlevinde geçiş yapmanızı sağlar.

::: moniker-end

### <a name="eventsource-provider"></a>EventSource sağlayıcı

ASP.NET Core 1.1.0 veya üzeri bir sürümü hedefleyen uygulamalar için, [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcı paketi olay izlemeyi uygulayabilir. Windows üzerinde [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)kullanır. Sağlayıcı platformlar arası, ancak Linux veya macOS için henüz bir olay koleksiyonu ve görüntüleme aracı yok.

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

Günlükleri toplamanın ve görüntülemenin iyi bir yolu, [PerfView yardımcı programını](https://github.com/Microsoft/perfview)kullanmaktır. ETW günlüklerini görüntülemeye yönelik başka araçlar da mevcuttur, ancak PerfView, ASP.NET tarafından yayınlanan ETW olaylarıyla çalışmak için en iyi deneyimi sağlar.

Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView 'ı yapılandırmak için, dizeyi `*Microsoft-Extensions-Logging` **ek sağlayıcılar** listesine ekleyin. (Dizenin başlangıcında yıldız işaretini kaçırmayın.)

![PerfView ek sağlayıcıları](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Windows olay günlüğü sağlayıcısı

[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcı paketi, Windows olay günlüğüne günlük çıktısı gönderir.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

[AddEventLog aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) , en az `EventLogSettings` bir günlük düzeyinde geçiş yapmanızı sağlar.

::: moniker-end

### <a name="tracesource-provider"></a>TraceSource sağlayıcısı

[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcı paketi <xref:System.Diagnostics.TraceSource> kitaplıklarını ve sağlayıcıları kullanır.

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

[Addtracesource aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) , bir kaynak anahtarı ve bir izleme dinleyicisi geçirmenize olanak sağlar.

Bu sağlayıcıyı kullanmak için, bir uygulamanın .NET Framework çalışması gerekir (.NET Core yerine). Sağlayıcı, iletileri örnek uygulamada <xref:System.Diagnostics.TextWriterTraceListener> kullanılan gibi çeşitli [dinleyicilerine](/dotnet/framework/debug-trace-profile/trace-listeners)yönlendirebilir.

::: moniker range="< aspnetcore-2.0"

Aşağıdaki örnek, konsol penceresine `TraceSource` ve daha yüksek `Warning` iletileri günlüğe kaydeden bir sağlayıcıyı yapılandırır.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a>Azure App Service sağlayıcı

[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcı paketi, günlükleri bir Azure App Service uygulamasının dosya sistemindeki metin dosyalarına ve bir Azure depolama hesabındaki [BLOB depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) alanına yazar. Sağlayıcı paketi, .NET Core 1,1 veya üstünü hedefleyen uygulamalar tarafından kullanılabilir.

::: moniker range=">= aspnetcore-2.0"

.NET Core hedefleniyorsa aşağıdaki noktalara göz önünde kalın:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Sağlayıcı paketi [Microsoft. AspNetCore. All metapackage](xref:fundamentals/metapackage)ASP.NET Core dahildir.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Sağlayıcı paketi [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e dahil değildir. Sağlayıcıyı kullanmak için paketini yükledikten sonra.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

.NET Framework veya `Microsoft.AspNetCore.App` metapackage 'e başvurabiliyorsa, sağlayıcı paketini projeye ekleyin. Çağır `AddAzureWebAppDiagnostics`:

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

Aşırı yükleme, <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>geçiş yapmanızı sağlar. <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> Ayarlar nesnesi, günlük çıkış şablonu, blob adı ve dosya boyutu sınırı gibi varsayılan ayarları geçersiz kılabilir. (*Çıktı şablonu* , bir `ILogger` yöntem çağrısıyla birlikte sağlandığının yanı sıra tüm günlüklere uygulanan bir ileti şablonudur.)

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Sağlayıcı ayarlarını yapılandırmak için aşağıdaki örnekte <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> gösterildiği <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>gibi ve kullanın:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

App Service uygulamasına dağıtırken, uygulama, Azure portal **App Service** sayfasının [App Service Günlükler](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) bölümündeki ayarları kabul eder. Aşağıdaki ayarlar güncelleştirilirken, değişiklikler uygulamanın yeniden başlatılmasını veya yeniden dağıtımı gerekmeden hemen etkili olur.

* **Uygulama günlüğü (dosya sistemi)**
* **Uygulama günlüğü (blob)**

Günlük dosyaları için varsayılan konum *D:\\Home\\LogFiles\\uygulama* klasöründedir ve varsayılan dosya adı *Diagnostics-YYYYMMDD. txt*' dir. Varsayılan dosya boyutu sınırı 10 MB 'tır ve tutulan varsayılan en fazla dosya sayısı 2 ' dir. Varsayılan blob adı *{app-name} {timestamp}/yyyy/mm/dd/ss/{Guid}-ApplicationLog.txt*.

Sağlayıcı yalnızca proje Azure ortamında çalıştırıldığında çalışır. Proje yerel olarak&mdash;çalıştırıldığında, yerel dosyalara veya blob 'lar için yerel geliştirme depolamasına yazmazsa, bu, hiçbir etkiye sahip değildir.

#### <a name="azure-log-streaming"></a>Azure günlük akışı

Azure günlük akışı, günlük etkinliklerini gerçek zamanlı olarak görüntülemenize izin verir:

* Uygulama sunucusu
* Web sunucusu
* Başarısız istek izleme

Azure günlük akışını yapılandırmak için:

* Uygulamanızın Portal sayfasından **App Service günlükleri** sayfasına gidin.
* **Uygulama günlüğünü (FileSystem)** **Açık**olarak ayarlayın.
* Günlük **düzeyini**seçin.

Uygulama iletilerini görüntülemek için **günlük akışı** sayfasına gidin. Uygulama tarafından `ILogger` arabirim aracılığıyla günlüğe kaydedilir.

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Azure Application Insights izleme günlüğü

[Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) sağlayıcı paketi günlükleri Azure Application Insights yazar. Application Insights, bir Web uygulamasını izleyen ve telemetri verilerini sorgulamak ve analiz etmek için araçlar sağlayan bir hizmettir. Bu sağlayıcıyı kullanıyorsanız, Application Insights araçlarını kullanarak günlüklerinizi sorgulayabilir ve analiz edebilirsiniz.

Günlüğe kaydetme sağlayıcısı, ASP.NET Core için tüm kullanılabilir telemetri sağlayan paket olan [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore)'un bağımlılığı olarak eklenmiştir. Bu paketi kullanırsanız, sağlayıcı paketini yüklemek zorunda kalmazsınız.

ASP.NET 4. x için [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) paketini&mdash;kullanmayın.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Application Insights genel bakış](/azure/application-insights/app-insights-overview)
* [ASP.NET Core uygulamalar için Application Insights](/azure/azure-monitor/app/asp-net-core-no-visualstudio) -günlük kaydı ile birlikte Application Insights telemetrinin tam aralığını uygulamak istiyorsanız buraya başlayın.
* [.NET Core ILogger günlükleri Için Applicationınsightsloggerprovider](/azure/azure-monitor/app/ilogger) -günlük sağlayıcısını Application Insights telemetri olmadan uygulamak istiyorsanız buraya başlayın.
* [Günlüğe kaydetme bağdaştırıcılarını Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).
* Microsoft Learn sitede Application Insights SDK-etkileşimli öğreticisini [yükleyip başlatın](/learn/modules/instrument-web-app-code-with-application-insights) .
::: moniker-end

> [!NOTE]
> 5/1/2019 itibariyle, [ASP.NET Core için Application Insights](/azure/azure-monitor/app/asp-net-core) başlıklı makale güncel değildir ve öğretici adımları çalışmaz. Bunun yerine [ASP.NET Core uygulamalar için Application Insights](/azure/azure-monitor/app/asp-net-core-no-visualstudio) bakın. Sorunun farkındayız ve düzeltmek için çalışıyoruz.

## <a name="third-party-logging-providers"></a>Üçüncü taraf günlük oluşturma sağlayıcıları

ASP.NET Core ile birlikte çalışan üçüncü taraf günlük çerçeveleri:

* [ELMAH.io](https://elmah.io/) ([GitHub deposu](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposu](https://github.com/mattwcole/gelf-extensions-logging))
* [Jsnlog](https://jsnlog.com/) ([GitHub deposu](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub deposu](https://github.com/catalingavan/KissLog-net))
* [Loggr](https://loggr.net/) ([GitHub deposu](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([GitHub deposu](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([GitHub deposu](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub deposu](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub deposu](https://github.com/googleapis/google-cloud-dotnet))

Bazı üçüncü taraf çerçeveler [, yapılandırılmış günlük olarak da bilinen anlam günlüğe kaydetme](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)işlemini gerçekleştirebilir.

Bir üçüncü taraf çerçevesinin kullanılması, yerleşik sağlayıcılardan birini kullanmaya benzer:

1. Projenize bir NuGet paketi ekleyin.
1. Bir `ILoggerFactory`çağırın.

Daha fazla bilgi için bkz. her sağlayıcının belgeleri. Üçüncü taraf günlüğü sağlayıcıları Microsoft tarafından desteklenmez.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/logging/loggermessage>
