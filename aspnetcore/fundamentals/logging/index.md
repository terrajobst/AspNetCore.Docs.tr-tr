---
title: ASP.NET core'da günlüğe kaydetme
author: ardalis
description: ASP.NET core'da günlüğe kaydetme çerçevesi hakkında bilgi edinin. Yerleşik günlük sağlayıcıları bulmak ve popüler üçüncü taraf sağlayıcılar hakkında daha fazla bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/logging/index
ms.openlocfilehash: 38a395a97e9a0b7ccb0bfef0d1947ef379bf748c
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41886768"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="ec1b3-104">ASP.NET core'da günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="ec1b3-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="ec1b3-105">Tarafından [Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ec1b3-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ec1b3-106">ASP.NET Core çeşitli günlük sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="ec1b3-107">Bir veya daha fazla hedefe günlükleri gönderme yerleşik sağlayıcılar sağlar ve bir üçüncü taraf günlüğe kaydetme çerçevesi içinde takabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="ec1b3-108">Bu makalede, yerleşik günlüğe kaydetme API'si ve sağlayıcıları kodunuzda nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

<span data-ttu-id="ec1b3-109">IIS ile barındırırken stdout günlüğe kaydetme hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-109">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="ec1b3-110">Azure App Service stdout günlüğe kaydetme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-110">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

<span data-ttu-id="ec1b3-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ec1b3-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="ec1b3-112">Günlükleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="ec1b3-112">How to create logs</span></span>

<span data-ttu-id="ec1b3-113">Günlükleri oluşturmak için uygulama bir [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) nesnesinden [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-113">To create logs, implement an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="ec1b3-114">Ardından bu Günlükçü nesnede günlük yöntemlerini çağırın:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-114">Then call logging methods on that logger object:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="ec1b3-115">Bu örnek ile günlükleri oluşturur `TodoController` olarak sınıf *kategori*.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-115">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="ec1b3-116">Kategorileri açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#log-category).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-116">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="ec1b3-117">Günlük zaman uyumsuz kullanma maliyeti karşılıyor olmadığını kadar hızlı olması gerektiğinden, ASP.NET Core Günlükçü yöntemleri zaman uyumsuz sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-117">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="ec1b3-118">Burada, geçerli olmayan bir durumda kullanıyorsanız oturum şekilde değiştirmeyi göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-118">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="ec1b3-119">Data store yavaşsa, günlük iletilerini önce hızlı bir depoya yazmak ve sonra bunları daha sonra yavaş deposuna taşıyın.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-119">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="ec1b3-120">Örneğin, okuma ve başka bir işlem tarafından yavaş depolama için kalıcı bir ileti kuyruğu oturum açın.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-120">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="ec1b3-121">Sağlayıcıları ekleme</span><span class="sxs-lookup"><span data-stu-id="ec1b3-121">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ec1b3-122">Oturum açma sağlayıcısı ile oluşturduğunuz iletileri alan bir `ILogger` nesnesini görüntüler ve bunları depolar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-122">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="ec1b3-123">Örneğin, konsolu sağlayıcısı iletileri konsolda görüntüler ve Azure App Service sağlayıcısı Azure blob depolama alanında depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-123">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="ec1b3-124">Bir sağlayıcı kullanmak için sağlayıcının çağrı `Add<ProviderName>` uzantı yönteminde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-124">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="ec1b3-125">Konsol ve hata ayıklama günlüğü sağlayıcıları çağrısıyla varsayılan proje şablonu sağlar [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) uzantı yönteminde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-125">The default project template enables the Console and Debug logging providers with a call to the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ec1b3-126">Oturum açma sağlayıcısı ile oluşturduğunuz iletileri alan bir `ILogger` nesnesini görüntüler ve bunları depolar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-126">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="ec1b3-127">Örneğin, konsolu sağlayıcısı iletileri konsolda görüntüler ve Azure App Service sağlayıcısı Azure blob depolama alanında depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-127">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="ec1b3-128">Bir sağlayıcı kullanmak için kendi NuGet paketini yükleyin ve bir örneği üzerinde sağlayıcının uzantı metodu çağırma [Iloggerfactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory)aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-128">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="ec1b3-129">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection) sağlar (dı) `ILoggerFactory` örneği.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-129">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="ec1b3-130">`AddConsole` Ve `AddDebug` genişletme yöntemleri tanımlanmış [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) ve [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paketleri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-130">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="ec1b3-131">Her bir genişletme yöntemi çağıran `ILoggerFactory.AddProvider` sağlayıcının bir örneğini geçirerek yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-131">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="ec1b3-132">[1.x örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) günlük sağlayıcıları ekler `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-132">The [1.x sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="ec1b3-133">Daha önce yürüten koddan günlük çıkış elde etmek istiyorsanız, günlüğe kaydetme hizmeti sağlayıcıları Ekle `Startup` sınıf oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-133">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="ec1b3-134">Daha fazla bilgi edinin [yerleşik günlük sağlayıcıları](#built-in-logging-providers) ve bağlantılarda bulabilirsiniz [üçüncü taraf günlük sağlayıcıları](#third-party-logging-providers) makalenin ilerleyen bölümlerinde.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-134">Learn more about the [built-in logging providers](#built-in-logging-providers) and find links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="configuration"></a><span data-ttu-id="ec1b3-135">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ec1b3-135">Configuration</span></span>

<span data-ttu-id="ec1b3-136">Sağlayıcı Yapılandırması günlüğe kaydetme, bir veya daha fazla yapılandırma sağlayıcıları tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-136">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="ec1b3-137">Dosya biçimleri (INI, JSON ve XML).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-137">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="ec1b3-138">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-138">Command-line arguments.</span></span>
* <span data-ttu-id="ec1b3-139">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-139">Environment variables.</span></span>
* <span data-ttu-id="ec1b3-140">Bellek içi .NET nesneleri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-140">In-memory .NET objects.</span></span>
* <span data-ttu-id="ec1b3-141">Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolama.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-141">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="ec1b3-142">Gibi bir şifrelenmiş kullanıcı depolamak [Azure anahtar kasası](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-142">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="ec1b3-143">Özel sağlayıcılar (veya oluşturulan yüklü).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-143">Custom providers (installed or created).</span></span>

<span data-ttu-id="ec1b3-144">Örneğin, günlük kaydı yapılandırması sık tarafından sağlanan `Logging` uygulama ayarları dosyaları bölümü.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-144">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="ec1b3-145">Aşağıdaki örnek, tipik bir içeriğini gösterir *appsettings. Development.JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-145">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="ec1b3-146">`LogLevel` anahtarları günlük adlarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-146">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="ec1b3-147">`Default` Anahtarı açıkça listelenen günlükler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-147">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="ec1b3-148">Değeri temsil [günlük düzeyi](#log-level) verilen günlüğe uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-148">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="ec1b3-149">Günlük anahtarları küme `IncludeScopes` (`Console` örnekte), belirtin [oturum kapsamları](#log-scopes) belirtilen günlük için etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-149">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="ec1b3-150">`LogLevel` anahtarları günlük adlarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-150">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="ec1b3-151">`Default` Anahtarı açıkça listelenen günlükler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-151">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="ec1b3-152">Değeri temsil [günlük düzeyi](#log-level) verilen günlüğe uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-152">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="ec1b3-153">Uygulama yapılandırma sağlayıcıları hakkında daha fazla bilgi için bkz: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-153">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="ec1b3-154">Örnek günlük çıktısı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-154">Sample logging output</span></span>

<span data-ttu-id="ec1b3-155">Önceki bölümde gösterilen örnek kod ile komut satırından çalıştırdığınızda, konsol günlüklerine görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-155">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="ec1b3-156">Konsol çıktının bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-156">Here's an example of console output:</span></span>

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

<span data-ttu-id="ec1b3-157">Bu günlükler giderek oluşturulan `http://localhost:5000/api/todo/0`, her ikisi de yürütülmesini tetikler `ILogger` çağrıları önceki bölümde gösterilen.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-157">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="ec1b3-158">Visual Studio'da örnek uygulamayı çalıştırdığınızda hata ayıklama penceresinde göründükleri gibi aynı günlüklerinin bir örnek aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-158">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="ec1b3-159">Tarafından oluşturulan günlükleri `ILogger` çağrıları önceki bölümde gösterilen "TodoApi.Controllers.TodoController" ile başlar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-159">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="ec1b3-160">"Microsoft" Kategoriler ile başlayan ASP.NET Core günlüklerdir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-160">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="ec1b3-161">ASP.NET Core kendisi ve uygulama kodunuz aynı günlüğe kaydetme API'si ve aynı günlük sağlayıcıları kullanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-161">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="ec1b3-162">Bu makalenin geri kalanında, bazı ayrıntılar ve günlüğe kaydetme seçeneklerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-162">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="ec1b3-163">NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="ec1b3-163">NuGet packages</span></span>

<span data-ttu-id="ec1b3-164">`ILogger` Ve `ILoggerFactory` arabirimdir içinde [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ve bunlar için varsayılan uygulamalarıdır içinde [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-164">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="ec1b3-165">Günlük kategorisi</span><span class="sxs-lookup"><span data-stu-id="ec1b3-165">Log category</span></span>

<span data-ttu-id="ec1b3-166">A *kategori* oluşturduğunuz her bir günlük dahildir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-166">A *category* is included with each log that you create.</span></span> <span data-ttu-id="ec1b3-167">Kategori oluşturduğunuzda, belirttiğiniz bir `ILogger` nesne.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-167">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="ec1b3-168">Kategori herhangi bir dize olabilir, ancak günlükler yazıldığı sınıfın tam adını kullanmak için bir kuraldır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-168">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="ec1b3-169">Örneğin: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="ec1b3-169">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="ec1b3-170">Kategori bir dize olarak belirtebilir veya kategori türünden türetilen bir genişletme yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-170">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="ec1b3-171">Bir dize olarak kategorisini belirtmek için çağrı `CreateLogger` üzerinde bir `ILoggerFactory` , aşağıda gösterildiği gibi örnek.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-171">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="ec1b3-172">Çoğu zaman, kullanımı daha kolay olacak `ILogger<T>`, aşağıdaki örnekte olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-172">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="ec1b3-173">Bu çağırmakla eşdeğerdir `CreateLogger` tam olarak nitelenmiş tür adını `T`.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-173">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="ec1b3-174">Günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="ec1b3-174">Log level</span></span>

<span data-ttu-id="ec1b3-175">Her bir günlük yazma, belirttiğiniz kendi [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-175">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="ec1b3-176">Günlük düzeyini önem derecesi veya önem derecesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-176">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="ec1b3-177">Örneğin yazabilirsiniz bir `Information` oturum normalde, bir yöntem sona erdiğinde bir `Warning` bir yöntem dönüş kodu 404 hatası döndürdüğünde ve bir günlük `Error` günlüğe beklenmeyen bir özel durum yakalayın.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-177">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="ec1b3-178">Aşağıdaki kod örneğinde, yöntemlerin adlarını (örneğin, `LogWarning`) günlük düzeyini belirtin.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-178">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="ec1b3-179">İlk parametre [oturum öğesini belirten Olay No.](#log-event-id)</span><span class="sxs-lookup"><span data-stu-id="ec1b3-179">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="ec1b3-180">İkinci parametre bir [ileti şablonunu](#log-message-template) kalan yöntem parametreleri tarafından sağlanan bağımsız değişken değerleri yer tutucuları olan.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-180">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="ec1b3-181">Yöntem parametreleri, bu makalenin sonraki bölümlerinde daha ayrıntılı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-181">The method parameters are explained in more detail later in this article.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="ec1b3-182">Yöntem adında düzeyi günlük yöntemler [için ILogger genişletme yöntemleri](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-182">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="ec1b3-183">Arka planda bu yöntemleri çağırmak bir `Log` gereken yöntemini bir `LogLevel` parametresi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-183">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="ec1b3-184">Çağırabilirsiniz `Log` biri bu genişletme yöntemleri, ancak söz dizimi yerine doğrudan yöntemi nispeten karmaşık.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-184">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="ec1b3-185">Daha fazla bilgi için [ILogger arabirimi](/dotnet/api/microsoft.extensions.logging.ilogger) ve [Günlükçü uzantılarını kaynak kodu](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-185">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="ec1b3-186">ASP.NET Core, aşağıdaki tanımlar [günlük düzeyleri](/dotnet/api/microsoft.extensions.logging.loglevel), burada en çok yüksek derecesi sıralı.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-186">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="ec1b3-187">İzleme = 0</span><span class="sxs-lookup"><span data-stu-id="ec1b3-187">Trace = 0</span></span>

  <span data-ttu-id="ec1b3-188">Yalnızca bir geliştirici için değerli bilgiler için bir sorun hata ayıklama.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-188">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="ec1b3-189">Bu iletiler, uygulama hassas verileri içerebilir ve bir üretim ortamında bu nedenle etkin olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-189">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="ec1b3-190">*Varsayılan olarak devre dışıdır.*</span><span class="sxs-lookup"><span data-stu-id="ec1b3-190">*Disabled by default.*</span></span> <span data-ttu-id="ec1b3-191">Örnek: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="ec1b3-191">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="ec1b3-192">Hata ayıklama = 1</span><span class="sxs-lookup"><span data-stu-id="ec1b3-192">Debug = 1</span></span>

  <span data-ttu-id="ec1b3-193">Bilgi için geliştirme ve hata ayıklama sırasında kısa vadeli fayda vardır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-193">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="ec1b3-194">Örnek: `Entering method Configure with flag set to true.` genellikle etkinleştirme mıydı `Debug` düzeyi yüksek hacimli günlükleri nedeniyle giderirken sürece üretimde günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-194">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="ec1b3-195">Bilgi = 2</span><span class="sxs-lookup"><span data-stu-id="ec1b3-195">Information = 2</span></span>

  <span data-ttu-id="ec1b3-196">Uygulama'nın genel akışı izlemek için.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-196">For tracking the general flow of the application.</span></span> <span data-ttu-id="ec1b3-197">Bu günlükler genellikle uzun vadeli bir değer var.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-197">These logs typically have some long-term value.</span></span> <span data-ttu-id="ec1b3-198">Örnek: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="ec1b3-198">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="ec1b3-199">Uyarı = 3</span><span class="sxs-lookup"><span data-stu-id="ec1b3-199">Warning = 3</span></span>

  <span data-ttu-id="ec1b3-200">Uygulama akışındaki olağan dışı ya da beklenmeyen olaylar için.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-200">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="ec1b3-201">Bunlar, hata veya uygulamanın durdurmasına neden olmaz, ancak araştırılması gereken, diğer koşulları olabilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-201">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="ec1b3-202">İşlenmiş istisnaları kullanmak için ortak bir yerde `Warning` günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-202">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="ec1b3-203">Örnek: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="ec1b3-203">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="ec1b3-204">Hata = 4</span><span class="sxs-lookup"><span data-stu-id="ec1b3-204">Error = 4</span></span>

  <span data-ttu-id="ec1b3-205">Hatalar ve özel durumlar için işlenemez.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-205">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="ec1b3-206">Bu iletiler, geçerli etkinliği ya da (örneğin, geçerli HTTP isteği) işlemi bir hata, birçok farklı uygulama başarısızlığı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-206">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="ec1b3-207">Örnek günlük iletisi: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="ec1b3-207">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="ec1b3-208">Kritik = 5</span><span class="sxs-lookup"><span data-stu-id="ec1b3-208">Critical = 5</span></span>

  <span data-ttu-id="ec1b3-209">Hemen ilgilenilmesi gereken hataları.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-209">For failures that require immediate attention.</span></span> <span data-ttu-id="ec1b3-210">Örnekler: veri kaybı senaryolarına, disk alanı yetersiz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-210">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="ec1b3-211">Günlük düzeyi ne kadar günlük çıktısını bir belirli depolama ortamına yazılır denetlemek veya penceresini görüntülemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-211">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="ec1b3-212">Örneğin, üretim ortamında, tüm günlükleri isteyebileceğiniz `Information` düzeyi ve toplu veri deposu ve tüm günlüklerin Git alt `Warning` düzeyi ve daha yüksek bir değer veri deposu için Git.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-212">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="ec1b3-213">Geliştirme sırasında normalde günlüklerini gönderebilir `Warning` veya konsola daha yüksek önem derecesi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-213">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="ec1b3-214">Sorun giderme gerektiğinde ekleyebilirsiniz `Debug` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-214">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="ec1b3-215">[Günlük filtreleme](#log-filtering) bu makalenin devamındaki bölümüne bir sağlayıcı işleme hangi günlük düzeyleri denetlemek nasıl açıklar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-215">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="ec1b3-216">ASP.NET Core framework Yazar `Debug` düzey framework olayları için günlükleri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-216">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="ec1b3-217">Günlük örnekleri, bu makalede daha önce günlüklere hariç tutuldu. `Information` düzeyi, dolayısıyla `Debug` düzeyi günlükleri gösterilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-217">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="ec1b3-218">İşte bir örnek konsol günlüklerinin göstermek üzere yapılandırılmış örnek uygulamayı çalıştırırsanız `Debug` ve konsolu sağlayıcısı için daha yüksek günlükleri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-218">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="ec1b3-219">Günlük Olay Kimliği</span><span class="sxs-lookup"><span data-stu-id="ec1b3-219">Log event ID</span></span>

<span data-ttu-id="ec1b3-220">Her bir günlük yazma belirtebileceğiniz bir *öğesini belirten Olay No.*</span><span class="sxs-lookup"><span data-stu-id="ec1b3-220">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="ec1b3-221">Örnek uygulamayı yerel olarak tanımlanan bir kullanarak bunu yapar `LoggingEvents` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-221">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="ec1b3-222">Olay Kimliği birbiriyle günlüğe kaydedilen olayları kümesini ilişkilendirmek için kullanabileceğiniz bir tamsayı değerdir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-222">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="ec1b3-223">Örneğin, bir öğe, alışveriş sepetine eklemek için bir günlüğe olay kimliği 1000 olabilir ve bir satın alma siparişinin tamamlanması için bir günlük olay kimliği 1001 olabilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-223">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="ec1b3-224">Günlüğünü Çıkışta, olay kimliği bir alanda depolanmış veya olabilir sağlayıcısına bağlı olarak bir SMS mesajı dahil.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-224">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="ec1b3-225">Olay kimliklerini hata ayıklama sağlayıcısı göstermez ancak Konsolu sağlayıcısı bunları ayraçlar içine sonra kategori gösterir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-225">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="ec1b3-226">Günlük ileti şablonu</span><span class="sxs-lookup"><span data-stu-id="ec1b3-226">Log message template</span></span>

<span data-ttu-id="ec1b3-227">Bir günlük iletisine yazma her zaman bir ileti şablonu sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-227">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="ec1b3-228">İleti şablonunu bir dize olabilir veya adlandırılmış yer tutucu değerleri hangi bağımsız değişken yerleştirilir içerebilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-228">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="ec1b3-229">Şablon bir biçim dizesi değil ve yok numaralı yer tutucuları adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-229">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="ec1b3-230">Hangi parametrelerin değerlerini sağlamak için kullanılan yer tutucuları, bunların adları sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-230">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="ec1b3-231">Aşağıdaki koda sahipseniz:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-231">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="ec1b3-232">Elde edilen günlük iletisi şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-232">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="ec1b3-233">Günlüğe kaydetme çerçevesi uygulamak günlük sağlayıcıları için mümkün kılmak için bu şekilde biçimlendirme ileti [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-233">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="ec1b3-234">Biçimlendirilmiş ileti şablonu yalnızca günlüğe kaydetme sistem değişkenleri geçirildiğinden günlük sağlayıcıları alanlara ileti şablonunu ek olarak parametre değerleri depolayabilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-234">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="ec1b3-235">Azure tablo depolaması için çıktı günlüğünüzün yönlendiren ve Günlükçü yöntem çağrınız şöyle görünür değilse:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-235">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="ec1b3-236">Her Azure tablo varlığın `ID` ve `RequestTime` sorgu günlüğü verilerini kolaylaştıran özellikler.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-236">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="ec1b3-237">Belirli bir içindeki tüm günlükler bulabilirsiniz `RequestTime` kısa mesaj zaman aşımı ayrıştırma gerek kalmadan aralığı.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-237">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="ec1b3-238">Özel durumları</span><span class="sxs-lookup"><span data-stu-id="ec1b3-238">Logging exceptions</span></span>

<span data-ttu-id="ec1b3-239">Günlükçü yöntemleri aşağıdaki örnekteki gibi bir özel durum geçirmenize olanak tanıyan aşırı yüklemeleri vardır:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-239">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="ec1b3-240">Farklı sağlayıcıları, özel durum bilgilerini farklı yollarla işler.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-240">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="ec1b3-241">Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktının bir örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-241">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="ec1b3-242">Günlük Filtresi</span><span class="sxs-lookup"><span data-stu-id="ec1b3-242">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ec1b3-243">En düşük günlük düzeyi veya tüm sağlayıcıları ya da tüm kategorileri özel sağlayıcı ve kategori için belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-243">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="ec1b3-244">Bunlar görüntülenen veya depolanan olmayan şekilde en düşük düzeyin herhangi bir günlük bu sağlayıcısına geçirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-244">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="ec1b3-245">Tüm günlükler gizlemek istiyorsanız, belirtebilirsiniz `LogLevel.None` en düşük günlük düzeyi olarak.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-245">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="ec1b3-246">Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-246">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="ec1b3-247">Yapılandırma filtresi kuralları oluşturabilir</span><span class="sxs-lookup"><span data-stu-id="ec1b3-247">Create filter rules in configuration</span></span>

<span data-ttu-id="ec1b3-248">Çağıran kod proje şablonları oluşturma `CreateDefaultBuilder` konsol ve hata ayıklama sağlayıcıları için günlük kaydı ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-248">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="ec1b3-249">`CreateDefaultBuilder` Yöntemi ayrıca ayarlar günlüğünü yapılandırmasında aramak için bir `Logging` bölümünde, aşağıdaki gibi kod kullanarak:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-249">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="ec1b3-250">Yapılandırma verileri sağlayıcısı ve kategorisi, aşağıdaki örnekte olduğu gibi en az günlük düzeyleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-250">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="ec1b3-251">Bu JSON, bir hata ayıklama sağlayıcısı, konsolu sağlayıcısı için dört ve tüm sağlayıcılar için geçerli bir altı filtre kuralları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-251">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="ec1b3-252">Bu kurallar daha sonra nasıl yalnızca biri için her bir sağlayıcı seçilir görürsünüz, bir `ILogger` nesnesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-252">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="ec1b3-253">Kod içinde filtre kuralları</span><span class="sxs-lookup"><span data-stu-id="ec1b3-253">Filter rules in code</span></span>

<span data-ttu-id="ec1b3-254">Aşağıdaki örnekte gösterildiği gibi kodda filtre kuralları kaydedebilir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-254">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="ec1b3-255">İkinci `AddFilter` , tür adını kullanarak hata ayıklama sağlayıcıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-255">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="ec1b3-256">İlk `AddFilter` bir sağlayıcı türü belirtmeyen çünkü tüm sağlayıcılar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-256">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="ec1b3-257">Nasıl filtreleme kuralları uygulanır</span><span class="sxs-lookup"><span data-stu-id="ec1b3-257">How filtering rules are applied</span></span>

<span data-ttu-id="ec1b3-258">Yapılandırma verilerini ve `AddFilter` Yukarıdaki örneklerde gösterilen kod aşağıdaki tabloda gösterilen kuralları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-258">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="ec1b3-259">İlk altı yapılandırma örnekten gelmesi ve son iki kod örneğindeki gelir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-259">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="ec1b3-260">Sayı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-260">Number</span></span> | <span data-ttu-id="ec1b3-261">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-261">Provider</span></span>      | <span data-ttu-id="ec1b3-262">Şununla kategori...</span><span class="sxs-lookup"><span data-stu-id="ec1b3-262">Categories that begin with ...</span></span>          | <span data-ttu-id="ec1b3-263">En düşük günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="ec1b3-263">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="ec1b3-264">1.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-264">1</span></span>      | <span data-ttu-id="ec1b3-265">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-265">Debug</span></span>         | <span data-ttu-id="ec1b3-266">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="ec1b3-266">All categories</span></span>                          | <span data-ttu-id="ec1b3-267">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="ec1b3-267">Information</span></span>       |
| <span data-ttu-id="ec1b3-268">2</span><span class="sxs-lookup"><span data-stu-id="ec1b3-268">2</span></span>      | <span data-ttu-id="ec1b3-269">Konsol</span><span class="sxs-lookup"><span data-stu-id="ec1b3-269">Console</span></span>       | <span data-ttu-id="ec1b3-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="ec1b3-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="ec1b3-271">Uyarı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-271">Warning</span></span>           |
| <span data-ttu-id="ec1b3-272">3</span><span class="sxs-lookup"><span data-stu-id="ec1b3-272">3</span></span>      | <span data-ttu-id="ec1b3-273">Konsol</span><span class="sxs-lookup"><span data-stu-id="ec1b3-273">Console</span></span>       | <span data-ttu-id="ec1b3-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="ec1b3-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="ec1b3-275">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-275">Debug</span></span>             |
| <span data-ttu-id="ec1b3-276">4</span><span class="sxs-lookup"><span data-stu-id="ec1b3-276">4</span></span>      | <span data-ttu-id="ec1b3-277">Konsol</span><span class="sxs-lookup"><span data-stu-id="ec1b3-277">Console</span></span>       | <span data-ttu-id="ec1b3-278">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="ec1b3-278">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="ec1b3-279">Hata</span><span class="sxs-lookup"><span data-stu-id="ec1b3-279">Error</span></span>             |
| <span data-ttu-id="ec1b3-280">5</span><span class="sxs-lookup"><span data-stu-id="ec1b3-280">5</span></span>      | <span data-ttu-id="ec1b3-281">Konsol</span><span class="sxs-lookup"><span data-stu-id="ec1b3-281">Console</span></span>       | <span data-ttu-id="ec1b3-282">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="ec1b3-282">All categories</span></span>                          | <span data-ttu-id="ec1b3-283">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="ec1b3-283">Information</span></span>       |
| <span data-ttu-id="ec1b3-284">6</span><span class="sxs-lookup"><span data-stu-id="ec1b3-284">6</span></span>      | <span data-ttu-id="ec1b3-285">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="ec1b3-285">All providers</span></span> | <span data-ttu-id="ec1b3-286">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="ec1b3-286">All categories</span></span>                          | <span data-ttu-id="ec1b3-287">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-287">Debug</span></span>             |
| <span data-ttu-id="ec1b3-288">7</span><span class="sxs-lookup"><span data-stu-id="ec1b3-288">7</span></span>      | <span data-ttu-id="ec1b3-289">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="ec1b3-289">All providers</span></span> | <span data-ttu-id="ec1b3-290">Sistem</span><span class="sxs-lookup"><span data-stu-id="ec1b3-290">System</span></span>                                  | <span data-ttu-id="ec1b3-291">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-291">Debug</span></span>             |
| <span data-ttu-id="ec1b3-292">8</span><span class="sxs-lookup"><span data-stu-id="ec1b3-292">8</span></span>      | <span data-ttu-id="ec1b3-293">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-293">Debug</span></span>         | <span data-ttu-id="ec1b3-294">Microsoft</span><span class="sxs-lookup"><span data-stu-id="ec1b3-294">Microsoft</span></span>                               | <span data-ttu-id="ec1b3-295">İzleme</span><span class="sxs-lookup"><span data-stu-id="ec1b3-295">Trace</span></span>             |

<span data-ttu-id="ec1b3-296">Oluştururken bir `ILogger` , günlükleri yazmak için nesne `ILoggerFactory` nesne bu Günlükçü için uygulanacak sağlayıcı başına tek bir kural seçer.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-296">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="ec1b3-297">Tüm iletileri tarafından yazılan `ILogger` nesne seçili kuralları göre filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-297">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="ec1b3-298">Her bir sağlayıcı ve kategori çifti için olası en belirgin kural kullanılabilir kurallardan seçilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-298">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="ec1b3-299">Aşağıdaki algoritmadan her sağlayıcısı için kullanılan zaman bir `ILogger` için belirli bir kategori oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-299">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="ec1b3-300">Sağlayıcı veya diğer adıyla eşleşen tüm kuralları'nı seçin.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-300">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="ec1b3-301">Bulunursa, tüm kuralları ile boş bir sağlayıcı seçin.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-301">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="ec1b3-302">Önceki adımda sonuçtan seçme kuralları ile en uzun kategori ön ek eşleştirme.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-302">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="ec1b3-303">Bulunursa, bir kategori belirtmeyin tüm kuralları'nı seçin.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-303">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="ec1b3-304">Birden çok kural seçtiyseniz ele **son** biri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-304">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="ec1b3-305">Hiçbir kural seçtiyseniz, kullanın `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-305">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="ec1b3-306">Örneğin, önceki kuralların listesini varsa ve oluşturduğunuz düşünün bir `ILogger` nesne kategorisi "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" için:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-306">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="ec1b3-307">Hata ayıklama sağlayıcısı için 1, 6 ve 8 kuralları geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-307">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="ec1b3-308">Kural 8 en belirgin olduğundan, seçili durumdaki.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-308">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="ec1b3-309">Konsolu sağlayıcısı için 3, 4, 5 ve 6 kuralları geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-309">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="ec1b3-310">Kural 3 en belirgin değil.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-310">Rule 3 is most specific.</span></span>

<span data-ttu-id="ec1b3-311">Günlükleri ile oluşturduğunuzda bir `ILogger` "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" kategorisi için günlükleri `Trace` düzeyi ve hata ayıklama sağlayıcısı ve günlükleri için yukarıdaki geçer `Debug` düzeyi ve üzeri konsol sağlayıcıya geçer.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-311">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="ec1b3-312">Sağlayıcı diğer adları</span><span class="sxs-lookup"><span data-stu-id="ec1b3-312">Provider aliases</span></span>

<span data-ttu-id="ec1b3-313">Tür adı, yapılandırmada bir sağlayıcısını belirtmek için kullanabilirsiniz, ancak her sağlayıcı daha kısa bir tanımlar *diğer* kullanımı daha kolay olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-313">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="ec1b3-314">Yerleşik sağlayıcılar için aşağıdaki diğer adlar kullanın:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-314">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="ec1b3-315">Konsol</span><span class="sxs-lookup"><span data-stu-id="ec1b3-315">Console</span></span>
* <span data-ttu-id="ec1b3-316">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-316">Debug</span></span>
* <span data-ttu-id="ec1b3-317">Olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="ec1b3-317">EventLog</span></span>
* <span data-ttu-id="ec1b3-318">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="ec1b3-318">AzureAppServices</span></span>
* <span data-ttu-id="ec1b3-319">TraceSource</span><span class="sxs-lookup"><span data-stu-id="ec1b3-319">TraceSource</span></span>
* <span data-ttu-id="ec1b3-320">EventSource</span><span class="sxs-lookup"><span data-stu-id="ec1b3-320">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="ec1b3-321">Varsayılan en düşük düzeyi</span><span class="sxs-lookup"><span data-stu-id="ec1b3-321">Default minimum level</span></span>

<span data-ttu-id="ec1b3-322">Yalnızca yapılandırma veya kod hiçbir kural belirtilen sağlayıcı ve kategori için uygularsanız, etkinleşir en az bir düzeyi ayarı yoktur.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-322">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="ec1b3-323">Aşağıdaki örnek, en düşük düzeyi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-323">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="ec1b3-324">En düşük düzey açıkça ayarlamazsanız, varsayılan değer: `Information`, anlamına `Trace` ve `Debug` günlükleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-324">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="ec1b3-325">Filtre işlevleri</span><span class="sxs-lookup"><span data-stu-id="ec1b3-325">Filter functions</span></span>

<span data-ttu-id="ec1b3-326">Filtreleme kurallarını uygulamak için bir filtre işlevi kod yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-326">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="ec1b3-327">Tüm sağlayıcıları ve yapılandırma veya kod tarafından atanmış kural kategorisi için bir filtre işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-327">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="ec1b3-328">İşlev kodu, sağlayıcı türü, kategori ve günlük düzeyi bir ileti günlüğe olup olmadığına karar vermek için erişimi vardır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-328">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="ec1b3-329">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-329">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ec1b3-330">Bazı günlük sağlayıcıları, ne zaman günlükleri bir depolama ortamına veya göz ardı belirtin günlük düzeyi ve kategoriye göre olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-330">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="ec1b3-331">`AddConsole` Ve `AddDebug` genişletme yöntemleri, filtre ölçütlerinde geçirmenize olanak tanıyan aşırı yükler sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-331">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="ec1b3-332">Aşağıdaki örnek kod konsol sağlayıcı günlüklere yok saymak neden `Warning` düzey sırasında hata ayıklama sağlayıcısı framework oluşturduğu günlükleri yok sayar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-332">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="ec1b3-333">`AddEventLog` Yöntemi olan alan bir aşırı yüklemesini bir `EventLogSettings` filtreleme bir işlevde içerebilen örneği kendi `Filter` özelliği.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-333">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="ec1b3-334">Kendi günlük düzeyi ve diğer parametrelere dayanır beri TraceSource sağlayıcısı bu aşırı yüklemeler hiçbirini sağlamaz `SourceSwitch` ve `TraceListener` kullanır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-334">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="ec1b3-335">Filtreleme kuralları ile kayıtlı tüm sağlayıcılar için ayarlayabileceğiniz bir `ILoggerFactory` kullanarak örneği `WithFilter` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-335">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="ec1b3-336">Aşağıdaki örnekte, uygulamada oturum hata ayıklama düzeyinde izin verirken framework günlüklerine uyarıları (kategorisi "Microsoft" veya "Sistem" ile başlayan) sınırlar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-336">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="ec1b3-337">Belirtebileceğiniz filtreleme için belirli bir kategori yazılan tüm günlükler önlemek için kullanmak istiyorsanız, `LogLevel.None` olarak bu kategori için en düşük günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-337">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="ec1b3-338">Tamsayı değerini `LogLevel.None` daha yüksek olduğu 6 olduğu `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-338">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="ec1b3-339">`WithFilter` Genişletme yöntemi tarafından sağlanır [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-339">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="ec1b3-340">Yeni bir yöntem döndürür `ILoggerFactory` tüm Günlükçü sağlayıcıları için geçirilen günlük iletileri filtreler örneği ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-340">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="ec1b3-341">Diğer etkilemez `ILoggerFactory` örnekleri, özgün dahil olmak üzere `ILoggerFactory` örneği.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-341">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="ec1b3-342">Günlük kapsamları</span><span class="sxs-lookup"><span data-stu-id="ec1b3-342">Log scopes</span></span>

<span data-ttu-id="ec1b3-343">Bir dizi içinde mantıksal işlemlerini gruplandırabilirsiniz bir *kapsam* bu kümesinin bir parçası oluşturulan her oturum için aynı verileri iliştirmek için.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-343">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="ec1b3-344">Örneğin, işlem kimliği eklemek için bir işlem işleme bir parçası olarak oluşturulan her günlük isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-344">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="ec1b3-345">Bir kapsam bir `IDisposable` tarafından döndürülen tür [ILogger.BeginScope&lt;TState&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) yöntemi ve bırakılana kadar bağlanabilmelerini.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-345">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="ec1b3-346">Bir kapsam, Günlükçü çağrıları sarmalama tarafından kullandığınız bir `using` burada gösterildiği gibi engelleme:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-346">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="ec1b3-347">Aşağıdaki kod, konsolu sağlayıcısı için kapsamları etkinleştirir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-347">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="ec1b3-348">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-348">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="ec1b3-349">Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-349">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="ec1b3-350">Yapılandırması hakkında daha fazla bilgi için bkz: [yapılandırma](#configuration) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-350">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ec1b3-351">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-351">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="ec1b3-352">Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-352">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ec1b3-353">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-353">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="ec1b3-354">Her günlük iletisi kapsamlı bilgiler içerir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-354">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="ec1b3-355">Yerleşik günlük sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="ec1b3-355">Built-in logging providers</span></span>

<span data-ttu-id="ec1b3-356">ASP.NET Core aşağıdaki sağlayıcıları birlikte gelir:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-356">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="ec1b3-357">Console</span><span class="sxs-lookup"><span data-stu-id="ec1b3-357">Console</span></span>](#console-provider)
* [<span data-ttu-id="ec1b3-358">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-358">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="ec1b3-359">EventSource</span><span class="sxs-lookup"><span data-stu-id="ec1b3-359">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="ec1b3-360">EventLog</span><span class="sxs-lookup"><span data-stu-id="ec1b3-360">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="ec1b3-361">TraceSource</span><span class="sxs-lookup"><span data-stu-id="ec1b3-361">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="ec1b3-362">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="ec1b3-362">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="ec1b3-363">Konsolu sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-363">Console provider</span></span>

<span data-ttu-id="ec1b3-364">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcı paketi konsola günlük çıktısını gönderir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-364">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="ec1b3-365">[AddConsole aşırı](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) geçirdiğiniz olanak veren bir en düşük günlük düzeyi, bir filtre işlevi ve kapsamları desteklenip desteklenmediğini belirten bir Boole değeri.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-365">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="ec1b3-366">Başka bir seçenek de geçirmektir bir `IConfiguration` kapsamları desteği ve günlüğe kaydetme düzeylerini belirleyebilirsiniz nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-366">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="ec1b3-367">Üretim amaçlı kullanım için konsolu sağlayıcısı düşünüyorsanız, performansı üzerinde önemli bir etkisi olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-367">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="ec1b3-368">Visual Studio'da yeni bir proje oluşturduğunuzda `AddConsole` yöntemi aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-368">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="ec1b3-369">Bu kod başvurduğu `Logging` bölümünü *appSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-369">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="ec1b3-370">Hata ayıklama düzeyinde açıklandığı şekilde oturum sınırı framework günlükleri uyarılar için uygulama izin verirken gösterilen ayarları [günlük filtreleme](#log-filtering) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-370">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="ec1b3-371">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-371">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="ec1b3-372">Sağlayıcı hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="ec1b3-372">Debug provider</span></span>

<span data-ttu-id="ec1b3-373">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcı paketi kullanarak günlük çıktı Yazar [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) sınıfı (`Debug.WriteLine` yöntem çağrılarını).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-373">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="ec1b3-374">Linux üzerinde bu sağlayıcı günlüklerine Yazar */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-374">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="ec1b3-375">[AddDebug aşırı](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) en düşük günlük düzeyi veya bir filtre işlevi geçirdiğiniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-375">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="ec1b3-376">EventSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-376">EventSource provider</span></span>

<span data-ttu-id="ec1b3-377">ASP.NET Core 1.1.0 hedefleyen uygulamalar için veya sonraki bir sürümü [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcı paketi, olay izleme uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-377">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="ec1b3-378">Windows üzerinde kullandığı [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-378">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="ec1b3-379">Platformlar arası sağlayıcısıdır, ancak henüz Linux veya macOS için toplama ve görüntüleme araçları hiçbir olay yok.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-379">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="ec1b3-380">Toplamak ve günlükleri görüntülemek için en iyi yolu kullanmaktır [PerfView yardımcı programı](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-380">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="ec1b3-381">ETW günlükleri görüntülemek için diğer araçları vardır, ancak PerfView ASP.NET tarafından yayılan ETW olayları ile çalışmak için en iyi deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-381">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="ec1b3-382">Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView yapılandırmak için dize Ekle `*Microsoft-Extensions-Logging` için **ek sağlayıcılar** listesi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-382">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="ec1b3-383">(Yıldız işareti dizenin başlangıcında kaçırmayın.)</span><span class="sxs-lookup"><span data-stu-id="ec1b3-383">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview ek sağlayıcılar](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="ec1b3-385">Windows olay günlüğü sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-385">Windows EventLog provider</span></span>

<span data-ttu-id="ec1b3-386">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcı paketi, Windows olay günlüğüne günlük çıktısını gönderir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="ec1b3-387">[AddEventLog aşırı](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) geçirdiğiniz let `EventLogSettings` veya en düşük günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-387">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="ec1b3-388">TraceSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-388">TraceSource provider</span></span>

<span data-ttu-id="ec1b3-389">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcı paketi kullanan [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) kitaplıkları ve sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="ec1b3-390">[AddTraceSource aşırı](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) kaynak anahtarı ve bir izleme dinleyicisi geçirdiğiniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-390">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="ec1b3-391">Bu sağlayıcıyı kullanmak için .NET Framework (yerine üzerinde .NET Core) çalıştırmak bir uygulama sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-391">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="ec1b3-392">Çeşitli iletileri yönlendirmek sağlayıcısı sağlar [dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners), gibi [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) kullanılan örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-392">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="ec1b3-393">Aşağıdaki örnek yapılandırır bir `TraceSource` günlükleri sağlayıcısı `Warning` ve konsol penceresinde daha yüksek ileti.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-393">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="ec1b3-394">Azure App Service sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-394">Azure App Service provider</span></span>

<span data-ttu-id="ec1b3-395">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcı paketi metin dosyalarını bir Azure App Service uygulamanın dosya sistemi ve çok günlükler Yazar [blob depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) bir Azure depolama hesabındaki.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-395">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="ec1b3-396">Sağlayıcı paketi, .NET Core 1.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-396">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ec1b3-397">.NET Core'u hedefleyen, aşağıdaki noktalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-397">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="ec1b3-398">ASP.NET Core sağlayıcısı paketinde [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) fakat [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-398">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="ec1b3-399">Açıkça Remove() çağırmayın [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-399">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="ec1b3-400">Azure App Service'te uygulama dağıtıldığında sağlayıcısı otomatik olarak uygulama için kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-400">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="ec1b3-401">.NET Framework'ü hedefleyen veya başvuru `Microsoft.AspNetCore.App` metapackage, sağlayıcı paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-401">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="ec1b3-402">Çağırma `AddAzureWebAppDiagnostics` üzerinde bir [Iloggerfactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) örneği:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-402">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="ec1b3-403">Bir [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) geçirdiğiniz sağlar aşırı [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) ile geçersiz kılma günlük çıktı şablonu, blob adı ve dosya gibi varsayılan ayarları boyut sınırını aştı.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-403">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="ec1b3-404">(*Çıkış şablon* çağırdığınızda sağlayan bir üzerine tüm günlükler için uygulanan bir ileti şablonu bir `ILogger` yöntemi.)</span><span class="sxs-lookup"><span data-stu-id="ec1b3-404">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="ec1b3-405">Bir App Service uygulamasına dağıtmak, uygulama ayarlarında yapılırken [tanılama günlükleri](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) bölümünü **App Service** Azure Portalı'nın sayfasında.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-405">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="ec1b3-406">Bu ayarlar güncelleştirildiğinde, değişiklikleri hemen yeniden başlatma veya yeniden dağıtma işlemi uygulamanın gerek olmadan etkinleşir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-406">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure günlük ayarları](index/_static/azure-logging-settings.png)

<span data-ttu-id="ec1b3-408">Günlük dosyaları için varsayılan konum alıyor *D:\\giriş\\LogFiles\\uygulama* klasörü ve varsayılan dosya adı olan *tanılama yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-408">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="ec1b3-409">Varsayılan dosya boyutu sınırını 10 MB'tır ve korunan dosyaları varsayılan en yüksek sayısı 2'dir.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-409">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="ec1b3-410">Varsayılan blob adı *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-410">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="ec1b3-411">Varsayılan davranış hakkında daha fazla bilgi için bkz. [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-411">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="ec1b3-412">Sağlayıcı, yalnızca proje Azure ortamında çalışan olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-412">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="ec1b3-413">Projeyi yerel olarak çalıştırdığınızda herhangi bir etkisi&mdash;yerel dosyaları ya da yerel geliştirme deposu bloblar için yazma değil.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-413">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="ec1b3-414">Üçüncü taraf günlük sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="ec1b3-414">Third-party logging providers</span></span>

<span data-ttu-id="ec1b3-415">ASP.NET Core ile çalışan üçüncü taraf günlük altyapılarına:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-415">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="ec1b3-416">[elmah.io](https://elmah.io/) ([GitHub deposunu](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="ec1b3-416">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="ec1b3-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposunu](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="ec1b3-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="ec1b3-418">[JSNLog](http://jsnlog.com/) ([GitHub deposunu](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="ec1b3-418">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="ec1b3-419">[Loggr](http://loggr.net/) ([GitHub deposunu](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="ec1b3-419">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="ec1b3-420">[NLog](http://nlog-project.org/) ([GitHub deposunu](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="ec1b3-420">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="ec1b3-421">[Serilog](https://serilog.net/) ([GitHub deposunu](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="ec1b3-421">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="ec1b3-422">Bazı üçüncü taraf çerçeveleri gerçekleştirebilirsiniz [yapılandırılmış günlük kaydı olarak da bilinen anlamlı günlük kaydını](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-422">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="ec1b3-423">Bir üçüncü taraf framework kullanarak, yerleşik sağlayıcılardan birini kullanmaya benzer:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-423">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="ec1b3-424">Bir NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-424">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="ec1b3-425">Uzantı metodu çağırma `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-425">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="ec1b3-426">Daha fazla bilgi için her framework'ün belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-426">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="ec1b3-427">Azure günlük akışı</span><span class="sxs-lookup"><span data-stu-id="ec1b3-427">Azure log streaming</span></span>

<span data-ttu-id="ec1b3-428">Azure günlük akışını gerçek zamanlı günlüğünü etkinliği görüntülemenize olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-428">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="ec1b3-429">Uygulama sunucusu</span><span class="sxs-lookup"><span data-stu-id="ec1b3-429">The application server</span></span>
* <span data-ttu-id="ec1b3-430">Web sunucusu</span><span class="sxs-lookup"><span data-stu-id="ec1b3-430">The web server</span></span>
* <span data-ttu-id="ec1b3-431">Başarısız istek izleme</span><span class="sxs-lookup"><span data-stu-id="ec1b3-431">Failed request tracing</span></span>

<span data-ttu-id="ec1b3-432">Azure günlük akışını yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="ec1b3-432">To configure Azure log streaming:</span></span>

* <span data-ttu-id="ec1b3-433">Gidin **tanılama günlükleri** sayfasından uygulamanızın portal sayfası</span><span class="sxs-lookup"><span data-stu-id="ec1b3-433">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="ec1b3-434">Ayarlama **uygulama günlüğü (dosya sistemi)** açık.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-434">Set **Application Logging (Filesystem)** to on.</span></span>

![Azure portalı tanılama günlükleri sayfası](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="ec1b3-436">Gidin **günlük akışını** uygulama iletilerini görüntülemek için sayfa.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-436">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="ec1b3-437">Uygulama oturum açmadıysanız `ILogger` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-437">They're logged by application through the `ILogger` interface.</span></span>

![Azure portal uygulaması günlük akışı](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="ec1b3-439">Azure Application Insights izleme günlüğü</span><span class="sxs-lookup"><span data-stu-id="ec1b3-439">Azure Application Insights trace logging</span></span>

<span data-ttu-id="ec1b3-440">[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK, ASP.NET Core günlük kaydı altyapısı aracılığıyla oluşturulan günlüklerinden izleme telemetrisi toplama özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="ec1b3-440">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="ec1b3-441">Daha fazla bilgi için [ASP.NET Core için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) ve [Applicationınsights/Microsoft-aspnetcore Wiki: günlük](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="ec1b3-441">For more information, see [Application Insights for ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) and [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec1b3-442">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ec1b3-442">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
