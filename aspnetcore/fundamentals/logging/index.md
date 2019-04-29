---
title: ASP.NET core'da günlüğe kaydetme
author: tdykstra
description: ASP.NET core'da günlüğe kaydetme çerçevesi hakkında bilgi edinin. Yerleşik günlük sağlayıcıları bulmak ve popüler üçüncü taraf sağlayıcılar hakkında daha fazla bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 8a2e310b47e32e9015b0c127ed79d8f6bdf2e44d
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982859"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="178bf-104">ASP.NET core'da günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="178bf-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="178bf-105">Tarafından [Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="178bf-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="178bf-106">ASP.NET Core çeşitli günlük yerleşik ve üçüncü taraf sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler.</span><span class="sxs-lookup"><span data-stu-id="178bf-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="178bf-107">Bu makalede, yerleşik sağlayıcılar ile günlüğe kaydetme API'si kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="178bf-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="178bf-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="178bf-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="178bf-109">Sağlayıcıları Ekle</span><span class="sxs-lookup"><span data-stu-id="178bf-109">Add providers</span></span>

<span data-ttu-id="178bf-110">Oturum açma sağlayıcısı görüntüler veya günlüklerinde depolar.</span><span class="sxs-lookup"><span data-stu-id="178bf-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="178bf-111">Örneğin, konsolu sağlayıcısı günlükleri konsolda görüntüler ve Azure Application Insights sağlayıcısı bunları Azure Application Insights içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="178bf-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="178bf-112">Günlükleri, birden çok sağlayıcı ekleyerek, birden çok hedefe gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="178bf-113">Bir sağlayıcı eklemek için sağlayıcının çağrı `Add{provider name}` uzantı yönteminde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="178bf-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="178bf-114">Varsayılan proje şablonu çağrılarının <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, aşağıdaki günlük kaydı sağlayıcıları ekler:</span><span class="sxs-lookup"><span data-stu-id="178bf-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="178bf-115">Konsol</span><span class="sxs-lookup"><span data-stu-id="178bf-115">Console</span></span>
* <span data-ttu-id="178bf-116">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-116">Debug</span></span>
* <span data-ttu-id="178bf-117">EventSource (ASP.NET Core 2.2 içinde başlayarak)</span><span class="sxs-lookup"><span data-stu-id="178bf-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="178bf-118">Kullanırsanız `CreateDefaultBuilder`, varsayılan sağlayıcıları kendi seçtiklerinizle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="178bf-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="178bf-119">Çağrı <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, istediğiniz sağlayıcıları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="178bf-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="178bf-120">Bir sağlayıcı kullanmak için kendi NuGet paketini yükleyin ve bir örneği üzerinde sağlayıcının uzantı metodu çağırma <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span><span class="sxs-lookup"><span data-stu-id="178bf-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="178bf-121">ASP.NET Core [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) sağlar `ILoggerFactory` örneği.</span><span class="sxs-lookup"><span data-stu-id="178bf-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="178bf-122">`AddConsole` Ve `AddDebug` genişletme yöntemleri tanımlanmış [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) ve [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paketleri.</span><span class="sxs-lookup"><span data-stu-id="178bf-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="178bf-123">Her bir genişletme yöntemi çağıran `ILoggerFactory.AddProvider` sağlayıcının bir örneğini geçirerek yöntemi.</span><span class="sxs-lookup"><span data-stu-id="178bf-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="178bf-124">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) günlük sağlayıcıları ekler `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="178bf-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="178bf-125">Daha önce yürüten koddan günlük çıktısını almak için günlüğe kaydetme hizmeti sağlayıcıları Ekle `Startup` sınıf oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="178bf-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="178bf-126">Daha fazla bilgi edinin [yerleşik günlük sağlayıcıları](#built-in-logging-providers) ve [üçüncü taraf günlük sağlayıcıları](#third-party-logging-providers) makalenin ilerleyen bölümlerinde.</span><span class="sxs-lookup"><span data-stu-id="178bf-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="178bf-127">Günlükleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="178bf-127">Create logs</span></span>

<span data-ttu-id="178bf-128">Alma bir <xref:Microsoft.Extensions.Logging.ILogger%601> DI nesne.</span><span class="sxs-lookup"><span data-stu-id="178bf-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="178bf-129">Aşağıdaki denetleyici örneği oluşturur `Information` ve `Warning` günlükleri.</span><span class="sxs-lookup"><span data-stu-id="178bf-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="178bf-130">*Kategori* olduğu `TodoApiSample.Controllers.TodoController` (tam sınıf adını `TodoController` örnek uygulamada):</span><span class="sxs-lookup"><span data-stu-id="178bf-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="178bf-131">Aşağıdaki Razor sayfaları örnek günlükleriyle oluşturur `Information` olarak *düzeyi* ve `TodoApiSample.Pages.AboutModel` olarak *kategori*:</span><span class="sxs-lookup"><span data-stu-id="178bf-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="178bf-132">Yukarıdaki örnekte günlükleriyle oluşturur `Information` ve `Warning` olarak *düzeyi* ve `TodoController` olarak sınıf *kategori*.</span><span class="sxs-lookup"><span data-stu-id="178bf-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="178bf-133">Günlük *düzeyi* günlüğe kaydedilen olayın önem derecesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="178bf-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="178bf-134">Günlük *kategori* her bir günlük ilişkilendirilmiş bir dizedir.</span><span class="sxs-lookup"><span data-stu-id="178bf-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="178bf-135">`ILogger<T>` Örneği oluşturur türünde tam olarak nitelenmiş ada sahip günlükleri `T` kategori olarak.</span><span class="sxs-lookup"><span data-stu-id="178bf-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="178bf-136">[Düzeyleri](#log-level) ve [kategorileri](#log-category) bu makalenin ilerleyen bölümlerinde daha ayrıntılı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="178bf-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="178bf-137">Başlangıç günlükleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="178bf-137">Create logs in Startup</span></span>

<span data-ttu-id="178bf-138">Günlükleri yazmak için `Startup` sınıfı, içeren bir `ILogger` parametresinde Oluşturucu imzası:</span><span class="sxs-lookup"><span data-stu-id="178bf-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="178bf-139">Günlükleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="178bf-139">Create logs in Program</span></span>

<span data-ttu-id="178bf-140">Günlükleri yazmak için `Program` sınıfı, Al bir `ILogger` örneğinden dı:</span><span class="sxs-lookup"><span data-stu-id="178bf-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="178bf-141">Hiçbir zaman uyumsuz Günlükçü yöntemi</span><span class="sxs-lookup"><span data-stu-id="178bf-141">No asynchronous logger methods</span></span>

<span data-ttu-id="178bf-142">Günlük hızlı şekilde zaman uyumsuz kodun performans maliyeti karşılıyor olmadığından emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="178bf-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="178bf-143">Günlük veri deponuz yavaşsa, kendisine doğrudan yazmayın.</span><span class="sxs-lookup"><span data-stu-id="178bf-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="178bf-144">Günlük iletilerini başlangıçta hızlı bir depoya yazmayı düşünebilirsiniz, sonra bunları daha sonra yavaş deposuna taşıyın.</span><span class="sxs-lookup"><span data-stu-id="178bf-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="178bf-145">Örneğin, SQL Server oturum açtığınızdan, doğrudan Bunu yapmak istemediğiniz bir `Log` yöntemi, bu yana `Log` yöntemleri zaman uyumludur.</span><span class="sxs-lookup"><span data-stu-id="178bf-145">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="178bf-146">Bunun yerine, zaman uyumlu bir bellek içi kuyruğuna günlük iletilerini ve SQL Server veri gönderme, zaman uyumsuz işi yapmak için sıradaki iletilerin çekme arka plan çalışanı vardır.</span><span class="sxs-lookup"><span data-stu-id="178bf-146">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="178bf-147">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="178bf-147">Configuration</span></span>

<span data-ttu-id="178bf-148">Sağlayıcı Yapılandırması günlüğe kaydetme, bir veya daha fazla yapılandırma sağlayıcıları tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="178bf-148">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="178bf-149">Dosya biçimleri (INI, JSON ve XML).</span><span class="sxs-lookup"><span data-stu-id="178bf-149">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="178bf-150">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="178bf-150">Command-line arguments.</span></span>
* <span data-ttu-id="178bf-151">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="178bf-151">Environment variables.</span></span>
* <span data-ttu-id="178bf-152">Bellek içi .NET nesneleri.</span><span class="sxs-lookup"><span data-stu-id="178bf-152">In-memory .NET objects.</span></span>
* <span data-ttu-id="178bf-153">Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolama.</span><span class="sxs-lookup"><span data-stu-id="178bf-153">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="178bf-154">Gibi bir şifrelenmiş kullanıcı depolamak [Azure anahtar kasası](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="178bf-154">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="178bf-155">Özel sağlayıcılar (veya oluşturulan yüklü).</span><span class="sxs-lookup"><span data-stu-id="178bf-155">Custom providers (installed or created).</span></span>

<span data-ttu-id="178bf-156">Örneğin, günlük kaydı yapılandırması sık tarafından sağlanan `Logging` uygulama ayarları dosyaları bölümü.</span><span class="sxs-lookup"><span data-stu-id="178bf-156">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="178bf-157">Aşağıdaki örnek, tipik bir içeriğini gösterir *appsettings. Development.JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="178bf-157">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="178bf-158">`Logging` Özelliği olabilir `LogLevel` ve günlük sağlayıcısı özellikleri (konsol gösterilmiştir).</span><span class="sxs-lookup"><span data-stu-id="178bf-158">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="178bf-159">`LogLevel` Özelliği altında `Logging` en düşük belirtir [düzeyi](#log-level) için seçili kategorilerdeki oturum.</span><span class="sxs-lookup"><span data-stu-id="178bf-159">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="178bf-160">Örnekte, `System` ve `Microsoft` kategorileri oturum sırasında `Information` düzeyi ve diğer tüm oturum sırasında `Debug` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="178bf-160">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="178bf-161">Diğer özellikleri altında `Logging` günlük sağlayıcılarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="178bf-161">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="178bf-162">Örnek için konsol sağlayıcısıdır.</span><span class="sxs-lookup"><span data-stu-id="178bf-162">The example is for the Console provider.</span></span> <span data-ttu-id="178bf-163">Bir sağlayıcı destekliyorsa [oturum kapsamları](#log-scopes), `IncludeScopes` ayarların etkinleştirildiğinden olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="178bf-163">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="178bf-164">Sağlayıcı özelliği (gibi `Console` örnekte) ayrıca bir `LogLevel` özelliği.</span><span class="sxs-lookup"><span data-stu-id="178bf-164">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="178bf-165">`LogLevel` bir sağlayıcı altında bu sağlayıcı için oturum düzeyleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="178bf-165">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="178bf-166">Düzeyleri belirtilirse `Logging.{providername}.LogLevel`, herhangi bir şey kümesinde geçersiz kılma `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="178bf-166">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

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

<span data-ttu-id="178bf-167">`LogLevel` anahtarları günlük adlarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="178bf-167">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="178bf-168">`Default` Anahtarı açıkça listelenen günlükler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="178bf-168">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="178bf-169">Değeri temsil [günlük düzeyi](#log-level) verilen günlüğe uygulanır.</span><span class="sxs-lookup"><span data-stu-id="178bf-169">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="178bf-170">Uygulama yapılandırma sağlayıcıları hakkında daha fazla bilgi için bkz: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="178bf-170">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="178bf-171">Örnek günlük çıktısı</span><span class="sxs-lookup"><span data-stu-id="178bf-171">Sample logging output</span></span>

<span data-ttu-id="178bf-172">Komut satırından uygulama çalıştırıldığında önceki bölümde gösterilen örnek kod ile günlükleri konsolda görünür.</span><span class="sxs-lookup"><span data-stu-id="178bf-172">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="178bf-173">Konsol çıktının bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="178bf-173">Here's an example of console output:</span></span>

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

<span data-ttu-id="178bf-174">Önceki günlükleri konumundaki örnek uygulamaya bir HTTP Get isteği yapılarak oluşturulan `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="178bf-174">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="178bf-175">Visual Studio'da örnek uygulamayı çalıştırdığınızda hata ayıklama penceresinde göründükleri gibi aynı günlüklerinin bir örnek aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="178bf-175">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="178bf-176">Tarafından oluşturulan günlükleri `ILogger` çağrıları önceki bölümde gösterilen "TodoApi.Controllers.TodoController" ile başlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-176">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="178bf-177">"Microsoft" Kategoriler ile başlayan ASP.NET Core framework koddan günlüklerdir.</span><span class="sxs-lookup"><span data-stu-id="178bf-177">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="178bf-178">ASP.NET Core ve uygulama kodu aynı günlüğe kaydetme API'si ve sağlayıcıları kullanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="178bf-178">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="178bf-179">Bu makalenin geri kalanında, bazı ayrıntılar ve günlüğe kaydetme seçeneklerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="178bf-179">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="178bf-180">NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="178bf-180">NuGet packages</span></span>

<span data-ttu-id="178bf-181">`ILogger` Ve `ILoggerFactory` arabirimdir içinde [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ve bunlar için varsayılan uygulamalarıdır içinde [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="178bf-181">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="178bf-182">Günlük kategorisi</span><span class="sxs-lookup"><span data-stu-id="178bf-182">Log category</span></span>

<span data-ttu-id="178bf-183">Olduğunda bir `ILogger` nesnesi oluşturulur, bir *kategori* için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-183">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="178bf-184">Bu örneği tarafından oluşturulan her günlük iletisi kategori dahildir `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="178bf-184">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="178bf-185">Kategori herhangi bir dize olabilir, ancak kuralı, "TodoApi.Controllers.TodoController" gibi bir sınıf adı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="178bf-185">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="178bf-186">Kullanım `ILogger<T>` almak için bir `ILogger` tam olarak nitelenmiş tür adını kullanan örnek `T` kategori olarak:</span><span class="sxs-lookup"><span data-stu-id="178bf-186">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="178bf-187">Kategori açıkça belirtmek için çağrı `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="178bf-187">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="178bf-188">`ILogger<T>` çağırmakla eşdeğerdir `CreateLogger` tam olarak nitelenmiş tür adını `T`.</span><span class="sxs-lookup"><span data-stu-id="178bf-188">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="178bf-189">Günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="178bf-189">Log level</span></span>

<span data-ttu-id="178bf-190">Her günlük belirtir bir <xref:Microsoft.Extensions.Logging.LogLevel> değeri.</span><span class="sxs-lookup"><span data-stu-id="178bf-190">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="178bf-191">Önem derecesi veya önem günlük düzeyini belirtir.</span><span class="sxs-lookup"><span data-stu-id="178bf-191">The log level indicates the severity or importance.</span></span> <span data-ttu-id="178bf-192">Örneğin, yazabilirsiniz bir `Information` normalde bir yöntem sona erdiğinde ve günlük `Warning` oturum bir yöntem döndürüldüğünde bir *404 Bulunamadı* durum kodu.</span><span class="sxs-lookup"><span data-stu-id="178bf-192">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="178bf-193">Aşağıdaki kod oluşturur `Information` ve `Warning` günlükleri:</span><span class="sxs-lookup"><span data-stu-id="178bf-193">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="178bf-194">Önceki kodda, ilk parametredir [oturum öğesini belirten Olay No.](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="178bf-194">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="178bf-195">İkinci parametre, kalan yöntem parametreleri tarafından sağlanan bağımsız değişken değerleri yer tutucuları olan bir ileti şablonudur.</span><span class="sxs-lookup"><span data-stu-id="178bf-195">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="178bf-196">Yöntem parametreleri açıklandığı [ileti şablon bölümü](#log-message-template) bu makalenin ilerleyen bölümlerinde.</span><span class="sxs-lookup"><span data-stu-id="178bf-196">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="178bf-197">Günlük düzeyi yöntem adı'içeren yöntemleri (örneğin, `LogInformation` ve `LogWarning`) olan [için ILogger genişletme yöntemleri](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="178bf-197">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="178bf-198">Bu yöntemleri çağırmak bir `Log` gereken yöntemini bir `LogLevel` parametresi.</span><span class="sxs-lookup"><span data-stu-id="178bf-198">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="178bf-199">Çağırabilirsiniz `Log` biri bu genişletme yöntemleri, ancak söz dizimi yerine doğrudan yöntemi nispeten karmaşık.</span><span class="sxs-lookup"><span data-stu-id="178bf-199">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="178bf-200">Daha fazla bilgi için <xref:Microsoft.Extensions.Logging.ILogger> ve [Günlükçü uzantılarını kaynak kodu](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="178bf-200">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="178bf-201">ASP.NET Core burada en düşük en üst derecesi sıralı aşağıdaki günlük düzeyleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-201">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="178bf-202">İzleme = 0</span><span class="sxs-lookup"><span data-stu-id="178bf-202">Trace = 0</span></span>

  <span data-ttu-id="178bf-203">Yalnızca hata ayıklama için genellikle değerli bilgiler.</span><span class="sxs-lookup"><span data-stu-id="178bf-203">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="178bf-204">Bu iletiler, uygulama hassas verileri içerebilir ve bir üretim ortamında bu nedenle etkin olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="178bf-204">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="178bf-205">*Varsayılan olarak devre dışıdır.*</span><span class="sxs-lookup"><span data-stu-id="178bf-205">*Disabled by default.*</span></span>

* <span data-ttu-id="178bf-206">Hata ayıklama = 1</span><span class="sxs-lookup"><span data-stu-id="178bf-206">Debug = 1</span></span>

  <span data-ttu-id="178bf-207">Bilgi için geliştirme ve hata ayıklama için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-207">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="178bf-208">Örnek: `Entering method Configure with flag set to true.` Etkinleştirme `Debug` düzeyi yalnızca, yüksek hacimli günlükleri nedeniyle sorun giderme sırasında üretimde günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="178bf-208">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="178bf-209">Bilgi = 2</span><span class="sxs-lookup"><span data-stu-id="178bf-209">Information = 2</span></span>

  <span data-ttu-id="178bf-210">Uygulama'nın genel akışı izlemek için.</span><span class="sxs-lookup"><span data-stu-id="178bf-210">For tracking the general flow of the app.</span></span> <span data-ttu-id="178bf-211">Bu günlükler genellikle uzun vadeli bir değer var.</span><span class="sxs-lookup"><span data-stu-id="178bf-211">These logs typically have some long-term value.</span></span> <span data-ttu-id="178bf-212">Örnek: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="178bf-212">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="178bf-213">Uyarı = 3</span><span class="sxs-lookup"><span data-stu-id="178bf-213">Warning = 3</span></span>

  <span data-ttu-id="178bf-214">Olağan dışı ya da beklenmeyen olaylarını için uygulama akışı.</span><span class="sxs-lookup"><span data-stu-id="178bf-214">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="178bf-215">Bunlar, hatalar veya durdurmak uygulamayı neden olmaz ancak incelenmesi gerekebilecek diğer koşulları olabilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-215">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="178bf-216">İşlenmiş istisnaları kullanmak için ortak bir yerde `Warning` günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="178bf-216">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="178bf-217">Örnek: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="178bf-217">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="178bf-218">Hata = 4</span><span class="sxs-lookup"><span data-stu-id="178bf-218">Error = 4</span></span>

  <span data-ttu-id="178bf-219">Hatalar ve özel durumlar için işlenemez.</span><span class="sxs-lookup"><span data-stu-id="178bf-219">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="178bf-220">Bu iletiler, geçerli etkinliği ya da (örneğin, geçerli HTTP isteği) işlemi bir hata, uygulama genelinde hata gösterir.</span><span class="sxs-lookup"><span data-stu-id="178bf-220">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="178bf-221">Örnek günlük iletisi: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="178bf-221">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="178bf-222">Kritik = 5</span><span class="sxs-lookup"><span data-stu-id="178bf-222">Critical = 5</span></span>

  <span data-ttu-id="178bf-223">Hemen ilgilenilmesi gereken hataları.</span><span class="sxs-lookup"><span data-stu-id="178bf-223">For failures that require immediate attention.</span></span> <span data-ttu-id="178bf-224">Örnekler: veri kaybı senaryolarına, disk alanı yetersiz.</span><span class="sxs-lookup"><span data-stu-id="178bf-224">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="178bf-225">Ne kadar günlük çıktısını bir belirli depolama ortamına yazılır denetlemek için günlük düzeyi kullanın veya penceresini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="178bf-225">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="178bf-226">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="178bf-226">For example:</span></span>

* <span data-ttu-id="178bf-227">Üretim ortamında, gönderme `Trace` aracılığıyla `Information` toplu veri deposuna düzeyi.</span><span class="sxs-lookup"><span data-stu-id="178bf-227">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="178bf-228">Gönderme `Warning` aracılığıyla `Critical` için bir değer veri deposu.</span><span class="sxs-lookup"><span data-stu-id="178bf-228">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="178bf-229">Geliştirme sırasında Gönder `Warning` aracılığıyla `Critical` konsola ekleyin `Trace` aracılığıyla `Information` sorunlarını giderirken.</span><span class="sxs-lookup"><span data-stu-id="178bf-229">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="178bf-230">[Günlük filtreleme](#log-filtering) bu makalenin devamındaki bölümüne bir sağlayıcı işleme hangi günlük düzeyleri denetlemek nasıl açıklar.</span><span class="sxs-lookup"><span data-stu-id="178bf-230">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="178bf-231">ASP.NET Core framework olayları için günlükler yazar.</span><span class="sxs-lookup"><span data-stu-id="178bf-231">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="178bf-232">Günlük örnekleri, bu makalede daha önce günlüklere hariç tutuldu. `Information` düzeyi, dolayısıyla `Debug` veya `Trace` düzeyi günlükleri oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="178bf-232">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="178bf-233">İşte bir örnek konsol günlüklerinin göstermek için örnek uygulamayı çalıştırarak üretilmiş `Debug` günlükleri:</span><span class="sxs-lookup"><span data-stu-id="178bf-233">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="178bf-234">Günlük Olay Kimliği</span><span class="sxs-lookup"><span data-stu-id="178bf-234">Log event ID</span></span>

<span data-ttu-id="178bf-235">Her günlük belirtebilirsiniz bir *öğesini belirten Olay No*.</span><span class="sxs-lookup"><span data-stu-id="178bf-235">Each log can specify an *event ID*.</span></span> <span data-ttu-id="178bf-236">Örnek uygulamayı yerel olarak tanımlanan kullanarak bunu yapar `LoggingEvents` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="178bf-236">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="178bf-237">Olay Kimliği bir etkinlik kümesi ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="178bf-237">An event ID associates a set of events.</span></span> <span data-ttu-id="178bf-238">Örneğin, bir sayfada öğelerinin listesini görüntülemek için ilgili tüm günlükler 1001 olabilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-238">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="178bf-239">Oturum açma sağlayıcısı, bir Kimliği alanında günlük iletisi ya da hiç olay kimliği depolayabilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-239">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="178bf-240">Hata ayıklama sağlayıcısı olay kimlikleri göstermez.</span><span class="sxs-lookup"><span data-stu-id="178bf-240">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="178bf-241">Konsolu sağlayıcısı olay kimlikleri, köşeli ayraç içinde kategori sonra gösterir:</span><span class="sxs-lookup"><span data-stu-id="178bf-241">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="178bf-242">Günlük ileti şablonu</span><span class="sxs-lookup"><span data-stu-id="178bf-242">Log message template</span></span>

<span data-ttu-id="178bf-243">Her günlük iletisi şablonu belirtir.</span><span class="sxs-lookup"><span data-stu-id="178bf-243">Each log specifies a message template.</span></span> <span data-ttu-id="178bf-244">İleti şablonunu argüman sağlanmıyorsa yer tutucuları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-244">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="178bf-245">Sayılar değil yer tutucu adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="178bf-245">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="178bf-246">Hangi parametrelerin değerlerini sağlamak için kullanılan yer tutucuları, bunların adları sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="178bf-246">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="178bf-247">Aşağıdaki kodda, parametre adları ileti şablonunu sırayla dışında olduğuna dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="178bf-247">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="178bf-248">Bu kod, parametre değerleriniz ile sırada bir günlük iletisi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="178bf-248">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="178bf-249">Günlüğe kaydetme çerçevesi, bu şekilde çalışır, böylece günlük sağlayıcıları uygulayabilirsiniz [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="178bf-249">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="178bf-250">Bağımsız değişkenleri kendilerini yalnızca biçimlendirilmiş ileti şablonunu, günlüğe kaydetme sistemi geçirilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-250">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="178bf-251">Günlüğe kaydetme sağlayıcıları alanları olarak parametre değerlerini depolamak bu bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-251">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="178bf-252">Örneğin, Günlükçü yöntemi çağrıları görünüm şöyle düşünün:</span><span class="sxs-lookup"><span data-stu-id="178bf-252">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="178bf-253">Azure tablo depolaması için günlükleri gönderiyorsanız, her Azure Tablo varlığı olabilir `ID` ve `RequestTime` sorgu günlüğü verilerini kolaylaştıran özellikler.</span><span class="sxs-lookup"><span data-stu-id="178bf-253">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="178bf-254">Bir sorgu içinde belirli bir tüm günlükler bulabilirsiniz `RequestTime` olmadan ayrıştırma, kısa mesaj zaman aşımı aralığı.</span><span class="sxs-lookup"><span data-stu-id="178bf-254">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="178bf-255">Özel durumları</span><span class="sxs-lookup"><span data-stu-id="178bf-255">Logging exceptions</span></span>

<span data-ttu-id="178bf-256">Günlükçü yöntemleri aşağıdaki örnekteki gibi bir özel durum geçirmenize olanak tanıyan aşırı yüklemeleri vardır:</span><span class="sxs-lookup"><span data-stu-id="178bf-256">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="178bf-257">Farklı sağlayıcıları, özel durum bilgilerini farklı yollarla işler.</span><span class="sxs-lookup"><span data-stu-id="178bf-257">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="178bf-258">Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktının bir örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="178bf-258">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="178bf-259">Günlük Filtresi</span><span class="sxs-lookup"><span data-stu-id="178bf-259">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="178bf-260">En düşük günlük düzeyi veya tüm sağlayıcıları ya da tüm kategorileri özel sağlayıcı ve kategori için belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="178bf-260">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="178bf-261">Bunlar görüntülenen veya depolanan olmayan şekilde en düşük düzeyin herhangi bir günlük bu sağlayıcısına geçirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="178bf-261">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="178bf-262">Tüm günlükler bastırmak için belirtin `LogLevel.None` en düşük günlük düzeyi olarak.</span><span class="sxs-lookup"><span data-stu-id="178bf-262">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="178bf-263">Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="178bf-263">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="178bf-264">Yapılandırma filtresi kuralları oluşturabilir</span><span class="sxs-lookup"><span data-stu-id="178bf-264">Create filter rules in configuration</span></span>

<span data-ttu-id="178bf-265">Proje şablonu kod çağrıları `CreateDefaultBuilder` konsol ve hata ayıklama sağlayıcıları için günlük kaydı ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="178bf-265">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="178bf-266">`CreateDefaultBuilder` Yöntemi ayrıca ayarlar günlüğünü yapılandırmasında aramak için bir `Logging` bölümünde, aşağıdaki gibi kod kullanarak:</span><span class="sxs-lookup"><span data-stu-id="178bf-266">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="178bf-267">Yapılandırma verileri sağlayıcısı ve kategorisi, aşağıdaki örnekte olduğu gibi en az günlük düzeyleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="178bf-267">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="178bf-268">Bu JSON altı bir filtre kuralları oluşturur: bir hata ayıklama sağlayıcısı, dört Konsolu sağlayıcısı için ve biri tüm sağlayıcılar için.</span><span class="sxs-lookup"><span data-stu-id="178bf-268">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="178bf-269">Tek bir kural her sağlayıcısı için seçilen zaman bir `ILogger` nesnesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="178bf-269">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="178bf-270">Kod içinde filtre kuralları</span><span class="sxs-lookup"><span data-stu-id="178bf-270">Filter rules in code</span></span>

<span data-ttu-id="178bf-271">Aşağıdaki örnek, kod içinde filtre kuralları kaydedebilir gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="178bf-271">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="178bf-272">İkinci `AddFilter` , tür adını kullanarak hata ayıklama sağlayıcıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="178bf-272">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="178bf-273">İlk `AddFilter` bir sağlayıcı türü belirtmeyen çünkü tüm sağlayıcılar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="178bf-273">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="178bf-274">Nasıl filtreleme kuralları uygulanır</span><span class="sxs-lookup"><span data-stu-id="178bf-274">How filtering rules are applied</span></span>

<span data-ttu-id="178bf-275">Yapılandırma verilerini ve `AddFilter` Yukarıdaki örneklerde gösterilen kod aşağıdaki tabloda gösterilen kuralları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="178bf-275">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="178bf-276">İlk altı yapılandırma örnekten gelmesi ve son iki kod örneğindeki gelir.</span><span class="sxs-lookup"><span data-stu-id="178bf-276">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="178bf-277">Sayı</span><span class="sxs-lookup"><span data-stu-id="178bf-277">Number</span></span> | <span data-ttu-id="178bf-278">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="178bf-278">Provider</span></span>      | <span data-ttu-id="178bf-279">Şununla kategori...</span><span class="sxs-lookup"><span data-stu-id="178bf-279">Categories that begin with ...</span></span>          | <span data-ttu-id="178bf-280">En düşük günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="178bf-280">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="178bf-281">1.</span><span class="sxs-lookup"><span data-stu-id="178bf-281">1</span></span>      | <span data-ttu-id="178bf-282">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-282">Debug</span></span>         | <span data-ttu-id="178bf-283">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="178bf-283">All categories</span></span>                          | <span data-ttu-id="178bf-284">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="178bf-284">Information</span></span>       |
| <span data-ttu-id="178bf-285">2</span><span class="sxs-lookup"><span data-stu-id="178bf-285">2</span></span>      | <span data-ttu-id="178bf-286">Konsol</span><span class="sxs-lookup"><span data-stu-id="178bf-286">Console</span></span>       | <span data-ttu-id="178bf-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="178bf-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="178bf-288">Uyarı</span><span class="sxs-lookup"><span data-stu-id="178bf-288">Warning</span></span>           |
| <span data-ttu-id="178bf-289">3</span><span class="sxs-lookup"><span data-stu-id="178bf-289">3</span></span>      | <span data-ttu-id="178bf-290">Konsol</span><span class="sxs-lookup"><span data-stu-id="178bf-290">Console</span></span>       | <span data-ttu-id="178bf-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="178bf-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="178bf-292">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-292">Debug</span></span>             |
| <span data-ttu-id="178bf-293">4</span><span class="sxs-lookup"><span data-stu-id="178bf-293">4</span></span>      | <span data-ttu-id="178bf-294">Konsol</span><span class="sxs-lookup"><span data-stu-id="178bf-294">Console</span></span>       | <span data-ttu-id="178bf-295">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="178bf-295">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="178bf-296">Hata</span><span class="sxs-lookup"><span data-stu-id="178bf-296">Error</span></span>             |
| <span data-ttu-id="178bf-297">5</span><span class="sxs-lookup"><span data-stu-id="178bf-297">5</span></span>      | <span data-ttu-id="178bf-298">Konsol</span><span class="sxs-lookup"><span data-stu-id="178bf-298">Console</span></span>       | <span data-ttu-id="178bf-299">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="178bf-299">All categories</span></span>                          | <span data-ttu-id="178bf-300">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="178bf-300">Information</span></span>       |
| <span data-ttu-id="178bf-301">6</span><span class="sxs-lookup"><span data-stu-id="178bf-301">6</span></span>      | <span data-ttu-id="178bf-302">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="178bf-302">All providers</span></span> | <span data-ttu-id="178bf-303">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="178bf-303">All categories</span></span>                          | <span data-ttu-id="178bf-304">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-304">Debug</span></span>             |
| <span data-ttu-id="178bf-305">7</span><span class="sxs-lookup"><span data-stu-id="178bf-305">7</span></span>      | <span data-ttu-id="178bf-306">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="178bf-306">All providers</span></span> | <span data-ttu-id="178bf-307">Sistem</span><span class="sxs-lookup"><span data-stu-id="178bf-307">System</span></span>                                  | <span data-ttu-id="178bf-308">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-308">Debug</span></span>             |
| <span data-ttu-id="178bf-309">8</span><span class="sxs-lookup"><span data-stu-id="178bf-309">8</span></span>      | <span data-ttu-id="178bf-310">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-310">Debug</span></span>         | <span data-ttu-id="178bf-311">Microsoft</span><span class="sxs-lookup"><span data-stu-id="178bf-311">Microsoft</span></span>                               | <span data-ttu-id="178bf-312">İzleme</span><span class="sxs-lookup"><span data-stu-id="178bf-312">Trace</span></span>             |

<span data-ttu-id="178bf-313">Olduğunda bir `ILogger` nesnesi oluşturulur, `ILoggerFactory` nesne bu Günlükçü için uygulanacak sağlayıcı başına tek bir kural seçer.</span><span class="sxs-lookup"><span data-stu-id="178bf-313">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="178bf-314">Tüm iletileri tarafından yazılmış bir `ILogger` örneği seçili kuralları göre filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="178bf-314">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="178bf-315">Her bir sağlayıcı ve kategori çifti için olası en belirgin kural kullanılabilir kurallardan seçilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-315">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="178bf-316">Aşağıdaki algoritmadan her sağlayıcısı için kullanılan zaman bir `ILogger` için belirli bir kategori oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="178bf-316">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="178bf-317">Sağlayıcı veya diğer adıyla eşleşen tüm kuralları'nı seçin.</span><span class="sxs-lookup"><span data-stu-id="178bf-317">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="178bf-318">Eşleşme bulunursa, tüm kuralları ile boş bir sağlayıcı seçin.</span><span class="sxs-lookup"><span data-stu-id="178bf-318">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="178bf-319">Önceki adımda sonuçtan seçme kuralları ile en uzun kategori ön ek eşleştirme.</span><span class="sxs-lookup"><span data-stu-id="178bf-319">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="178bf-320">Eşleşme bulunursa, bir kategori belirtmeyin tüm kuralları'nı seçin.</span><span class="sxs-lookup"><span data-stu-id="178bf-320">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="178bf-321">Birden çok kural seçtiyseniz ele **son** biri.</span><span class="sxs-lookup"><span data-stu-id="178bf-321">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="178bf-322">Hiçbir kural seçtiyseniz, kullanın `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="178bf-322">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="178bf-323">İle önceki kurallar listesinde, oluşturduğunuz düşünün bir `ILogger` nesne kategorisi "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" için:</span><span class="sxs-lookup"><span data-stu-id="178bf-323">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="178bf-324">Hata ayıklama sağlayıcısı için 1, 6 ve 8 kuralları geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="178bf-324">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="178bf-325">Kural 8 en belirgin olduğundan, seçili durumdaki.</span><span class="sxs-lookup"><span data-stu-id="178bf-325">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="178bf-326">Konsolu sağlayıcısı için 3, 4, 5 ve 6 kuralları geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="178bf-326">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="178bf-327">Kural 3 en belirgin değil.</span><span class="sxs-lookup"><span data-stu-id="178bf-327">Rule 3 is most specific.</span></span>

<span data-ttu-id="178bf-328">Ortaya çıkan `ILogger` örneği gönderir günlüklerini `Trace` düzeyi ve yukarıdaki hata ayıklama sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="178bf-328">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="178bf-329">Günlükleri `Debug` düzeyi ve konsol sağlayıcısına yukarıdaki gönderilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-329">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="178bf-330">Sağlayıcı diğer adları</span><span class="sxs-lookup"><span data-stu-id="178bf-330">Provider aliases</span></span>

<span data-ttu-id="178bf-331">Her bir sağlayıcı tanımlar bir *diğer* yapılandırmasında tam nitelikli tür adı yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-331">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="178bf-332">Yerleşik sağlayıcılar için aşağıdaki diğer adlar kullanın:</span><span class="sxs-lookup"><span data-stu-id="178bf-332">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="178bf-333">Konsol</span><span class="sxs-lookup"><span data-stu-id="178bf-333">Console</span></span>
* <span data-ttu-id="178bf-334">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-334">Debug</span></span>
* <span data-ttu-id="178bf-335">EventLog</span><span class="sxs-lookup"><span data-stu-id="178bf-335">EventLog</span></span>
* <span data-ttu-id="178bf-336">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="178bf-336">AzureAppServicesFile</span></span>
* <span data-ttu-id="178bf-337">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="178bf-337">AzureAppServicesBlob</span></span>
* <span data-ttu-id="178bf-338">TraceSource</span><span class="sxs-lookup"><span data-stu-id="178bf-338">TraceSource</span></span>
* <span data-ttu-id="178bf-339">EventSource</span><span class="sxs-lookup"><span data-stu-id="178bf-339">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="178bf-340">Varsayılan en düşük düzeyi</span><span class="sxs-lookup"><span data-stu-id="178bf-340">Default minimum level</span></span>

<span data-ttu-id="178bf-341">Yalnızca yapılandırma veya kod hiçbir kural belirtilen sağlayıcı ve kategori için uygularsanız, etkinleşir en az bir düzeyi ayarı yoktur.</span><span class="sxs-lookup"><span data-stu-id="178bf-341">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="178bf-342">Aşağıdaki örnek, en düşük düzeyi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="178bf-342">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="178bf-343">En düşük düzey açıkça ayarlamazsanız, varsayılan değer: `Information`, anlamına `Trace` ve `Debug` günlükleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="178bf-343">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="178bf-344">Filtre işlevleri</span><span class="sxs-lookup"><span data-stu-id="178bf-344">Filter functions</span></span>

<span data-ttu-id="178bf-345">Tüm sağlayıcıları ve yapılandırma veya kod tarafından atanmış kural kategorisi için bir filtre işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="178bf-345">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="178bf-346">İşlev kodu, sağlayıcı türü, kategori ve günlük düzeyi erişimi vardır.</span><span class="sxs-lookup"><span data-stu-id="178bf-346">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="178bf-347">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="178bf-347">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="178bf-348">Bazı günlük sağlayıcıları, ne zaman günlükleri bir depolama ortamına veya göz ardı belirtin günlük düzeyi ve kategoriye göre olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="178bf-348">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="178bf-349">`AddConsole` Ve `AddDebug` genişletme yöntemleri filtreleme ölçütleri kabul eden aşırı yükler sağlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-349">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="178bf-350">Aşağıdaki örnek kod konsol sağlayıcı günlüklere yok saymak neden `Warning` düzey sırasında hata ayıklama sağlayıcısı framework oluşturduğu günlükleri yok sayar.</span><span class="sxs-lookup"><span data-stu-id="178bf-350">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="178bf-351">`AddEventLog` Yöntemi olan alan bir aşırı yüklemesini bir `EventLogSettings` filtreleme bir işlevde içerebilen örneği kendi `Filter` özelliği.</span><span class="sxs-lookup"><span data-stu-id="178bf-351">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="178bf-352">Kendi günlük düzeyi ve diğer parametrelere dayanır beri TraceSource sağlayıcısı bu aşırı yüklemeler hiçbirini sağlamaz `SourceSwitch` ve `TraceListener` kullanır.</span><span class="sxs-lookup"><span data-stu-id="178bf-352">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="178bf-353">Filtreleme kuralları ile kayıtlı tüm sağlayıcılar için ayarlanacak bir `ILoggerFactory` kullanın, örnek `WithFilter` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="178bf-353">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="178bf-354">Aşağıdaki örnek, uygulama kodu tarafından oluşturulan günlükleri için hata ayıklama düzeyinde açılırken uyarıları framework günlükleri (kategorisi "Microsoft" veya "Sistem" ile başlayan) sınırlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-354">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="178bf-355">Yazılmakta olan herhangi bir günlük önlemek için şunu belirtin `LogLevel.None` en düşük günlük düzeyi olarak.</span><span class="sxs-lookup"><span data-stu-id="178bf-355">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="178bf-356">Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="178bf-356">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="178bf-357">`WithFilter` Genişletme yöntemi tarafından sağlanır [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="178bf-357">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="178bf-358">Yeni bir yöntem döndürür `ILoggerFactory` tüm Günlükçü sağlayıcıları için geçirilen günlük iletileri filtreler örneği ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="178bf-358">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="178bf-359">Diğer etkilemez `ILoggerFactory` örnekleri, özgün dahil olmak üzere `ILoggerFactory` örneği.</span><span class="sxs-lookup"><span data-stu-id="178bf-359">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="178bf-360">Sistem kategorileri ve düzeyleri</span><span class="sxs-lookup"><span data-stu-id="178bf-360">System categories and levels</span></span>

<span data-ttu-id="178bf-361">Hangi günlüklerin bunları beklediğiniz hakkında notlar ile ASP.NET Core ve Entity Framework Core tarafından kullanılan bazı kategorileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="178bf-361">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="178bf-362">Kategori</span><span class="sxs-lookup"><span data-stu-id="178bf-362">Category</span></span>                            | <span data-ttu-id="178bf-363">Notlar</span><span class="sxs-lookup"><span data-stu-id="178bf-363">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="178bf-364">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="178bf-364">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="178bf-365">ASP.NET Core genel tanılama.</span><span class="sxs-lookup"><span data-stu-id="178bf-365">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="178bf-366">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="178bf-366">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="178bf-367">Hangi anahtarları kabul bulundu kullanılan ve.</span><span class="sxs-lookup"><span data-stu-id="178bf-367">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="178bf-368">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="178bf-368">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="178bf-369">İzin verilen bir ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="178bf-369">Hosts allowed.</span></span> |
| <span data-ttu-id="178bf-370">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="178bf-370">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="178bf-371">Tamamlanması için geçen süreyi HTTP istekleri ve ne zaman başladıklarını.</span><span class="sxs-lookup"><span data-stu-id="178bf-371">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="178bf-372">Hangi barındırma başlangıç derlemeler yüklü.</span><span class="sxs-lookup"><span data-stu-id="178bf-372">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="178bf-373">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="178bf-373">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="178bf-374">MVC ve Razor tanılama.</span><span class="sxs-lookup"><span data-stu-id="178bf-374">MVC and Razor diagnostics.</span></span> <span data-ttu-id="178bf-375">Model bağlama, filtre yürütme, görünüm derlemesi eylem seçimi.</span><span class="sxs-lookup"><span data-stu-id="178bf-375">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="178bf-376">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="178bf-376">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="178bf-377">Bilgi eşleşen rota.</span><span class="sxs-lookup"><span data-stu-id="178bf-377">Route matching information.</span></span> |
| <span data-ttu-id="178bf-378">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="178bf-378">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="178bf-379">Bağlantı başlatma, durdurma ve canlı tutma yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-379">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="178bf-380">HTTPS sertifika bilgileri.</span><span class="sxs-lookup"><span data-stu-id="178bf-380">HTTPS certificate information.</span></span> |
| <span data-ttu-id="178bf-381">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="178bf-381">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="178bf-382">Dosyaları sunulur.</span><span class="sxs-lookup"><span data-stu-id="178bf-382">Files served.</span></span> |
| <span data-ttu-id="178bf-383">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="178bf-383">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="178bf-384">Genel Entity Framework Core tanılama.</span><span class="sxs-lookup"><span data-stu-id="178bf-384">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="178bf-385">Veritabanı geçişlerini etkinlik ve değişiklik algılama yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="178bf-385">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="178bf-386">Günlük kapsamları</span><span class="sxs-lookup"><span data-stu-id="178bf-386">Log scopes</span></span>

 <span data-ttu-id="178bf-387">A *kapsam* mantıksal işlem kümesi gruplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="178bf-387">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="178bf-388">Bu gruplandırma, aynı veri kümesinin bir parçası oluşturulan her bir günlüğe eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-388">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="178bf-389">Örneğin, bir işlem işleme bir parçası olarak oluşturulan her günlük işlem kimliği içerebilir</span><span class="sxs-lookup"><span data-stu-id="178bf-389">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="178bf-390">Bir kapsam bir `IDisposable` tarafından döndürülen tür <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> yöntemi ve bırakılana kadar bağlanabilmelerini.</span><span class="sxs-lookup"><span data-stu-id="178bf-390">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="178bf-391">Günlükçü çağrılarında sarmalama tarafından bir kapsamı kullanan bir `using` engelle:</span><span class="sxs-lookup"><span data-stu-id="178bf-391">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="178bf-392">Aşağıdaki kod, konsolu sağlayıcısı için kapsamları etkinleştirir:</span><span class="sxs-lookup"><span data-stu-id="178bf-392">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="178bf-393">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="178bf-393">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="178bf-394">Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="178bf-394">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="178bf-395">Yapılandırması hakkında daha fazla bilgi için bkz: [yapılandırma](#configuration) bölümü.</span><span class="sxs-lookup"><span data-stu-id="178bf-395">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="178bf-396">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="178bf-396">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="178bf-397">Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="178bf-397">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="178bf-398">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="178bf-398">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="178bf-399">Her günlük iletisi kapsamlı bilgiler içerir:</span><span class="sxs-lookup"><span data-stu-id="178bf-399">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="178bf-400">Yerleşik günlük sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="178bf-400">Built-in logging providers</span></span>

<span data-ttu-id="178bf-401">ASP.NET Core aşağıdaki sağlayıcıları birlikte gelir:</span><span class="sxs-lookup"><span data-stu-id="178bf-401">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="178bf-402">Console</span><span class="sxs-lookup"><span data-stu-id="178bf-402">Console</span></span>](#console-provider)
* [<span data-ttu-id="178bf-403">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-403">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="178bf-404">EventSource</span><span class="sxs-lookup"><span data-stu-id="178bf-404">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="178bf-405">EventLog</span><span class="sxs-lookup"><span data-stu-id="178bf-405">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="178bf-406">TraceSource</span><span class="sxs-lookup"><span data-stu-id="178bf-406">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="178bf-407">Seçenekler [Azure'da oturum](#logging-in-azure) bu makalenin sonraki bölümlerinde ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="178bf-407">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="178bf-408">Stdout günlüğü hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> ve <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="178bf-408">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="178bf-409">Konsolu sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="178bf-409">Console provider</span></span>

<span data-ttu-id="178bf-410">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcı paketi konsola günlük çıktısını gönderir.</span><span class="sxs-lookup"><span data-stu-id="178bf-410">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="178bf-411">[AddConsole aşırı](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) en düşük günlük düzeyi, bir filtre işlevi ve kapsamları desteklenip desteklenmediğini belirten bir Boole değeri geçirdiğiniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-411">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="178bf-412">Başka bir seçenek de geçirmektir bir `IConfiguration` kapsamları desteği ve günlüğe kaydetme düzeylerini belirleyebilirsiniz nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="178bf-412">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="178bf-413">Konsolu sağlayıcısı performansı üzerinde önemli bir etkisi vardır ve genellikle üretim amaçlı kullanım için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="178bf-413">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="178bf-414">Visual Studio'da yeni bir proje oluşturduğunuzda `AddConsole` yöntemi aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="178bf-414">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="178bf-415">Bu kod başvurduğu `Logging` bölümünü *appSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="178bf-415">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="178bf-416">Hata ayıklama düzeyinde açıklandığı şekilde oturum sınırı framework günlükleri uyarılar için uygulama izin verirken gösterilen ayarları [günlük filtreleme](#log-filtering) bölümü.</span><span class="sxs-lookup"><span data-stu-id="178bf-416">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="178bf-417">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="178bf-417">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="178bf-418">Konsol çıkış günlüğü görmek için proje klasöründeki bir komut istemi açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="178bf-418">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="178bf-419">Sağlayıcı hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="178bf-419">Debug provider</span></span>

<span data-ttu-id="178bf-420">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcı paketi kullanarak günlük çıktı Yazar [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) sınıfı (`Debug.WriteLine` yöntem çağrılarını).</span><span class="sxs-lookup"><span data-stu-id="178bf-420">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="178bf-421">Linux üzerinde bu sağlayıcı günlüklerine Yazar */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="178bf-421">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="178bf-422">[AddDebug aşırı](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) en düşük günlük düzeyi veya bir filtre işlevi geçirdiğiniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-422">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="178bf-423">EventSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="178bf-423">EventSource provider</span></span>

<span data-ttu-id="178bf-424">ASP.NET Core 1.1.0 hedefleyen uygulamalar için veya sonraki bir sürümü [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcı paketi, olay izleme uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="178bf-424">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="178bf-425">Windows üzerinde kullandığı [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="178bf-425">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="178bf-426">Platformlar arası sağlayıcısıdır, ancak henüz Linux veya macOS için toplama ve görüntüleme araçları hiçbir olay yok.</span><span class="sxs-lookup"><span data-stu-id="178bf-426">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="178bf-427">Toplamak ve günlükleri görüntülemek için en iyi yolu kullanmaktır [PerfView yardımcı programı](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="178bf-427">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="178bf-428">ETW günlükleri görüntülemek için diğer araçları vardır, ancak PerfView ASP.NET tarafından yayılan ETW olayları ile çalışmak için en iyi deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-428">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="178bf-429">Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView yapılandırmak için dize Ekle `*Microsoft-Extensions-Logging` için **ek sağlayıcılar** listesi.</span><span class="sxs-lookup"><span data-stu-id="178bf-429">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="178bf-430">(Yıldız işareti dizenin başlangıcında kaçırmayın.)</span><span class="sxs-lookup"><span data-stu-id="178bf-430">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview ek sağlayıcılar](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="178bf-432">Windows olay günlüğü sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="178bf-432">Windows EventLog provider</span></span>

<span data-ttu-id="178bf-433">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcı paketi, Windows olay günlüğüne günlük çıktısını gönderir.</span><span class="sxs-lookup"><span data-stu-id="178bf-433">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="178bf-434">[AddEventLog aşırı](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) geçirdiğiniz let `EventLogSettings` veya en düşük günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="178bf-434">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="178bf-435">TraceSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="178bf-435">TraceSource provider</span></span>

<span data-ttu-id="178bf-436">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcı paketi kullanan <xref:System.Diagnostics.TraceSource> kitaplıkları ve sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="178bf-436">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

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

<span data-ttu-id="178bf-437">[AddTraceSource aşırı](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) kaynak anahtarı ve bir izleme dinleyicisi geçirdiğiniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="178bf-437">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="178bf-438">Bu sağlayıcıyı kullanmak için .NET Framework (yerine üzerinde .NET Core) çalıştırmak bir uygulama vardır.</span><span class="sxs-lookup"><span data-stu-id="178bf-438">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="178bf-439">Sağlayıcı için çeşitli iletiler yönlendirebilirsiniz [dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners), gibi <xref:System.Diagnostics.TextWriterTraceListener> örnek uygulamada kullanılan.</span><span class="sxs-lookup"><span data-stu-id="178bf-439">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="178bf-440">Aşağıdaki örnek yapılandırır bir `TraceSource` günlükleri sağlayıcısı `Warning` ve konsol penceresinde daha yüksek ileti.</span><span class="sxs-lookup"><span data-stu-id="178bf-440">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="178bf-441">Azure'da günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="178bf-441">Logging in Azure</span></span>

<span data-ttu-id="178bf-442">Azure'da günlüğe kaydetme hakkında daha fazla bilgi için aşağıdaki bölümlere bakın:</span><span class="sxs-lookup"><span data-stu-id="178bf-442">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="178bf-443">Azure App Service sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="178bf-443">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="178bf-444">Azure günlük akışı</span><span class="sxs-lookup"><span data-stu-id="178bf-444">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="178bf-445">Azure Application Insights izleme günlüğü</span><span class="sxs-lookup"><span data-stu-id="178bf-445">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="178bf-446">Azure App Service sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="178bf-446">Azure App Service provider</span></span>

<span data-ttu-id="178bf-447">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcı paketi metin dosyalarını bir Azure App Service uygulamanın dosya sistemi ve çok günlükler Yazar [blob depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) bir Azure depolama hesabındaki.</span><span class="sxs-lookup"><span data-stu-id="178bf-447">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="178bf-448">Sağlayıcı paketi, .NET Core 1.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="178bf-448">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="178bf-449">.NET Core'u hedefleyen, aşağıdaki noktalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="178bf-449">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="178bf-450">ASP.NET Core sağlayıcısı paketinde [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="178bf-450">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="178bf-451">Sağlayıcı paketi bulunup bulunmadığına [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="178bf-451">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="178bf-452">Sağlayıcıyı kullanmak için paketi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="178bf-452">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="178bf-453">.NET Framework'ü hedefleyen veya başvuru `Microsoft.AspNetCore.App` metapackage, sağlayıcı paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="178bf-453">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="178bf-454">Çağırma `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="178bf-454">Invoke `AddAzureWebAppDiagnostics`:</span></span>

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

<span data-ttu-id="178bf-455">Bir <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> geçirdiğiniz sağlar aşırı <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="178bf-455">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="178bf-456">Ayarlar nesnesini günlük çıktı şablonu, blob adı ve dosya boyutu sınırını gibi varsayılan ayarları geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="178bf-456">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="178bf-457">(*Çıkış şablon* ile sağlanan ek olarak, tüm günlükleri uygulanan bir ileti şablonu bir `ILogger` yöntem çağrısı.)</span><span class="sxs-lookup"><span data-stu-id="178bf-457">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="178bf-458">Sağlayıcı ayarlarını yapılandırmak için kullanmayı <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> ve <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="178bf-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="178bf-459">Bir App Service uygulamasına dağıtmak, uygulama ayarlarında yapılırken [tanılama günlükleri](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) bölümünü **App Service** Azure Portalı'nın sayfasında.</span><span class="sxs-lookup"><span data-stu-id="178bf-459">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="178bf-460">Bu ayarlar güncelleştirildiğinde, değişiklikleri hemen yeniden başlatma veya yeniden dağıtma işlemi uygulamanın gerek olmadan etkinleşir.</span><span class="sxs-lookup"><span data-stu-id="178bf-460">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure günlük ayarları](index/_static/azure-logging-settings.png)

<span data-ttu-id="178bf-462">Günlük dosyaları için varsayılan konum alıyor *D:\\giriş\\LogFiles\\uygulama* klasörü ve varsayılan dosya adı olan *tanılama yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="178bf-462">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="178bf-463">Varsayılan dosya boyutu sınırını 10 MB'tır ve korunan dosyaları varsayılan en yüksek sayısı 2'dir.</span><span class="sxs-lookup"><span data-stu-id="178bf-463">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="178bf-464">Varsayılan blob adı *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="178bf-464">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="178bf-465">Sağlayıcı, yalnızca proje Azure ortamında çalışan olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="178bf-465">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="178bf-466">Projeyi yerel olarak çalıştırdığınızda herhangi bir etkisi&mdash;yerel dosyaları ya da yerel geliştirme deposu bloblar için yazma değil.</span><span class="sxs-lookup"><span data-stu-id="178bf-466">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="178bf-467">Azure günlük akışı</span><span class="sxs-lookup"><span data-stu-id="178bf-467">Azure log streaming</span></span>

<span data-ttu-id="178bf-468">Azure günlük akışını gerçek zamanlı günlüğünü etkinliği görüntülemenize olanak tanır:</span><span class="sxs-lookup"><span data-stu-id="178bf-468">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="178bf-469">Uygulama sunucusu</span><span class="sxs-lookup"><span data-stu-id="178bf-469">The app server</span></span>
* <span data-ttu-id="178bf-470">Web sunucusu</span><span class="sxs-lookup"><span data-stu-id="178bf-470">The web server</span></span>
* <span data-ttu-id="178bf-471">Başarısız istek izleme</span><span class="sxs-lookup"><span data-stu-id="178bf-471">Failed request tracing</span></span>

<span data-ttu-id="178bf-472">Azure günlük akışını yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="178bf-472">To configure Azure log streaming:</span></span>

* <span data-ttu-id="178bf-473">Gidin **tanılama günlükleri** sayfasından uygulamanızın portal sayfası.</span><span class="sxs-lookup"><span data-stu-id="178bf-473">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="178bf-474">Ayarlama **uygulama günlüğü (dosya sistemi)** için **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="178bf-474">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Azure portalı tanılama günlükleri sayfası](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="178bf-476">Gidin **günlük akışını** uygulama iletilerini görüntülemek için sayfa.</span><span class="sxs-lookup"><span data-stu-id="178bf-476">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="178bf-477">Tarafından uygulama üzerinden oturum açmadıysanız `ILogger` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="178bf-477">They're logged by the app through the `ILogger` interface.</span></span>

![Azure portal uygulaması günlük akışı](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="178bf-479">Azure Application Insights izleme günlüğü</span><span class="sxs-lookup"><span data-stu-id="178bf-479">Azure Application Insights trace logging</span></span>

<span data-ttu-id="178bf-480">Application Insights SDK'sı, toplamak ve ASP.NET Core günlük kaydı altyapısı tarafından oluşturulan günlükleri bildirin.</span><span class="sxs-lookup"><span data-stu-id="178bf-480">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="178bf-481">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="178bf-481">For more information, see the following resources:</span></span>

* [<span data-ttu-id="178bf-482">Application Insights'a genel bakış</span><span class="sxs-lookup"><span data-stu-id="178bf-482">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="178bf-483">ASP.NET Core için Application Insights</span><span class="sxs-lookup"><span data-stu-id="178bf-483">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="178bf-484">[Application Insights bağdaştırıcılarını günlüğü](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span><span class="sxs-lookup"><span data-stu-id="178bf-484">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="178bf-485">Application Insights ILogger uygulama örnekleri</span><span class="sxs-lookup"><span data-stu-id="178bf-485">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="178bf-486">Üçüncü taraf günlük sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="178bf-486">Third-party logging providers</span></span>

<span data-ttu-id="178bf-487">ASP.NET Core ile çalışan üçüncü taraf günlük altyapılarına:</span><span class="sxs-lookup"><span data-stu-id="178bf-487">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="178bf-488">[elmah.io](https://elmah.io/) ([GitHub deposunu](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="178bf-488">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="178bf-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposunu](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="178bf-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="178bf-490">[JSNLog](http://jsnlog.com/) ([GitHub deposunu](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="178bf-490">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="178bf-491">[KissLog.net](https://kisslog.net/) ([GitHub deposunu](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="178bf-491">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="178bf-492">[Loggr](http://loggr.net/) ([GitHub deposunu](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="178bf-492">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="178bf-493">[NLog](http://nlog-project.org/) ([GitHub deposunu](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="178bf-493">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="178bf-494">[Sentry](https://sentry.io/welcome/) ([GitHub deposunu](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="178bf-494">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="178bf-495">[Serilog](https://serilog.net/) ([GitHub deposunu](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="178bf-495">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="178bf-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github deposunu](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="178bf-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="178bf-497">Bazı üçüncü taraf çerçeveleri gerçekleştirebilirsiniz [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="178bf-497">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="178bf-498">Bir üçüncü taraf framework kullanarak, yerleşik sağlayıcılardan birini kullanmaya benzer:</span><span class="sxs-lookup"><span data-stu-id="178bf-498">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="178bf-499">Bir NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="178bf-499">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="178bf-500">Çağrısı bir `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="178bf-500">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="178bf-501">Daha fazla bilgi için her sağlayıcının belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="178bf-501">For more information, see each provider's documentation.</span></span> <span data-ttu-id="178bf-502">Üçüncü taraf günlük sağlayıcıları, Microsoft tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="178bf-502">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="178bf-503">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="178bf-503">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
