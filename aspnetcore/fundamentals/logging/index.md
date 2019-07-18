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
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="38ae1-104">ASP.NET Core oturum açma</span><span class="sxs-lookup"><span data-stu-id="38ae1-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="38ae1-105">[Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="38ae1-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="38ae1-106">ASP.NET Core, çeşitli yerleşik ve üçüncü taraf günlük sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler.</span><span class="sxs-lookup"><span data-stu-id="38ae1-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="38ae1-107">Bu makalede, yerleşik sağlayıcılarla günlüğe kaydetme API 'sinin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="38ae1-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="38ae1-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="38ae1-109">Sağlayıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="38ae1-109">Add providers</span></span>

<span data-ttu-id="38ae1-110">Günlük sağlayıcısı günlükleri görüntüler veya depolar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="38ae1-111">Örneğin, konsol sağlayıcısı günlükleri konsolunda görüntüler ve Azure Application Insights sağlayıcısı bunları Azure Application Insights depolar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="38ae1-112">Birden çok sağlayıcı eklenerek Günlükler birden çok hedefe gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="38ae1-113">Bir sağlayıcı eklemek için sağlayıcının `Add{provider name}` uzantı yöntemini *program.cs*içinde çağırın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="38ae1-114">Yukarıdaki kod, ve `Microsoft.Extensions.Logging` `Microsoft.Extensions.Configuration`için başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-114">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="38ae1-115">Varsayılan proje şablonu, aşağıdaki <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>günlük sağlayıcılarını ekleyen çağırır:</span><span class="sxs-lookup"><span data-stu-id="38ae1-115">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="38ae1-116">Konsol</span><span class="sxs-lookup"><span data-stu-id="38ae1-116">Console</span></span>
* <span data-ttu-id="38ae1-117">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-117">Debug</span></span>
* <span data-ttu-id="38ae1-118">EventSource (ASP.NET Core 2,2 ' den başlayarak)</span><span class="sxs-lookup"><span data-stu-id="38ae1-118">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="38ae1-119">`CreateDefaultBuilder`Kullanıyorsanız, varsayılan sağlayıcıları kendi seçimlerinizle değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-119">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="38ae1-120">Öğesini <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>çağırın ve istediğiniz sağlayıcıları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-120">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="38ae1-121">Bir sağlayıcıyı kullanmak için, NuGet paketini yükleyip sağlayıcının uzantı yöntemini bir örneğine <xref:Microsoft.Extensions.Logging.ILoggerFactory>çağırın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-121">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="38ae1-122">ASP.NET Core [bağımlılığı ekleme (dı)](xref:fundamentals/dependency-injection) `ILoggerFactory` örneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-122">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="38ae1-123">Ve genişletme yöntemleri [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) ve [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paketlerinde tanımlanmıştır. `AddDebug` `AddConsole`</span><span class="sxs-lookup"><span data-stu-id="38ae1-123">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="38ae1-124">Her genişletme yöntemi, sağlayıcının `ILoggerFactory.AddProvider` bir örneğini geçirerek yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-124">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="38ae1-125">[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) , `Startup.Configure` yöntemine günlük sağlayıcıları ekler.</span><span class="sxs-lookup"><span data-stu-id="38ae1-125">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="38ae1-126">Daha önce yürütülen koddan günlük çıktısını almak için, `Startup` sınıf oluşturucusunda günlük sağlayıcılarını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-126">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="38ae1-127">Makalenin ilerleyen kısımlarında [yerleşik günlük sağlayıcıları](#built-in-logging-providers) ve [üçüncü taraf günlüğü sağlayıcıları](#third-party-logging-providers) hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-127">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="38ae1-128">Günlükleri oluştur</span><span class="sxs-lookup"><span data-stu-id="38ae1-128">Create logs</span></span>

<span data-ttu-id="38ae1-129">Dı 'dan <xref:Microsoft.Extensions.Logging.ILogger%601> bir nesne al.</span><span class="sxs-lookup"><span data-stu-id="38ae1-129">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="38ae1-130">Aşağıdaki denetleyici örneği ve `Information` `Warning` günlüklerini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="38ae1-130">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="38ae1-131">`TodoController` *Kategori* `TodoApiSample.Controllers.TodoController` (örnek uygulamadaki tam sınıf adı):</span><span class="sxs-lookup"><span data-stu-id="38ae1-131">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="38ae1-132">Aşağıdaki Razor Pages örnek `Information` , *düzeyi* ve `TodoApiSample.Pages.AboutModel` *Kategori*olarak içeren Günlükler oluşturur:</span><span class="sxs-lookup"><span data-stu-id="38ae1-132">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="38ae1-133">Yukarıdaki örnek, ve ile, `Information` *kategorisi*olarak `Warning` ve *düzeyi* `TodoController` olan Günlükler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="38ae1-133">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="38ae1-134">Günlük *düzeyi* günlüğe kaydedilen etkinliğin önem derecesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-134">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="38ae1-135">Günlük *kategorisi* , her günlük ile ilişkili bir dizedir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-135">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="38ae1-136">Örnek, kategori olarak türünde `T` tam adı olan Günlükler oluşturur. `ILogger<T>`</span><span class="sxs-lookup"><span data-stu-id="38ae1-136">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="38ae1-137">[Düzeyler](#log-level) ve [Kategoriler](#log-category) Bu makalenin ilerleyen kısımlarında daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-137">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="38ae1-138">Başlangıçta günlük oluşturma</span><span class="sxs-lookup"><span data-stu-id="38ae1-138">Create logs in Startup</span></span>

<span data-ttu-id="38ae1-139">`Startup` Sınıf içindeki günlükleri yazmak için, Oluşturucu imzasına bir `ILogger` parametre ekleyin:</span><span class="sxs-lookup"><span data-stu-id="38ae1-139">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="38ae1-140">Programda günlük oluşturma</span><span class="sxs-lookup"><span data-stu-id="38ae1-140">Create logs in Program</span></span>

<span data-ttu-id="38ae1-141">`Program` Sınıf içindeki günlükleri yazmak için, dı 'den bir `ILogger` örnek alın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-141">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="38ae1-142">Zaman uyumsuz günlükçü yöntemi yok</span><span class="sxs-lookup"><span data-stu-id="38ae1-142">No asynchronous logger methods</span></span>

<span data-ttu-id="38ae1-143">Günlüğe kaydetme, zaman uyumsuz kodun performans maliyetine değer olmaması kadar hızlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-143">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="38ae1-144">Günlüğe kaydetme veri depoluizin yavaşsa, doğrudan buna yazmayın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-144">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="38ae1-145">Başlangıç olarak günlük iletilerini hızlı bir mağazaya yazmayı ve sonra yavaş depoya daha sonra taşımayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="38ae1-145">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="38ae1-146">Örneğin, SQL Server için oturum açıyorsanız `Log` `Log` Yöntemler zaman uyumlu olduğundan, bunu doğrudan bir yöntemde yapmak istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-146">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="38ae1-147">Bunun yerine, günlük iletilerini bir bellek içi kuyruğa eşzamanlı olarak ekleyin ve bir arka plan çalışanı, SQL Server veri gönderme zaman uyumsuz çalışmasını sağlamak için iletileri kuyruktan çekin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-147">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="38ae1-148">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="38ae1-148">Configuration</span></span>

<span data-ttu-id="38ae1-149">Günlüğe kaydetme sağlayıcısı yapılandırması bir veya daha fazla yapılandırma sağlayıcısı tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="38ae1-149">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="38ae1-150">Dosya biçimleri (ıNı, JSON ve XML).</span><span class="sxs-lookup"><span data-stu-id="38ae1-150">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="38ae1-151">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="38ae1-151">Command-line arguments.</span></span>
* <span data-ttu-id="38ae1-152">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="38ae1-152">Environment variables.</span></span>
* <span data-ttu-id="38ae1-153">Bellek içi .NET nesneleri.</span><span class="sxs-lookup"><span data-stu-id="38ae1-153">In-memory .NET objects.</span></span>
* <span data-ttu-id="38ae1-154">Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolaması.</span><span class="sxs-lookup"><span data-stu-id="38ae1-154">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="38ae1-155">[Azure Key Vault](xref:security/key-vault-configuration)gibi şifreli bir kullanıcı deposu.</span><span class="sxs-lookup"><span data-stu-id="38ae1-155">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="38ae1-156">Özel sağlayıcılar (yüklü veya oluşturulmuş).</span><span class="sxs-lookup"><span data-stu-id="38ae1-156">Custom providers (installed or created).</span></span>

<span data-ttu-id="38ae1-157">Örneğin, günlük yapılandırma genellikle uygulama ayarları dosyalarının `Logging` bölümü tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-157">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="38ae1-158">Aşağıdaki örnek tipik bir appSettings 'in içeriğini gösterir *. Development. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="38ae1-158">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="38ae1-159">Özelliği `Logging` , ve günlük `LogLevel` sağlayıcısı özelliklerine sahip olabilir (konsol gösterilir).</span><span class="sxs-lookup"><span data-stu-id="38ae1-159">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="38ae1-160">Altındaki `LogLevel` özelliği`Logging` , Seçili kategoriler için günlüğe kaydedilecek minimum [düzeyi](#log-level) belirtir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-160">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="38ae1-161">Örnekte `System` ve `Microsoft` kategoriler `Information` düzeyinde günlüğe kaydedilir ve diğer tüm diğerleri `Debug` düzeyinde günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-161">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="38ae1-162">Günlük sağlayıcılarını belirt `Logging` altındaki diğer özellikler.</span><span class="sxs-lookup"><span data-stu-id="38ae1-162">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="38ae1-163">Örnek, konsol sağlayıcısına yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-163">The example is for the Console provider.</span></span> <span data-ttu-id="38ae1-164">Bir sağlayıcı, [günlük kapsamlarını](#log-scopes)destekliyorsa, `IncludeScopes` etkinleştirilip etkinleştirilmeyeceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-164">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="38ae1-165">Bir sağlayıcı özelliği (örnekte olduğu `Console` gibi), bir `LogLevel` özellik de belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-165">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="38ae1-166">`LogLevel`sağlayıcı altında, bu sağlayıcının günlüğe kaydedilecek düzeyleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-166">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="38ae1-167">' De `Logging.{providername}.LogLevel`düzeyler belirtilirse, içinde `Logging.LogLevel`ayarlanan her şeyi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-167">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

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

<span data-ttu-id="38ae1-168">`LogLevel`Anahtarlar günlük adlarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="38ae1-168">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="38ae1-169">`Default` Anahtar açıkça listelenmemiş Günlükler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-169">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="38ae1-170">Değer, belirtilen günlüğe uygulanan [günlük düzeyini](#log-level) temsil eder.</span><span class="sxs-lookup"><span data-stu-id="38ae1-170">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="38ae1-171">Yapılandırma sağlayıcılarını uygulama hakkında daha fazla bilgi için <xref:fundamentals/configuration/index>bkz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-171">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="38ae1-172">Örnek günlüğe kaydetme çıkışı</span><span class="sxs-lookup"><span data-stu-id="38ae1-172">Sample logging output</span></span>

<span data-ttu-id="38ae1-173">Yukarıdaki bölümde gösterilen örnek kodla, uygulama komut satırından çalıştırıldığında Günlükler konsolunda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-173">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="38ae1-174">Konsol çıkışının bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-174">Here's an example of console output:</span></span>

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

<span data-ttu-id="38ae1-175">Yukarıdaki Günlükler, üzerinde `http://localhost:5000/api/todo/0`örnek uygulamaya bir http get isteği yapılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="38ae1-175">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="38ae1-176">Visual Studio 'da örnek uygulamayı çalıştırdığınızda hata ayıklama penceresinde göründükleri günlüklere yönelik bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-176">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="38ae1-177">Yukarıdaki bölümde gösterilen `ILogger` çağrılar tarafından oluşturulan Günlükler "TodoApi. Controllers. TodoController" ile başlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-177">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="38ae1-178">"Microsoft" kategorileri ile başlayan Günlükler ASP.NET Core Framework kodundan alınır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-178">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="38ae1-179">ASP.NET Core ve uygulama kodu aynı günlük API 'sini ve sağlayıcılarını kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="38ae1-179">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="38ae1-180">Bu makalenin geri kalanında günlüğe kaydetme için bazı ayrıntılar ve seçenekler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-180">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="38ae1-181">NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="38ae1-181">NuGet packages</span></span>

<span data-ttu-id="38ae1-182">Ve arabirimleri Microsoft [. Extensions. Logging. soyutlamalar](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)içinde ve bu uygulamalar için varsayılan uygulamalar [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/)içinde bulunur. `ILoggerFactory` `ILogger`</span><span class="sxs-lookup"><span data-stu-id="38ae1-182">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="38ae1-183">Günlük kategorisi</span><span class="sxs-lookup"><span data-stu-id="38ae1-183">Log category</span></span>

<span data-ttu-id="38ae1-184">Bir `ILogger` nesne oluşturulduğunda, için bir *Kategori* belirtilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-184">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="38ae1-185">Bu kategori, bu örneği `ILogger`tarafından oluşturulan her günlük iletisine dahildir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-185">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="38ae1-186">Kategori herhangi bir dize olabilir, ancak kural, "TodoApi. Controllers. TodoController" gibi sınıf adını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-186">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="38ae1-187">Kategori `ILogger<T>` olarak`T` tam nitelikli `ILogger` tür adını kullanan bir örnek almak için kullanın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-187">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="38ae1-188">Kategoriyi açıkça belirtmek için şunu çağırın `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="38ae1-188">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="38ae1-189">`ILogger<T>`, tam nitelikli tür `CreateLogger` `T`adı ile çağırma ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-189">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="38ae1-190">Günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="38ae1-190">Log level</span></span>

<span data-ttu-id="38ae1-191">Her günlük bir <xref:Microsoft.Extensions.Logging.LogLevel> değer belirtir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-191">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="38ae1-192">Günlük düzeyi önem derecesini veya önemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-192">The log level indicates the severity or importance.</span></span> <span data-ttu-id="38ae1-193">Örneğin, bir yöntem normal olarak sona `Information` erdiğinde `Warning` ve bir yöntem *404* bulunmayan bir durum kodu döndürdüğünde bir günlük yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-193">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="38ae1-194">Aşağıdaki kod oluşturur `Information` ve `Warning` günlükleri:</span><span class="sxs-lookup"><span data-stu-id="38ae1-194">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="38ae1-195">Önceki kodda, ilk parametredir [oturum öğesini belirten Olay No.](#log-event-id)</span><span class="sxs-lookup"><span data-stu-id="38ae1-195">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="38ae1-196">İkinci parametre, kalan Yöntem parametreleri tarafından belirtilen bağımsız değişken değerleri için yer tutucuları olan bir ileti şablonudur.</span><span class="sxs-lookup"><span data-stu-id="38ae1-196">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="38ae1-197">Yöntem parametreleri bu makalenin ilerleyen kısımlarında bulunan [ileti şablonu bölümünde](#log-message-template) açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-197">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="38ae1-198">Yöntem adında düzeyi (örneğin, `LogInformation` ve `LogWarning`) içeren günlük yöntemleri, [ILogger için uzantı yöntemleridir](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="38ae1-198">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="38ae1-199">Bu yöntemler bir `LogLevel` parametre `Log` alan bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-199">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="38ae1-200">`Log` Yöntemi bu uzantı yöntemlerinden biri yerine doğrudan çağırabilirsiniz, ancak söz dizimi görece karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-200">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="38ae1-201">Daha fazla bilgi için bkz <xref:Microsoft.Extensions.Logging.ILogger> . ve [günlükçü uzantıları kaynak kodu](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="38ae1-201">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="38ae1-202">ASP.NET Core, en küçükten en yüksek öneme doğru sıralanan aşağıdaki günlük düzeylerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-202">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="38ae1-203">İzleme = 0</span><span class="sxs-lookup"><span data-stu-id="38ae1-203">Trace = 0</span></span>

  <span data-ttu-id="38ae1-204">Genellikle yalnızca hata ayıklama için değerli bilgiler için.</span><span class="sxs-lookup"><span data-stu-id="38ae1-204">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="38ae1-205">Bu iletiler hassas uygulama verileri içerebilir, bu nedenle bir üretim ortamında etkinleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-205">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="38ae1-206">*Varsayılan olarak devre dışıdır.*</span><span class="sxs-lookup"><span data-stu-id="38ae1-206">*Disabled by default.*</span></span>

* <span data-ttu-id="38ae1-207">Hata Ayıkla = 1</span><span class="sxs-lookup"><span data-stu-id="38ae1-207">Debug = 1</span></span>

  <span data-ttu-id="38ae1-208">Geliştirme ve hata ayıklama konusunda yararlı olabilecek bilgiler için.</span><span class="sxs-lookup"><span data-stu-id="38ae1-208">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="38ae1-209">Örnek: `Entering method Configure with flag set to true.`En `Debug` yüksek günlük hacimden dolayı yalnızca sorun giderirken üretimde düzey günlüklerini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-209">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="38ae1-210">Bilgi = 2</span><span class="sxs-lookup"><span data-stu-id="38ae1-210">Information = 2</span></span>

  <span data-ttu-id="38ae1-211">Uygulamanın genel akışını izlemek için.</span><span class="sxs-lookup"><span data-stu-id="38ae1-211">For tracking the general flow of the app.</span></span> <span data-ttu-id="38ae1-212">Bu günlüklerde genellikle uzun süreli bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-212">These logs typically have some long-term value.</span></span> <span data-ttu-id="38ae1-213">Örnek: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="38ae1-213">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="38ae1-214">Uyarı = 3</span><span class="sxs-lookup"><span data-stu-id="38ae1-214">Warning = 3</span></span>

  <span data-ttu-id="38ae1-215">Uygulama akışında anormal veya beklenmedik olaylar için.</span><span class="sxs-lookup"><span data-stu-id="38ae1-215">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="38ae1-216">Bunlar, uygulamanın durmasına neden olmayan ancak araştırılması gerekebilecek hataları veya diğer koşulları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-216">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="38ae1-217">İşlenmiş özel durumlar, `Warning` günlük düzeyini kullanmak için yaygın bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-217">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="38ae1-218">Örnek: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="38ae1-218">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="38ae1-219">Hata = 4</span><span class="sxs-lookup"><span data-stu-id="38ae1-219">Error = 4</span></span>

  <span data-ttu-id="38ae1-220">İşlenemeyen hatalar ve özel durumlar için.</span><span class="sxs-lookup"><span data-stu-id="38ae1-220">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="38ae1-221">Bu iletiler, uygulama genelinde bir hata değil geçerli etkinlikte veya işlemde (geçerli HTTP isteği gibi) bir hata olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-221">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="38ae1-222">Örnek günlük iletisi:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="38ae1-222">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="38ae1-223">Kritik = 5</span><span class="sxs-lookup"><span data-stu-id="38ae1-223">Critical = 5</span></span>

  <span data-ttu-id="38ae1-224">Anında ilgilenilmesi gereken hatalarda.</span><span class="sxs-lookup"><span data-stu-id="38ae1-224">For failures that require immediate attention.</span></span> <span data-ttu-id="38ae1-225">Örnekler: veri kaybı senaryoları, disk alanı yetersiz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-225">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="38ae1-226">Belirli bir depolama ortamında veya görüntüleme penceresinde ne kadar günlük çıkışının yazıldığını denetlemek için günlük düzeyini kullanın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-226">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="38ae1-227">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="38ae1-227">For example:</span></span>

* <span data-ttu-id="38ae1-228">Üretimde, birim veri `Trace` deposuna `Information` geçiş düzeyi olarak gönderin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-228">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="38ae1-229">' İ bir değer veri deposuna gönderin `Warning`. `Critical`</span><span class="sxs-lookup"><span data-stu-id="38ae1-229">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="38ae1-230">Geliştirme sırasında konsola gönderin `Warning` `Critical` ve sorun giderme sırasında aracılığıyla `Information` ekleyin `Trace` .</span><span class="sxs-lookup"><span data-stu-id="38ae1-230">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="38ae1-231">Bu makalede daha sonra bulunan [günlük filtreleme](#log-filtering) bölümünde, bir sağlayıcının hangi günlük düzeylerinin işlediğini nasıl denetleneceği açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-231">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="38ae1-232">ASP.NET Core çerçeve olayları için günlükleri yazar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-232">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="38ae1-233">Bu makalenin önceki kısımlarında yer alınan günlük örnekleri, günlüklerin `Information` altında tutulur, bu `Debug` nedenle `Trace` hiçbir veya düzeyi günlük oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-233">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="38ae1-234">Günlükleri göstermek `Debug` için yapılandırılmış örnek uygulama çalıştırılarak oluşturulan konsol günlüklerinin bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-234">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="38ae1-235">Günlüğe olay KIMLIĞI</span><span class="sxs-lookup"><span data-stu-id="38ae1-235">Log event ID</span></span>

<span data-ttu-id="38ae1-236">Her günlük belirtebilirsiniz bir *öğesini belirten Olay No*.</span><span class="sxs-lookup"><span data-stu-id="38ae1-236">Each log can specify an *event ID*.</span></span> <span data-ttu-id="38ae1-237">Örnek uygulama bunu yerel olarak tanımlanmış `LoggingEvents` bir sınıf kullanarak yapar:</span><span class="sxs-lookup"><span data-stu-id="38ae1-237">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="38ae1-238">Olay KIMLIĞI bir olay kümesini ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-238">An event ID associates a set of events.</span></span> <span data-ttu-id="38ae1-239">Örneğin, bir sayfadaki öğelerin listesini görüntülemek için ilgili tüm Günlükler 1001 olabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-239">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="38ae1-240">Günlüğe kaydetme sağlayıcısı, olay KIMLIĞINI günlüğe kaydetme iletisindeki kimlik alanında veya hiç değil, bir kimlik alanında saklayabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-240">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="38ae1-241">Hata ayıklama sağlayıcısı olay kimliklerini göstermiyor.</span><span class="sxs-lookup"><span data-stu-id="38ae1-241">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="38ae1-242">Konsol sağlayıcısı, etkinlik kimliklerini kategoriden sonra parantez içinde gösterir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-242">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="38ae1-243">Günlük iletisi şablonu</span><span class="sxs-lookup"><span data-stu-id="38ae1-243">Log message template</span></span>

<span data-ttu-id="38ae1-244">Her günlük bir ileti şablonunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-244">Each log specifies a message template.</span></span> <span data-ttu-id="38ae1-245">İleti şablonu, bağımsız değişkenlerin sağlandığı yer tutucuları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-245">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="38ae1-246">Sayılar değil, yer tutucular için adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-246">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="38ae1-247">Adlarının, değerlerinin sağlanması için hangi parametrelerin kullanılacağını belirleyen yer tutucular sırası.</span><span class="sxs-lookup"><span data-stu-id="38ae1-247">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="38ae1-248">Aşağıdaki kodda, parametre adlarının ileti şablonunda sıra dışı olduğuna dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="38ae1-248">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="38ae1-249">Bu kod, sırasıyla parametre değerleriyle bir günlük iletisi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="38ae1-249">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="38ae1-250">Günlüğe kaydetme altyapısı bu şekilde çalışarak, günlük sağlayıcılarının [yapılandırılmış günlüğe yazma olarak da bilinen anlamsal günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulayabilmesini sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-250">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="38ae1-251">Bağımsız değişkenler yalnızca biçimli ileti şablonuna değil, günlük sistemine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-251">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="38ae1-252">Bu bilgiler, günlük sağlayıcılarının parametre değerlerini alan olarak depolamasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-252">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="38ae1-253">Örneğin, günlükçü yönteminin şuna benzer şekilde göründüğünü varsayın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-253">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="38ae1-254">Günlükleri Azure Tablo depolama alanına gönderiyorsanız, her bir Azure Tablo varlığı, günlük verilerinde sorguları basitleştiren `ID` ve `RequestTime` özelliklere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-254">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="38ae1-255">Bir sorgu, belirli `RequestTime` bir aralıktaki tüm günlükleri, kısa mesajdan zaman aşımını ayrıştırmadan bulabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-255">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="38ae1-256">Günlüğe kaydetme özel durumları</span><span class="sxs-lookup"><span data-stu-id="38ae1-256">Logging exceptions</span></span>

<span data-ttu-id="38ae1-257">Günlükçü yöntemlerinin, aşağıdaki örnekte olduğu gibi bir özel durum iletmenizi sağlayan aşırı yüklemeleri vardır:</span><span class="sxs-lookup"><span data-stu-id="38ae1-257">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="38ae1-258">Özel durum bilgileri farklı yollarla farklı sağlayıcılarda işler.</span><span class="sxs-lookup"><span data-stu-id="38ae1-258">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="38ae1-259">Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktısına bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-259">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="38ae1-260">Günlük filtreleme</span><span class="sxs-lookup"><span data-stu-id="38ae1-260">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="38ae1-261">Belirli bir sağlayıcı ve kategori için en az bir günlük düzeyi veya tüm sağlayıcılar ya da tüm kategoriler için belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-261">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="38ae1-262">Minimum düzeyin altındaki tüm Günlükler bu sağlayıcıya aktarılmaz, bu nedenle görüntülenmez veya depolanmaz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-262">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="38ae1-263">Tüm günlükleri gizlemek için en düşük `LogLevel.None` günlük düzeyi olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-263">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="38ae1-264">Öğesinin `LogLevel.None` tamsayı değeri 6 ' dır `LogLevel.Critical` (5) daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-264">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="38ae1-265">Yapılandırmada filtre kuralları oluşturma</span><span class="sxs-lookup"><span data-stu-id="38ae1-265">Create filter rules in configuration</span></span>

<span data-ttu-id="38ae1-266">Proje şablonu kodu, konsol `CreateDefaultBuilder` ve hata ayıklama sağlayıcılarının günlüğünü ayarlamak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-266">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="38ae1-267">Yöntemi, aşağıdaki gibi bir kod kullanarak bir `Logging` bölümde yapılandırmayı aramak için günlüğe kaydetmeyi de ayarlar: `CreateDefaultBuilder`</span><span class="sxs-lookup"><span data-stu-id="38ae1-267">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17)]

<span data-ttu-id="38ae1-268">Yapılandırma verileri aşağıdaki örnekte olduğu gibi sağlayıcıya ve kategoriye göre en düşük günlük düzeylerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-268">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="38ae1-269">Bu JSON altı filtre kuralı oluşturur: biri hata ayıklama sağlayıcısı, konsol sağlayıcısı için dört ve diğeri tüm sağlayıcılar için.</span><span class="sxs-lookup"><span data-stu-id="38ae1-269">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="38ae1-270">Bir `ILogger` nesne oluşturulduğunda her sağlayıcı için tek bir kural seçilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-270">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="38ae1-271">Koddaki filtre kuralları</span><span class="sxs-lookup"><span data-stu-id="38ae1-271">Filter rules in code</span></span>

<span data-ttu-id="38ae1-272">Aşağıdaki örnek, koddaki filtre kurallarının nasıl kaydedileceği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-272">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="38ae1-273">İkincisi `AddFilter` , hata ayıklama sağlayıcısını tür adını kullanarak belirtir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-273">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="38ae1-274">Birincisi `AddFilter` bir sağlayıcı türü belirtmediğinden, tüm sağlayıcılar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-274">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="38ae1-275">Filtreleme kuralları nasıl uygulanır</span><span class="sxs-lookup"><span data-stu-id="38ae1-275">How filtering rules are applied</span></span>

<span data-ttu-id="38ae1-276">Yapılandırma verileri ve `AddFilter` önceki örneklerde gösterilen kod, aşağıdaki tabloda gösterilen kuralları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="38ae1-276">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="38ae1-277">İlk altı yapılandırma örneğinde ve son iki ise kod örneğinde gelir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-277">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="38ae1-278">Sayı</span><span class="sxs-lookup"><span data-stu-id="38ae1-278">Number</span></span> | <span data-ttu-id="38ae1-279">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="38ae1-279">Provider</span></span>      | <span data-ttu-id="38ae1-280">Şununla başlayan Kategoriler...</span><span class="sxs-lookup"><span data-stu-id="38ae1-280">Categories that begin with ...</span></span>          | <span data-ttu-id="38ae1-281">En düşük günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="38ae1-281">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="38ae1-282">1\.</span><span class="sxs-lookup"><span data-stu-id="38ae1-282">1</span></span>      | <span data-ttu-id="38ae1-283">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-283">Debug</span></span>         | <span data-ttu-id="38ae1-284">Tüm Kategoriler</span><span class="sxs-lookup"><span data-stu-id="38ae1-284">All categories</span></span>                          | <span data-ttu-id="38ae1-285">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="38ae1-285">Information</span></span>       |
| <span data-ttu-id="38ae1-286">2</span><span class="sxs-lookup"><span data-stu-id="38ae1-286">2</span></span>      | <span data-ttu-id="38ae1-287">Konsol</span><span class="sxs-lookup"><span data-stu-id="38ae1-287">Console</span></span>       | <span data-ttu-id="38ae1-288">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="38ae1-288">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="38ae1-289">Uyarı</span><span class="sxs-lookup"><span data-stu-id="38ae1-289">Warning</span></span>           |
| <span data-ttu-id="38ae1-290">3</span><span class="sxs-lookup"><span data-stu-id="38ae1-290">3</span></span>      | <span data-ttu-id="38ae1-291">Konsol</span><span class="sxs-lookup"><span data-stu-id="38ae1-291">Console</span></span>       | <span data-ttu-id="38ae1-292">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="38ae1-292">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="38ae1-293">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-293">Debug</span></span>             |
| <span data-ttu-id="38ae1-294">4</span><span class="sxs-lookup"><span data-stu-id="38ae1-294">4</span></span>      | <span data-ttu-id="38ae1-295">Konsol</span><span class="sxs-lookup"><span data-stu-id="38ae1-295">Console</span></span>       | <span data-ttu-id="38ae1-296">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="38ae1-296">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="38ae1-297">Hata</span><span class="sxs-lookup"><span data-stu-id="38ae1-297">Error</span></span>             |
| <span data-ttu-id="38ae1-298">5</span><span class="sxs-lookup"><span data-stu-id="38ae1-298">5</span></span>      | <span data-ttu-id="38ae1-299">Konsol</span><span class="sxs-lookup"><span data-stu-id="38ae1-299">Console</span></span>       | <span data-ttu-id="38ae1-300">Tüm Kategoriler</span><span class="sxs-lookup"><span data-stu-id="38ae1-300">All categories</span></span>                          | <span data-ttu-id="38ae1-301">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="38ae1-301">Information</span></span>       |
| <span data-ttu-id="38ae1-302">6</span><span class="sxs-lookup"><span data-stu-id="38ae1-302">6</span></span>      | <span data-ttu-id="38ae1-303">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="38ae1-303">All providers</span></span> | <span data-ttu-id="38ae1-304">Tüm Kategoriler</span><span class="sxs-lookup"><span data-stu-id="38ae1-304">All categories</span></span>                          | <span data-ttu-id="38ae1-305">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-305">Debug</span></span>             |
| <span data-ttu-id="38ae1-306">7</span><span class="sxs-lookup"><span data-stu-id="38ae1-306">7</span></span>      | <span data-ttu-id="38ae1-307">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="38ae1-307">All providers</span></span> | <span data-ttu-id="38ae1-308">Sistem</span><span class="sxs-lookup"><span data-stu-id="38ae1-308">System</span></span>                                  | <span data-ttu-id="38ae1-309">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-309">Debug</span></span>             |
| <span data-ttu-id="38ae1-310">8</span><span class="sxs-lookup"><span data-stu-id="38ae1-310">8</span></span>      | <span data-ttu-id="38ae1-311">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-311">Debug</span></span>         | <span data-ttu-id="38ae1-312">Microsoft</span><span class="sxs-lookup"><span data-stu-id="38ae1-312">Microsoft</span></span>                               | <span data-ttu-id="38ae1-313">İzlemesinin</span><span class="sxs-lookup"><span data-stu-id="38ae1-313">Trace</span></span>             |

<span data-ttu-id="38ae1-314">Bir `ILogger` nesne oluşturulduğunda `ILoggerFactory` , nesne bu günlükçü için uygulanacak her sağlayıcı için tek bir kural seçer.</span><span class="sxs-lookup"><span data-stu-id="38ae1-314">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="38ae1-315">Bir `ILogger` örnek tarafından yazılan tüm iletiler, seçilen kurallara göre filtrelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-315">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="38ae1-316">Her sağlayıcı ve kategori çifti için mümkün olan en özel kural kullanılabilir kurallardan seçilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-316">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="38ae1-317">Belirli bir kategori için oluşturulan her sağlayıcı `ILogger` için aşağıdaki algoritma kullanılır:</span><span class="sxs-lookup"><span data-stu-id="38ae1-317">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="38ae1-318">Sağlayıcı veya diğer adıyla eşleşen tüm kuralları seçin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-318">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="38ae1-319">Hiçbir eşleşme bulunmazsa, boş bir sağlayıcıya sahip tüm kurallar ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-319">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="38ae1-320">Önceki adımın sonucunda, en uzun eşleşen kategori ön ekine sahip kurallar ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-320">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="38ae1-321">Eşleşme bulunmazsa, kategori belirtmeyen tüm kuralları seçin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-321">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="38ae1-322">Birden çok kural seçilirse, **son** olanı götürün.</span><span class="sxs-lookup"><span data-stu-id="38ae1-322">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="38ae1-323">Hiçbir kural seçilmezse, kullanın `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="38ae1-323">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="38ae1-324">Yukarıdaki kurallar listesinde, "Microsoft. aspnetcore. Mvc `ILogger` . Razor. RazorViewEngine" kategorisi için bir nesne oluşturduğunuzu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="38ae1-324">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="38ae1-325">Hata ayıklama sağlayıcısı, kurallar 1, 6 ve 8 için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-325">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="38ae1-326">Kural 8 ' i en özeldir, yani seçili olanı seçilidir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-326">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="38ae1-327">Konsol sağlayıcısı için, kurallar 3, 4, 5 ve 6 geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-327">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="38ae1-328">Kural 3 en özeldir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-328">Rule 3 is most specific.</span></span>

<span data-ttu-id="38ae1-329">Elde edilen `ILogger` örnek, hata ayıklama `Trace` sağlayıcısına düzeyi ve üzeri Günlükler gönderir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-329">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="38ae1-330">`Debug` Düzeyi ve üzeri Günlükler konsol sağlayıcısına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-330">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="38ae1-331">Sağlayıcı diğer adları</span><span class="sxs-lookup"><span data-stu-id="38ae1-331">Provider aliases</span></span>

<span data-ttu-id="38ae1-332">Her sağlayıcı, tam nitelikli tür adı yerine yapılandırmada kullanılabilecek bir *diğer ad* tanımlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-332">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="38ae1-333">Yerleşik sağlayıcılar için aşağıdaki diğer adları kullanın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-333">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="38ae1-334">Konsol</span><span class="sxs-lookup"><span data-stu-id="38ae1-334">Console</span></span>
* <span data-ttu-id="38ae1-335">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-335">Debug</span></span>
* <span data-ttu-id="38ae1-336">EventSource</span><span class="sxs-lookup"><span data-stu-id="38ae1-336">EventSource</span></span>
* <span data-ttu-id="38ae1-337">EventLog</span><span class="sxs-lookup"><span data-stu-id="38ae1-337">EventLog</span></span>
* <span data-ttu-id="38ae1-338">TraceSource</span><span class="sxs-lookup"><span data-stu-id="38ae1-338">TraceSource</span></span>
* <span data-ttu-id="38ae1-339">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="38ae1-339">AzureAppServicesFile</span></span>
* <span data-ttu-id="38ae1-340">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="38ae1-340">AzureAppServicesBlob</span></span>
* <span data-ttu-id="38ae1-341">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="38ae1-341">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="38ae1-342">Varsayılan en düşük düzey</span><span class="sxs-lookup"><span data-stu-id="38ae1-342">Default minimum level</span></span>

<span data-ttu-id="38ae1-343">Yalnızca belirli bir sağlayıcı ve kategori için yapılandırma veya koddan kural uygulanmaz geçerli olan en düşük düzey ayar vardır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-343">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="38ae1-344">Aşağıdaki örnekte, en düşük düzeyin nasıl ayarlanacağı gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-344">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="38ae1-345">En düşük düzeyi açıkça ayarlamazsanız, varsayılan değer olur `Information`ve `Debug` bu `Trace` , günlüklerin yok sayılacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-345">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="38ae1-346">Filtre işlevleri</span><span class="sxs-lookup"><span data-stu-id="38ae1-346">Filter functions</span></span>

<span data-ttu-id="38ae1-347">Configuration veya Code tarafından kendisine atanmış kuralları olmayan tüm sağlayıcılar ve kategoriler için bir filtre işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-347">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="38ae1-348">İşlevindeki kodun sağlayıcı türü, kategorisi ve günlük düzeyine erişimi vardır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-348">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="38ae1-349">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="38ae1-349">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="38ae1-350">Bazı günlük sağlayıcıları, günlüklerin bir depolama ortamına ne zaman yazılacağını veya günlük düzeyi ve kategorisine göre yoksayılacağını belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-350">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="38ae1-351">`AddConsole` Ve`AddDebug` genişletme yöntemleri, filtreleme ölçütlerini kabul eden aşırı yüklemeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-351">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="38ae1-352">Aşağıdaki örnek kod, hata ayıklama sağlayıcısı Framework 'ün oluşturduğu günlükleri yok `Warning` sayırken, konsol sağlayıcısının düzeyi aşağıdaki günlükleri yoksaymasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="38ae1-352">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="38ae1-353">Yönteminde bir `EventLogSettings` örnek alan bir aşırı yükleme vardır ve bu, `Filter` özelliğinde bir filtreleme işlevi içerebilir. `AddEventLog`</span><span class="sxs-lookup"><span data-stu-id="38ae1-353">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="38ae1-354">Kendi günlük düzeyi ve diğer parametreleri temel aldığı ve `SourceSwitch` `TraceListener` kullandığı için, TraceSource sağlayıcısı bu aşırı yüklemelerin hiçbirini sağlamıyor.</span><span class="sxs-lookup"><span data-stu-id="38ae1-354">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="38ae1-355">Bir `ILoggerFactory` örnekle kayıtlı tüm sağlayıcıların filtreleme kurallarını ayarlamak için, `WithFilter` genişletme yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-355">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="38ae1-356">Aşağıdaki örnek, çerçeve günlüklerini kısıtlar (kategori, uygulama kodu tarafından oluşturulan Günlükler için hata ayıklama düzeyinde günlüğe yazılırken uyarılar için "Microsoft" veya "sistem" ile başlar).</span><span class="sxs-lookup"><span data-stu-id="38ae1-356">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="38ae1-357">Tüm günlüklerin yazılmasını engellemek için, en düşük günlük `LogLevel.None` düzeyi olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-357">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="38ae1-358">Öğesinin `LogLevel.None` tamsayı değeri 6 ' dır `LogLevel.Critical` (5) daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-358">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="38ae1-359">Genişletme yöntemi [Microsoft. Extensions. Logging. Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet paketi tarafından sağlanır. `WithFilter`</span><span class="sxs-lookup"><span data-stu-id="38ae1-359">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="38ae1-360">Yöntemi, ile kaydedilen tüm `ILoggerFactory` günlükçü sağlayıcılarına geçirilen günlük iletilerini filtreleyecek yeni bir örnek döndürür.</span><span class="sxs-lookup"><span data-stu-id="38ae1-360">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="38ae1-361">`ILoggerFactory` Özgün`ILoggerFactory` örnek dahil diğer örnekleri etkilemez.</span><span class="sxs-lookup"><span data-stu-id="38ae1-361">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="38ae1-362">Sistem kategorileri ve Düzeyler</span><span class="sxs-lookup"><span data-stu-id="38ae1-362">System categories and levels</span></span>

<span data-ttu-id="38ae1-363">ASP.NET Core ve Entity Framework Core tarafından kullanılan bazı kategoriler şunlardır ve bunlardan beklenen Günlükler hakkında notlar bulunur:</span><span class="sxs-lookup"><span data-stu-id="38ae1-363">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="38ae1-364">Kategori</span><span class="sxs-lookup"><span data-stu-id="38ae1-364">Category</span></span>                            | <span data-ttu-id="38ae1-365">Notlar</span><span class="sxs-lookup"><span data-stu-id="38ae1-365">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="38ae1-366">Microsoft. AspNetCore</span><span class="sxs-lookup"><span data-stu-id="38ae1-366">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="38ae1-367">Genel ASP.NET Core tanılama.</span><span class="sxs-lookup"><span data-stu-id="38ae1-367">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="38ae1-368">Microsoft. AspNetCore. DataProtection</span><span class="sxs-lookup"><span data-stu-id="38ae1-368">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="38ae1-369">Hangi anahtarların kabul edildiği, bulunduğu ve kullanıldığı.</span><span class="sxs-lookup"><span data-stu-id="38ae1-369">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="38ae1-370">Microsoft. AspNetCore. HostFiltering</span><span class="sxs-lookup"><span data-stu-id="38ae1-370">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="38ae1-371">İzin verilen konaklar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-371">Hosts allowed.</span></span> |
| <span data-ttu-id="38ae1-372">Microsoft. AspNetCore. Hosting</span><span class="sxs-lookup"><span data-stu-id="38ae1-372">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="38ae1-373">HTTP isteklerinin tamamlanması için geçen süre ve ne zaman başladıkları.</span><span class="sxs-lookup"><span data-stu-id="38ae1-373">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="38ae1-374">Hangi barındırma başlangıç derlemeleri yüklendi.</span><span class="sxs-lookup"><span data-stu-id="38ae1-374">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="38ae1-375">Microsoft. AspNetCore. Mvc</span><span class="sxs-lookup"><span data-stu-id="38ae1-375">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="38ae1-376">MVC ve Razor tanılama.</span><span class="sxs-lookup"><span data-stu-id="38ae1-376">MVC and Razor diagnostics.</span></span> <span data-ttu-id="38ae1-377">Model bağlama, filtre yürütme, derlemeyi görüntüleme, eylem seçimi.</span><span class="sxs-lookup"><span data-stu-id="38ae1-377">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="38ae1-378">Microsoft. AspNetCore. Routing</span><span class="sxs-lookup"><span data-stu-id="38ae1-378">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="38ae1-379">Eşleşen bilgileri yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-379">Route matching information.</span></span> |
| <span data-ttu-id="38ae1-380">Microsoft. AspNetCore. Server</span><span class="sxs-lookup"><span data-stu-id="38ae1-380">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="38ae1-381">Bağlantı başlatın, durdurun ve canlı yanıtları koruyun.</span><span class="sxs-lookup"><span data-stu-id="38ae1-381">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="38ae1-382">HTTPS sertifika bilgileri.</span><span class="sxs-lookup"><span data-stu-id="38ae1-382">HTTPS certificate information.</span></span> |
| <span data-ttu-id="38ae1-383">Microsoft. AspNetCore. StaticFiles</span><span class="sxs-lookup"><span data-stu-id="38ae1-383">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="38ae1-384">Sunulan dosyalar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-384">Files served.</span></span> |
| <span data-ttu-id="38ae1-385">Microsoft. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="38ae1-385">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="38ae1-386">Genel Entity Framework Core tanılama.</span><span class="sxs-lookup"><span data-stu-id="38ae1-386">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="38ae1-387">Veritabanı etkinliği ve yapılandırması, değişiklik algılama, geçişler.</span><span class="sxs-lookup"><span data-stu-id="38ae1-387">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="38ae1-388">Günlük kapsamları</span><span class="sxs-lookup"><span data-stu-id="38ae1-388">Log scopes</span></span>

 <span data-ttu-id="38ae1-389">*Kapsam* bir mantıksal işlemler kümesini gruplandırabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-389">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="38ae1-390">Bu gruplandırma, kümenin bir parçası olarak oluşturulan her günlüğe aynı verileri eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-390">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="38ae1-391">Örneğin, bir işlemin işlenmesi kapsamında oluşturulan her günlük işlem KIMLIĞI içerebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-391">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="38ae1-392">Kapsam, `IDisposable` <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> yöntemi tarafından döndürülen ve atılana kadar sürer olan bir türdür.</span><span class="sxs-lookup"><span data-stu-id="38ae1-392">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="38ae1-393">Bir `using` blok içinde günlükçü çağrılarını sarmalayarak kapsam kullanın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-393">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="38ae1-394">Aşağıdaki kod konsol sağlayıcısı için kapsamları etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-394">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="38ae1-395">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="38ae1-395">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="38ae1-396">Kapsam tabanlı günlüğe kaydetmeyi etkinleştirmek için konsolgünlükçüseçeneğininyapılandırılmasıgerekir.`IncludeScopes`</span><span class="sxs-lookup"><span data-stu-id="38ae1-396">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="38ae1-397">Yapılandırma hakkında bilgi için [yapılandırma](#configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-397">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="38ae1-398">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="38ae1-398">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="38ae1-399">Kapsam tabanlı günlüğe kaydetmeyi etkinleştirmek için konsolgünlükçüseçeneğininyapılandırılmasıgerekir.`IncludeScopes`</span><span class="sxs-lookup"><span data-stu-id="38ae1-399">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="38ae1-400">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="38ae1-400">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="38ae1-401">Her günlük iletisi kapsamlı bilgiler içerir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-401">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="38ae1-402">Yerleşik günlük oluşturma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="38ae1-402">Built-in logging providers</span></span>

<span data-ttu-id="38ae1-403">ASP.NET Core aşağıdaki sağlayıcıları sevk eder:</span><span class="sxs-lookup"><span data-stu-id="38ae1-403">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="38ae1-404">Console</span><span class="sxs-lookup"><span data-stu-id="38ae1-404">Console</span></span>](#console-provider)
* [<span data-ttu-id="38ae1-405">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="38ae1-405">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="38ae1-406">EventSource</span><span class="sxs-lookup"><span data-stu-id="38ae1-406">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="38ae1-407">EventLog</span><span class="sxs-lookup"><span data-stu-id="38ae1-407">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="38ae1-408">TraceSource</span><span class="sxs-lookup"><span data-stu-id="38ae1-408">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="38ae1-409">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="38ae1-409">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="38ae1-410">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="38ae1-410">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="38ae1-411">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="38ae1-411">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="38ae1-412">Stdout ve ASP.NET Core modüllü hata ayıklama günlüğü hakkında bilgi için bkz <xref:test/troubleshoot-azure-iis> . ve. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection></span><span class="sxs-lookup"><span data-stu-id="38ae1-412">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="38ae1-413">Konsol sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="38ae1-413">Console provider</span></span>

<span data-ttu-id="38ae1-414">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcı paketi, günlük çıktısını konsola gönderir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-414">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="38ae1-415">[Addconsole aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) , en az bir günlük düzeyi, bir filtre işlevi ve kapsamların desteklenip desteklenmediğini belirten bir Boole değeri geçirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-415">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="38ae1-416">Diğer bir seçenek de `IConfiguration` bir nesneyi geçirmektir ve bu da kapsamları destekler ve günlüğe kaydetme düzeylerini belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-416">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="38ae1-417">Konsol sağlayıcısı seçenekleri için bkz <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions>.</span><span class="sxs-lookup"><span data-stu-id="38ae1-417">For Console provider options, see <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions>.</span></span>

<span data-ttu-id="38ae1-418">Konsol sağlayıcısının performansı önemli ölçüde etkiler ve genellikle üretimde kullanım için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-418">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="38ae1-419">Visual Studio 'da yeni bir proje oluşturduğunuzda, `AddConsole` Yöntem şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="38ae1-419">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="38ae1-420">Bu kod, `Logging` *appSettings. JSON* dosyasının bölümüne başvurur:</span><span class="sxs-lookup"><span data-stu-id="38ae1-420">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="38ae1-421">Gösterilen ayarlar, [günlük filtreleme](#log-filtering) bölümünde açıklandığı gibi uygulamanın hata ayıklama düzeyinde oturum açmasına izin verirken çerçeveyi uyarılarla günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="38ae1-421">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="38ae1-422">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="38ae1-422">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="38ae1-423">Konsol günlüğü çıkışını görmek için proje klasöründe bir komut istemi açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-423">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="38ae1-424">Hata ayıklama sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="38ae1-424">Debug provider</span></span>

<span data-ttu-id="38ae1-425">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcı paketi, [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) sınıfını (`Debug.WriteLine` Yöntem çağrıları) kullanarak günlük çıktısını yazar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-425">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="38ae1-426">Linux 'ta, bu sağlayıcı günlükleri */var/log/Message*dosyasına yazar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-426">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="38ae1-427">[Adddebug aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) , en az bir günlük düzeyinde veya bir filtre işlevinde geçiş yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-427">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="38ae1-428">EventSource sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="38ae1-428">EventSource provider</span></span>

<span data-ttu-id="38ae1-429">ASP.NET Core 1.1.0 veya üzeri bir sürümü hedefleyen uygulamalar için, [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcı paketi olay izlemeyi uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-429">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="38ae1-430">Windows üzerinde [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)kullanır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-430">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="38ae1-431">Sağlayıcı platformlar arası, ancak Linux veya macOS için henüz bir olay koleksiyonu ve görüntüleme aracı yok.</span><span class="sxs-lookup"><span data-stu-id="38ae1-431">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="38ae1-432">Günlükleri toplamanın ve görüntülemenin iyi bir yolu, [PerfView yardımcı programını](https://github.com/Microsoft/perfview)kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-432">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="38ae1-433">ETW günlüklerini görüntülemeye yönelik başka araçlar da mevcuttur, ancak PerfView, ASP.NET tarafından yayınlanan ETW olaylarıyla çalışmak için en iyi deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-433">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="38ae1-434">Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView 'ı yapılandırmak için, dizeyi `*Microsoft-Extensions-Logging` **ek sağlayıcılar** listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-434">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="38ae1-435">(Dizenin başlangıcında yıldız işaretini kaçırmayın.)</span><span class="sxs-lookup"><span data-stu-id="38ae1-435">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView ek sağlayıcıları](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="38ae1-437">Windows olay günlüğü sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="38ae1-437">Windows EventLog provider</span></span>

<span data-ttu-id="38ae1-438">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcı paketi, Windows olay günlüğüne günlük çıktısı gönderir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-438">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="38ae1-439">[AddEventLog aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) , en az `EventLogSettings` bir günlük düzeyinde geçiş yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-439">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="38ae1-440">TraceSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="38ae1-440">TraceSource provider</span></span>

<span data-ttu-id="38ae1-441">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcı paketi <xref:System.Diagnostics.TraceSource> kitaplıklarını ve sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-441">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

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

<span data-ttu-id="38ae1-442">[Addtracesource aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) , bir kaynak anahtarı ve bir izleme dinleyicisi geçirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-442">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="38ae1-443">Bu sağlayıcıyı kullanmak için, bir uygulamanın .NET Framework çalışması gerekir (.NET Core yerine).</span><span class="sxs-lookup"><span data-stu-id="38ae1-443">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="38ae1-444">Sağlayıcı, iletileri örnek uygulamada <xref:System.Diagnostics.TextWriterTraceListener> kullanılan gibi çeşitli [dinleyicilerine](/dotnet/framework/debug-trace-profile/trace-listeners)yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-444">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="38ae1-445">Aşağıdaki örnek, konsol penceresine `TraceSource` ve daha yüksek `Warning` iletileri günlüğe kaydeden bir sağlayıcıyı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-445">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="38ae1-446">Azure App Service sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="38ae1-446">Azure App Service provider</span></span>

<span data-ttu-id="38ae1-447">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcı paketi, günlükleri bir Azure App Service uygulamasının dosya sistemindeki metin dosyalarına ve bir Azure depolama hesabındaki [BLOB depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) alanına yazar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-447">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="38ae1-448">Sağlayıcı paketi, .NET Core 1,1 veya üstünü hedefleyen uygulamalar tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-448">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="38ae1-449">.NET Core hedefleniyorsa aşağıdaki noktalara göz önünde kalın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-449">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="38ae1-450">Sağlayıcı paketi [Microsoft. AspNetCore. All metapackage](xref:fundamentals/metapackage)ASP.NET Core dahildir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-450">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="38ae1-451">Sağlayıcı paketi [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-451">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="38ae1-452">Sağlayıcıyı kullanmak için paketini yükledikten sonra.</span><span class="sxs-lookup"><span data-stu-id="38ae1-452">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="38ae1-453">.NET Framework veya `Microsoft.AspNetCore.App` metapackage 'e başvurabiliyorsa, sağlayıcı paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-453">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="38ae1-454">Çağır `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="38ae1-454">Invoke `AddAzureWebAppDiagnostics`:</span></span>

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

<span data-ttu-id="38ae1-455">Aşırı yükleme, <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>geçiş yapmanızı sağlar. <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*></span><span class="sxs-lookup"><span data-stu-id="38ae1-455">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="38ae1-456">Ayarlar nesnesi, günlük çıkış şablonu, blob adı ve dosya boyutu sınırı gibi varsayılan ayarları geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-456">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="38ae1-457">(*Çıktı şablonu* , bir `ILogger` yöntem çağrısıyla birlikte sağlandığının yanı sıra tüm günlüklere uygulanan bir ileti şablonudur.)</span><span class="sxs-lookup"><span data-stu-id="38ae1-457">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="38ae1-458">Sağlayıcı ayarlarını yapılandırmak için aşağıdaki örnekte <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> gösterildiği <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>gibi ve kullanın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="38ae1-459">App Service uygulamasına dağıtırken, uygulama, Azure portal **App Service** sayfasının [App Service Günlükler](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) bölümündeki ayarları kabul eder.</span><span class="sxs-lookup"><span data-stu-id="38ae1-459">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="38ae1-460">Aşağıdaki ayarlar güncelleştirilirken, değişiklikler uygulamanın yeniden başlatılmasını veya yeniden dağıtımı gerekmeden hemen etkili olur.</span><span class="sxs-lookup"><span data-stu-id="38ae1-460">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="38ae1-461">**Uygulama günlüğü (dosya sistemi)**</span><span class="sxs-lookup"><span data-stu-id="38ae1-461">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="38ae1-462">**Uygulama günlüğü (blob)**</span><span class="sxs-lookup"><span data-stu-id="38ae1-462">**Application Logging (Blob)**</span></span>

<span data-ttu-id="38ae1-463">Günlük dosyaları için varsayılan konum *D:\\Home\\LogFiles\\uygulama* klasöründedir ve varsayılan dosya adı *Diagnostics-YYYYMMDD. txt*' dir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-463">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="38ae1-464">Varsayılan dosya boyutu sınırı 10 MB 'tır ve tutulan varsayılan en fazla dosya sayısı 2 ' dir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-464">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="38ae1-465">Varsayılan blob adı *{app-name} {timestamp}/yyyy/mm/dd/ss/{Guid}-ApplicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="38ae1-465">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="38ae1-466">Sağlayıcı yalnızca proje Azure ortamında çalıştırıldığında çalışır.</span><span class="sxs-lookup"><span data-stu-id="38ae1-466">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="38ae1-467">Proje yerel olarak&mdash;çalıştırıldığında, yerel dosyalara veya blob 'lar için yerel geliştirme depolamasına yazmazsa, bu, hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-467">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="38ae1-468">Azure günlük akışı</span><span class="sxs-lookup"><span data-stu-id="38ae1-468">Azure log streaming</span></span>

<span data-ttu-id="38ae1-469">Azure günlük akışı, günlük etkinliklerini gerçek zamanlı olarak görüntülemenize izin verir:</span><span class="sxs-lookup"><span data-stu-id="38ae1-469">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="38ae1-470">Uygulama sunucusu</span><span class="sxs-lookup"><span data-stu-id="38ae1-470">The app server</span></span>
* <span data-ttu-id="38ae1-471">Web sunucusu</span><span class="sxs-lookup"><span data-stu-id="38ae1-471">The web server</span></span>
* <span data-ttu-id="38ae1-472">Başarısız istek izleme</span><span class="sxs-lookup"><span data-stu-id="38ae1-472">Failed request tracing</span></span>

<span data-ttu-id="38ae1-473">Azure günlük akışını yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="38ae1-473">To configure Azure log streaming:</span></span>

* <span data-ttu-id="38ae1-474">Uygulamanızın Portal sayfasından **App Service günlükleri** sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-474">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="38ae1-475">**Uygulama günlüğünü (FileSystem)** **Açık**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-475">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="38ae1-476">Günlük **düzeyini**seçin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-476">Choose the log **Level**.</span></span>

<span data-ttu-id="38ae1-477">Uygulama iletilerini görüntülemek için **günlük akışı** sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-477">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="38ae1-478">Uygulama tarafından `ILogger` arabirim aracılığıyla günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-478">They're logged by the app through the `ILogger` interface.</span></span>

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="38ae1-479">Azure Application Insights izleme günlüğü</span><span class="sxs-lookup"><span data-stu-id="38ae1-479">Azure Application Insights trace logging</span></span>

<span data-ttu-id="38ae1-480">[Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) sağlayıcı paketi günlükleri Azure Application Insights yazar.</span><span class="sxs-lookup"><span data-stu-id="38ae1-480">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="38ae1-481">Application Insights, bir Web uygulamasını izleyen ve telemetri verilerini sorgulamak ve analiz etmek için araçlar sağlayan bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-481">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="38ae1-482">Bu sağlayıcıyı kullanıyorsanız, Application Insights araçlarını kullanarak günlüklerinizi sorgulayabilir ve analiz edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-482">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="38ae1-483">Günlüğe kaydetme sağlayıcısı, ASP.NET Core için tüm kullanılabilir telemetri sağlayan paket olan [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore)'un bağımlılığı olarak eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-483">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="38ae1-484">Bu paketi kullanırsanız, sağlayıcı paketini yüklemek zorunda kalmazsınız.</span><span class="sxs-lookup"><span data-stu-id="38ae1-484">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="38ae1-485">ASP.NET 4. x için [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) paketini&mdash;kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-485">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="38ae1-486">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="38ae1-486">For more information, see the following resources:</span></span>

* [<span data-ttu-id="38ae1-487">Application Insights genel bakış</span><span class="sxs-lookup"><span data-stu-id="38ae1-487">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="38ae1-488">[ASP.NET Core uygulamalar için Application Insights](/azure/azure-monitor/app/asp-net-core-no-visualstudio) -günlük kaydı ile birlikte Application Insights telemetrinin tam aralığını uygulamak istiyorsanız buraya başlayın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-488">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core-no-visualstudio) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="38ae1-489">[.NET Core ILogger günlükleri Için Applicationınsightsloggerprovider](/azure/azure-monitor/app/ilogger) -günlük sağlayıcısını Application Insights telemetri olmadan uygulamak istiyorsanız buraya başlayın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-489">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="38ae1-490">[Günlüğe kaydetme bağdaştırıcılarını Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span><span class="sxs-lookup"><span data-stu-id="38ae1-490">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* <span data-ttu-id="38ae1-491">Microsoft Learn sitede Application Insights SDK-etkileşimli öğreticisini [yükleyip başlatın](/learn/modules/instrument-web-app-code-with-application-insights) .</span><span class="sxs-lookup"><span data-stu-id="38ae1-491">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>
::: moniker-end

> [!NOTE]
> <span data-ttu-id="38ae1-492">5/1/2019 itibariyle, [ASP.NET Core için Application Insights](/azure/azure-monitor/app/asp-net-core) başlıklı makale güncel değildir ve öğretici adımları çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-492">As of 5/1/2019, the article titled [Application Insights for ASP.NET Core](/azure/azure-monitor/app/asp-net-core) is out of date, and the tutorial steps don't work.</span></span> <span data-ttu-id="38ae1-493">Bunun yerine [ASP.NET Core uygulamalar için Application Insights](/azure/azure-monitor/app/asp-net-core-no-visualstudio) bakın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-493">Refer to [Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core-no-visualstudio) instead.</span></span> <span data-ttu-id="38ae1-494">Sorunun farkındayız ve düzeltmek için çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="38ae1-494">We are aware of the issue and are working to correct it.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="38ae1-495">Üçüncü taraf günlük oluşturma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="38ae1-495">Third-party logging providers</span></span>

<span data-ttu-id="38ae1-496">ASP.NET Core ile birlikte çalışan üçüncü taraf günlük çerçeveleri:</span><span class="sxs-lookup"><span data-stu-id="38ae1-496">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="38ae1-497">[ELMAH.io](https://elmah.io/) ([GitHub deposu](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="38ae1-497">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="38ae1-498">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposu](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="38ae1-498">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="38ae1-499">[Jsnlog](https://jsnlog.com/) ([GitHub deposu](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="38ae1-499">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="38ae1-500">[KissLog.net](https://kisslog.net/) ([GitHub deposu](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="38ae1-500">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="38ae1-501">[Loggr](https://loggr.net/) ([GitHub deposu](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="38ae1-501">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="38ae1-502">[NLog](https://nlog-project.org/) ([GitHub deposu](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="38ae1-502">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="38ae1-503">[Sentry](https://sentry.io/welcome/) ([GitHub deposu](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="38ae1-503">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="38ae1-504">[Serilog](https://serilog.net/) ([GitHub deposu](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="38ae1-504">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="38ae1-505">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub deposu](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="38ae1-505">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="38ae1-506">Bazı üçüncü taraf çerçeveler [, yapılandırılmış günlük olarak da bilinen anlam günlüğe kaydetme](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)işlemini gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="38ae1-506">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="38ae1-507">Bir üçüncü taraf çerçevesinin kullanılması, yerleşik sağlayıcılardan birini kullanmaya benzer:</span><span class="sxs-lookup"><span data-stu-id="38ae1-507">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="38ae1-508">Projenize bir NuGet paketi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38ae1-508">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="38ae1-509">Bir `ILoggerFactory`çağırın.</span><span class="sxs-lookup"><span data-stu-id="38ae1-509">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="38ae1-510">Daha fazla bilgi için bkz. her sağlayıcının belgeleri.</span><span class="sxs-lookup"><span data-stu-id="38ae1-510">For more information, see each provider's documentation.</span></span> <span data-ttu-id="38ae1-511">Üçüncü taraf günlüğü sağlayıcıları Microsoft tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="38ae1-511">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="38ae1-512">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="38ae1-512">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
