---
title: ASP.NET çekirdeği günlüğü
author: ardalis
description: ASP.NET Core günlük Framework'te hakkında bilgi edinin. Yerleşik günlük sağlayıcıları bulmak ve popüler üçüncü taraf sağlayıcılar hakkında daha fazla bilgi edinin.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: 4ceb7886cc9410c3b39beec68c2b11ea3578d851
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077783"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="598a2-104">ASP.NET çekirdeği günlüğü</span><span class="sxs-lookup"><span data-stu-id="598a2-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="598a2-105">Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="598a2-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="598a2-106">ASP.NET çekirdeği günlüğü sağlayıcıları çeşitli çalışır bir günlük API destekler.</span><span class="sxs-lookup"><span data-stu-id="598a2-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="598a2-107">Bir üçüncü taraf günlük framework takın ve yerleşik sağlayıcılar bir veya daha fazla hedeflere günlükleri göndermenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="598a2-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="598a2-108">Bu makalede yerleşik günlük API ve sağlayıcıları kodunuzu nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="598a2-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="598a2-110">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="598a2-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="598a2-112">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="598a2-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="598a2-113">Günlükleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="598a2-113">How to create logs</span></span>

<span data-ttu-id="598a2-114">Günlükleri oluşturmak için uygulaması bir [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) nesnesinin [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı:</span><span class="sxs-lookup"><span data-stu-id="598a2-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="598a2-115">Ardından Günlükçü nesnede günlüğe kaydetme yöntemlerini çağırın:</span><span class="sxs-lookup"><span data-stu-id="598a2-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="598a2-116">Bu örnek ile günlükleri oluşturur `TodoController` olarak sınıf *kategori*.</span><span class="sxs-lookup"><span data-stu-id="598a2-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="598a2-117">Kategoriler açıklanmıştır [bu makalenin ilerisinde yer](#log-category).</span><span class="sxs-lookup"><span data-stu-id="598a2-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="598a2-118">Günlük hızlı şekilde async kullanma maliyetini olmadığından emin olması gerektiğinden ASP.NET Core Günlükçü yöntemleri zaman uyumsuz sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="598a2-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="598a2-119">Burada, doğru olmayan bir durumda değilseniz, oturum şekilde değiştirmeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="598a2-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="598a2-120">Data store yavaşsa, günlük iletilerini önce hızlı bir depolama alanına yazın ve sonra bunları yavaş bir depolama alanına daha sonra taşıyın.</span><span class="sxs-lookup"><span data-stu-id="598a2-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="598a2-121">Örneğin, okuma ve başka bir işlem tarafından yavaş depolama için kalıcı bir ileti sırası için oturum açın.</span><span class="sxs-lookup"><span data-stu-id="598a2-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="598a2-122">Sağlayıcıları ekleme</span><span class="sxs-lookup"><span data-stu-id="598a2-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="598a2-124">Oturum açma sağlayıcısı ile oluşturduğunuz iletileri alan bir `ILogger` nesnesi, görüntüler ve bunları depolar.</span><span class="sxs-lookup"><span data-stu-id="598a2-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="598a2-125">Örneğin, konsolu sağlayıcısı konsolda iletileri görüntüler ve Azure uygulama hizmeti sağlayıcısı Azure blob storage'da depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="598a2-126">Bir sağlayıcı kullanmak için sağlayıcının çağrısı `Add<ProviderName>` uzantı yönteminde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="598a2-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="598a2-127">Varsayılan proje şablonu ile günlük kaydını etkinleştirir [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="598a2-127">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="598a2-129">Oturum açma sağlayıcısı ile oluşturduğunuz iletileri alan bir `ILogger` nesnesi, görüntüler ve bunları depolar.</span><span class="sxs-lookup"><span data-stu-id="598a2-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="598a2-130">Örneğin, konsolu sağlayıcısı konsolda iletileri görüntüler ve Azure uygulama hizmeti sağlayıcısı Azure blob storage'da depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="598a2-131">Bir sağlayıcı kullanmak için NuGet paketini yükleyin ve bir örneğinde sağlayıcının uzantı metodu çağırma [Iloggerfactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="598a2-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="598a2-132">ASP.NET Core [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) sağlayan `ILoggerFactory` örneği.</span><span class="sxs-lookup"><span data-stu-id="598a2-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="598a2-133">`AddConsole` Ve `AddDebug` genişletme yöntemleri tanımlanmış [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) ve [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paketler.</span><span class="sxs-lookup"><span data-stu-id="598a2-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="598a2-134">Her bir genişletme yöntemi çağırır `ILoggerFactory.AddProvider` sağlayıcının bir örneğini geçirerek yöntemi.</span><span class="sxs-lookup"><span data-stu-id="598a2-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="598a2-135">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) günlüğü sağlayıcıları ekler `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="598a2-135">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="598a2-136">Daha önce yürütür koddan günlük çıktısı alma istiyorsanız, günlük Sağlayıcıları Ekle `Startup` sınıfı oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="598a2-136">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

---

<span data-ttu-id="598a2-137">Her hakkında bilgi edineceksiniz [yerleşik oturum açma sağlayıcısı](#built-in-logging-providers) ve bağlandığı [üçüncü taraf günlüğü sağlayıcıları](#third-party-logging-providers) sonraki makalede.</span><span class="sxs-lookup"><span data-stu-id="598a2-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="598a2-138">Örnek günlük çıktısı</span><span class="sxs-lookup"><span data-stu-id="598a2-138">Sample logging output</span></span>

<span data-ttu-id="598a2-139">Örnek kod önceki bölümde gösterilen komut satırından çalıştırdığınızda, konsol günlüklerine görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="598a2-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="598a2-140">Konsol çıktısı bir örneği burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="598a2-140">Here's an example of console output:</span></span>

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

<span data-ttu-id="598a2-141">Bu günlükler giderek oluşturulan `http://localhost:5000/api/todo/0`, her ikisi de yürütülmesini tetikler `ILogger` önceki bölümde gösterilen çağrıları.</span><span class="sxs-lookup"><span data-stu-id="598a2-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="598a2-142">Burada, Visual Studio'daki örnek uygulamayı çalıştırdığınızda, hata ayıklama penceresinde göründükleri gibi aynı günlükleri örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="598a2-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="598a2-143">Tarafından oluşturulan günlükleri `ILogger` önceki bölümde gösterilen çağrıları "TodoApi.Controllers.TodoController" ile başlar.</span><span class="sxs-lookup"><span data-stu-id="598a2-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="598a2-144">"Microsoft" kategorileri ile başlayan ASP.NET Core günlüklerin.</span><span class="sxs-lookup"><span data-stu-id="598a2-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="598a2-145">ASP.NET Core kendisi ve uygulama kodunuz aynı günlüğü API ve aynı günlük sağlayıcıları kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="598a2-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="598a2-146">Bu makalenin sonraki bölümlerinde, bazı ayrıntılar ve günlüğe kaydetme seçeneklerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="598a2-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="598a2-147">NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="598a2-147">NuGet packages</span></span>

<span data-ttu-id="598a2-148">`ILogger` Ve `ILoggerFactory` arabirimleri olan [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ve bunlar için varsayılan uygulamalarıdır içinde [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="598a2-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="598a2-149">Günlük kategorisi</span><span class="sxs-lookup"><span data-stu-id="598a2-149">Log category</span></span>

<span data-ttu-id="598a2-150">A *kategori* oluşturduğunuz her bir günlük bulunur.</span><span class="sxs-lookup"><span data-stu-id="598a2-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="598a2-151">Kategori oluşturduğunuzda belirttiğiniz bir `ILogger` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="598a2-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="598a2-152">Kategori herhangi bir dize olabilir, ancak günlükler yazılmış olan sınıfın tam adını kullanmak için bir kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="598a2-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="598a2-153">Örneğin: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="598a2-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="598a2-154">Bir dize olarak kategori belirtin veya kategori türünden bir genişletme yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="598a2-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="598a2-155">Kategori dize olarak belirtmek için arama `CreateLogger` üzerinde bir `ILoggerFactory` , aşağıda gösterildiği gibi örneği.</span><span class="sxs-lookup"><span data-stu-id="598a2-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="598a2-156">Çoğu zaman, kullanımı daha kolay olacaktır `ILogger<T>`, aşağıdaki örnekte olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="598a2-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="598a2-157">Bu arama için eşdeğerdir `CreateLogger` tam olarak nitelenmiş tür adını `T`.</span><span class="sxs-lookup"><span data-stu-id="598a2-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="598a2-158">Günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="598a2-158">Log level</span></span>

<span data-ttu-id="598a2-159">Her saat bir günlük yazma belirttiğiniz kendi [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="598a2-159">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="598a2-160">Günlük düzeyini önem veya önem derecesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="598a2-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="598a2-161">Örneğin, yazabilir bir `Information` oturum bir yöntem normalde sona erdiğinde bir `Warning` bir 404 dönüş kodu bir yöntem ve bir oturum `Error` günlüğe beklenmeyen bir özel durum catch.</span><span class="sxs-lookup"><span data-stu-id="598a2-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="598a2-162">Aşağıdaki kod örneğinde, yöntemlerin adlarını (örneğin, `LogWarning`) günlük düzeyini belirtin.</span><span class="sxs-lookup"><span data-stu-id="598a2-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="598a2-163">İlk parametre [günlüğe olay kimliği](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="598a2-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="598a2-164">İkinci parametre bir [iletisi şablonunu](#log-message-template) kalan yöntem parametreleri tarafından sağlanan bağımsız değişken değerler için yer tutucu ile.</span><span class="sxs-lookup"><span data-stu-id="598a2-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="598a2-165">Yöntem parametreleri, bu makalenin sonraki bölümlerinde daha ayrıntılı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="598a2-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="598a2-166">Düzeyi yöntemi adında günlük yöntemleri [için ILogger genişletme yöntemleri](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="598a2-166">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="598a2-167">Arka planda bu yöntemlerini çağıran bir `Log` yönteminin alan bir `LogLevel` parametresi.</span><span class="sxs-lookup"><span data-stu-id="598a2-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="598a2-168">Çağırabilirsiniz `Log` bu genişletme yöntemleri, ancak sözdizimini biri yerine doğrudan yöntemi nispeten karmaşık.</span><span class="sxs-lookup"><span data-stu-id="598a2-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="598a2-169">Daha fazla bilgi için bkz: [ILogger arabirimi](/dotnet/api/microsoft.extensions.logging.ilogger) ve [Günlükçü uzantılarını kaynak kodu](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="598a2-169">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="598a2-170">ASP.NET Core tanımlar aşağıdaki [günlük düzeyleri](/dotnet/api/microsoft.extensions.logging.loglevel), burada alınan en az çok yüksek önem sıralı.</span><span class="sxs-lookup"><span data-stu-id="598a2-170">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="598a2-171">İzleme = 0</span><span class="sxs-lookup"><span data-stu-id="598a2-171">Trace = 0</span></span>

  <span data-ttu-id="598a2-172">Yalnızca bir sorun hata ayıklama bir geliştirici değerli bilgiler.</span><span class="sxs-lookup"><span data-stu-id="598a2-172">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="598a2-173">Bu iletiler önemli uygulama verileri içerebilir ve bu nedenle bir üretim ortamında etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="598a2-173">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="598a2-174">*Varsayılan olarak devre dışıdır.*</span><span class="sxs-lookup"><span data-stu-id="598a2-174">*Disabled by default.*</span></span> <span data-ttu-id="598a2-175">Örnek: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="598a2-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="598a2-176">Hata ayıklama = 1</span><span class="sxs-lookup"><span data-stu-id="598a2-176">Debug = 1</span></span>

  <span data-ttu-id="598a2-177">Bilgi için kısa vadeli yararlılığını geliştirme ve hata ayıklama sırasında sahiptir.</span><span class="sxs-lookup"><span data-stu-id="598a2-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="598a2-178">Örnek: `Entering method Configure with flag set to true.` genellikle etkinleştirmek olmayacaktır `Debug` düzeyi günlükleri yüksek hacimli nedeniyle giderirken sürece üretimde günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="598a2-178">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="598a2-179">Bilgi = 2</span><span class="sxs-lookup"><span data-stu-id="598a2-179">Information = 2</span></span>

  <span data-ttu-id="598a2-180">Uygulamanın genel akış izlemek için.</span><span class="sxs-lookup"><span data-stu-id="598a2-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="598a2-181">Bu günlükler genellikle uzun vadeli bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="598a2-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="598a2-182">Örnek: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="598a2-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="598a2-183">Uyarı = 3</span><span class="sxs-lookup"><span data-stu-id="598a2-183">Warning = 3</span></span>

  <span data-ttu-id="598a2-184">Uygulama akışındaki anormal veya beklenmedik olaylar için.</span><span class="sxs-lookup"><span data-stu-id="598a2-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="598a2-185">Bunlar, hatalar veya uygulamanın durdurmasına neden yoktur, ancak araştırılması gereken diğer koşullar olabilir.</span><span class="sxs-lookup"><span data-stu-id="598a2-185">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="598a2-186">İşlenmiş istisnaları kullanmak için ortak bir yerde `Warning` günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="598a2-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="598a2-187">Örnek: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="598a2-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="598a2-188">Hata = 4</span><span class="sxs-lookup"><span data-stu-id="598a2-188">Error = 4</span></span>

  <span data-ttu-id="598a2-189">Hatalar ve özel durumlar için işlenemiyor.</span><span class="sxs-lookup"><span data-stu-id="598a2-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="598a2-190">Bu iletiler, geçerli etkinliği ya da (örneğin, geçerli HTTP isteği) işlemi bir hata, uygulama genelinde hata gösterir.</span><span class="sxs-lookup"><span data-stu-id="598a2-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="598a2-191">Örnek günlük iletisi: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="598a2-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="598a2-192">Kritik = 5</span><span class="sxs-lookup"><span data-stu-id="598a2-192">Critical = 5</span></span>

  <span data-ttu-id="598a2-193">Hemen ilgilenilmesi gereken hataları.</span><span class="sxs-lookup"><span data-stu-id="598a2-193">For failures that require immediate attention.</span></span> <span data-ttu-id="598a2-194">Örnekler: veri kaybı senaryolarını, disk alanı yetersiz.</span><span class="sxs-lookup"><span data-stu-id="598a2-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="598a2-195">Günlük düzeyini ne kadar günlük çıktısı bir belirli depolama ortamına yazılır denetlemek veya penceresini görüntülemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="598a2-196">Örneğin, üretim aşamasında, tüm günlüklerini isteyebilirsiniz `Information` düzey ve toplu veri deposu ve tüm günlüklerini gitmek için alt `Warning` düzeyi ve daha yüksek bir değer veri deposuna gidin.</span><span class="sxs-lookup"><span data-stu-id="598a2-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="598a2-197">Geliştirme sırasında normal olarak günlüklerini gönderebilir `Warning` veya konsola daha yüksek önem derecesi.</span><span class="sxs-lookup"><span data-stu-id="598a2-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="598a2-198">Sorun giderme gerektiğinde ekleyebilirsiniz `Debug` düzeyi.</span><span class="sxs-lookup"><span data-stu-id="598a2-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="598a2-199">[Günlüğü filtreleme](#log-filtering) bölümde bu makalenin sonraki bölümlerinde, sağlayıcı işleme hangi günlük düzeyleri denetleme açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="598a2-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="598a2-200">ASP.NET Core framework Yazar `Debug` düzey framework olayları için günlükleri.</span><span class="sxs-lookup"><span data-stu-id="598a2-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="598a2-201">Günlük örnekleri, bu makalede daha önce günlükleri aşağıdaki hariç tutulan. `Information` düzeyi, dolayısıyla `Debug` düzey günlükleri gösterilen.</span><span class="sxs-lookup"><span data-stu-id="598a2-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="598a2-202">İşte bir örnek konsol günlüklerinin göstermek için yapılandırılmış bir örnek uygulama çalıştırırsanız `Debug` ve konsol sağlayıcısı için daha yüksek günlükleri.</span><span class="sxs-lookup"><span data-stu-id="598a2-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="598a2-203">Günlük Olay Kimliği</span><span class="sxs-lookup"><span data-stu-id="598a2-203">Log event ID</span></span>

<span data-ttu-id="598a2-204">Her saat bir günlük yazma belirleyebileceğiniz bir *olay kimliği*.</span><span class="sxs-lookup"><span data-stu-id="598a2-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="598a2-205">Örnek uygulamayı yerel olarak tanımlanan bir kullanarak bunu yapar `LoggingEvents` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="598a2-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="598a2-206">Olay Kimliği günlüğe kaydedilen olayları kümesi biriyle ilişkilendirmek için kullanabileceğiniz bir tamsayı değil.</span><span class="sxs-lookup"><span data-stu-id="598a2-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="598a2-207">Örneğin, alışveriş sepetine bir öğe eklemek için bir günlük olay kimliği 1000 olabilir ve daha sonra satın almasını tamamlamak için bir günlük olay kimliği 1001 olabilir.</span><span class="sxs-lookup"><span data-stu-id="598a2-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="598a2-208">Günlüğe kaydetme çıktısında olay kimliği bir alanda depolanan veya olabilir metin iletisi sağlayıcısı bağlı olarak dahil.</span><span class="sxs-lookup"><span data-stu-id="598a2-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="598a2-209">Hata ayıklama sağlayıcısı olay kimlikleri göstermez, ancak konsol sağlayıcısı bunları köşeli sonra kategori gösterir:</span><span class="sxs-lookup"><span data-stu-id="598a2-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="598a2-210">Günlük iletisi şablonu</span><span class="sxs-lookup"><span data-stu-id="598a2-210">Log message template</span></span>

<span data-ttu-id="598a2-211">Bir günlük iletisine yazma her zaman bir ileti şablonu sağlayın.</span><span class="sxs-lookup"><span data-stu-id="598a2-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="598a2-212">İleti şablon bir dize olabilir veya adlandırılmış yer tutucuları hangi bağımsız değişkeninin değerler yerleştirilir içerebilir.</span><span class="sxs-lookup"><span data-stu-id="598a2-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="598a2-213">Şablon bir biçim dizesi değil ve yer tutucuları, değil numaralı adlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="598a2-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="598a2-214">Hangi parametreleri değerlerini sağlamak için kullanılan yer tutucuları, bunların adları sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="598a2-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="598a2-215">Aşağıdaki kodu varsa:</span><span class="sxs-lookup"><span data-stu-id="598a2-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="598a2-216">Elde edilen günlük iletisi şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="598a2-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="598a2-217">Günlüğe kaydetme framework uygulamak, günlüğü sağlayıcıları olanak sağlamak için bu şekilde biçimlendirme ileti [yapılandırılmış günlük olarak da bilinen semantik günlük](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="598a2-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="598a2-218">Bağımsız değişkenler kendilerini yalnızca biçimlendirilmiş iletinin şablon günlük sistemi geçirildiğinden günlüğü sağlayıcıları iletisi şablonunu ek alanlar olarak parametre değerlerini depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="598a2-219">Azure tablo depolaması için çıktı günlüğünüzün yönlendirerek ve Günlükçü yöntem çağrısı şöyle varsa:</span><span class="sxs-lookup"><span data-stu-id="598a2-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="598a2-220">Her Azure Table varlığın olabilir `ID` ve `RequestTime` günlüğü verilerini sorguları basitleştirir özellikleri.</span><span class="sxs-lookup"><span data-stu-id="598a2-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="598a2-221">Belirli bir içindeki tüm günlükleri bulabilirsiniz `RequestTime` kısa mesaj zaman aşımı ayrıştırma gerek kalmadan aralık.</span><span class="sxs-lookup"><span data-stu-id="598a2-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="598a2-222">Özel durumları</span><span class="sxs-lookup"><span data-stu-id="598a2-222">Logging exceptions</span></span>

<span data-ttu-id="598a2-223">Aşağıdaki örnekte olduğu gibi bir özel durum geçirmenize olanak tanıyan aşırı Günlükçü yöntemleri vardır:</span><span class="sxs-lookup"><span data-stu-id="598a2-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="598a2-224">Farklı sağlayıcıları özel durum bilgilerini farklı yollarla işleyin.</span><span class="sxs-lookup"><span data-stu-id="598a2-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="598a2-225">Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktı örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="598a2-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="598a2-226">Günlük filtreleme</span><span class="sxs-lookup"><span data-stu-id="598a2-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="598a2-228">Tüm sağlayıcılar veya tüm kategorileri veya özel sağlayıcı ve kategori için en küçük günlük düzeyi belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="598a2-229">Bunlar görüntülenen depolanan ya da yok minimum düzeyin altındaki herhangi bir günlük bu sağlayıcı için geçirilen değil.</span><span class="sxs-lookup"><span data-stu-id="598a2-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="598a2-230">Tüm günlükler gizlemek istiyorsanız, belirtebilirsiniz `LogLevel.None` minimum günlük düzeyini olarak.</span><span class="sxs-lookup"><span data-stu-id="598a2-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="598a2-231">Tamsayı değeri `LogLevel.None` değerinden yüksek olduğu 6 olduğu `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="598a2-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="598a2-232">**Yapılandırmada filtre kuralları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="598a2-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="598a2-233">Proje şablonları çağıran kodu oluşturmak `CreateDefaultBuilder` konsol ve hata ayıklama sağlayıcıları için günlük kaydını ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="598a2-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="598a2-234">`CreateDefaultBuilder` Yöntemini de ayarlar günlüğünü yapılandırmasında aramak için bir `Logging` bölümünde, aşağıdaki gibi kod kullanarak:</span><span class="sxs-lookup"><span data-stu-id="598a2-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="598a2-235">Yapılandırma verileri sağlayıcısı ve aşağıdaki örnekteki gibi kategoriye göre en düşük günlük düzeyleri belirtir:</span><span class="sxs-lookup"><span data-stu-id="598a2-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="598a2-236">Bu JSON altı filtre kuralları, hata ayıklama sağlayıcının Birincisi, konsol sağlayıcısı için dört ve tüm sağlayıcılar için geçerli bir oluşturur.</span><span class="sxs-lookup"><span data-stu-id="598a2-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="598a2-237">Bu kurallar, daha sonra nasıl tek her sağlayıcı için seçilen görürsünüz, bir `ILogger` nesnesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="598a2-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="598a2-238">**Kodda filtre kuralları**</span><span class="sxs-lookup"><span data-stu-id="598a2-238">**Filter rules in code**</span></span>

<span data-ttu-id="598a2-239">Aşağıdaki örnekte gösterildiği gibi filtre kuralları kodda kaydedebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="598a2-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="598a2-240">İkinci `AddFilter` türü adını kullanarak hata ayıklama sağlayıcıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="598a2-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="598a2-241">İlk `AddFilter` bir sağlayıcı türü belirtmeyen çünkü tüm sağlayıcılar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="598a2-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="598a2-242">**Filtreleme nasıl kurallar uygulanır**</span><span class="sxs-lookup"><span data-stu-id="598a2-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="598a2-243">Yapılandırma verilerini ve `AddFilter` Yukarıdaki örneklerde gösterilen kodu aşağıdaki tabloda gösterilen kurallar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="598a2-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="598a2-244">İlk altı yapılandırma örneğinden gelen ve son iki kod örneğindeki gelir.</span><span class="sxs-lookup"><span data-stu-id="598a2-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="598a2-245">Sayı</span><span class="sxs-lookup"><span data-stu-id="598a2-245">Number</span></span> | <span data-ttu-id="598a2-246">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="598a2-246">Provider</span></span>      | <span data-ttu-id="598a2-247">İle başlayan kategoriler...</span><span class="sxs-lookup"><span data-stu-id="598a2-247">Categories that begin with ...</span></span>          | <span data-ttu-id="598a2-248">En küçük günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="598a2-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="598a2-249">1.</span><span class="sxs-lookup"><span data-stu-id="598a2-249">1</span></span>      | <span data-ttu-id="598a2-250">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-250">Debug</span></span>         | <span data-ttu-id="598a2-251">Tüm kategorileri</span><span class="sxs-lookup"><span data-stu-id="598a2-251">All categories</span></span>                          | <span data-ttu-id="598a2-252">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="598a2-252">Information</span></span>       |
| <span data-ttu-id="598a2-253">2</span><span class="sxs-lookup"><span data-stu-id="598a2-253">2</span></span>      | <span data-ttu-id="598a2-254">Konsol</span><span class="sxs-lookup"><span data-stu-id="598a2-254">Console</span></span>       | <span data-ttu-id="598a2-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="598a2-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="598a2-256">Uyarı</span><span class="sxs-lookup"><span data-stu-id="598a2-256">Warning</span></span>           |
| <span data-ttu-id="598a2-257">3</span><span class="sxs-lookup"><span data-stu-id="598a2-257">3</span></span>      | <span data-ttu-id="598a2-258">Konsol</span><span class="sxs-lookup"><span data-stu-id="598a2-258">Console</span></span>       | <span data-ttu-id="598a2-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="598a2-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="598a2-260">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-260">Debug</span></span>             |
| <span data-ttu-id="598a2-261">4</span><span class="sxs-lookup"><span data-stu-id="598a2-261">4</span></span>      | <span data-ttu-id="598a2-262">Konsol</span><span class="sxs-lookup"><span data-stu-id="598a2-262">Console</span></span>       | <span data-ttu-id="598a2-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="598a2-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="598a2-264">Hata</span><span class="sxs-lookup"><span data-stu-id="598a2-264">Error</span></span>             |
| <span data-ttu-id="598a2-265">5</span><span class="sxs-lookup"><span data-stu-id="598a2-265">5</span></span>      | <span data-ttu-id="598a2-266">Konsol</span><span class="sxs-lookup"><span data-stu-id="598a2-266">Console</span></span>       | <span data-ttu-id="598a2-267">Tüm kategorileri</span><span class="sxs-lookup"><span data-stu-id="598a2-267">All categories</span></span>                          | <span data-ttu-id="598a2-268">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="598a2-268">Information</span></span>       |
| <span data-ttu-id="598a2-269">6</span><span class="sxs-lookup"><span data-stu-id="598a2-269">6</span></span>      | <span data-ttu-id="598a2-270">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="598a2-270">All providers</span></span> | <span data-ttu-id="598a2-271">Tüm kategorileri</span><span class="sxs-lookup"><span data-stu-id="598a2-271">All categories</span></span>                          | <span data-ttu-id="598a2-272">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-272">Debug</span></span>             |
| <span data-ttu-id="598a2-273">7</span><span class="sxs-lookup"><span data-stu-id="598a2-273">7</span></span>      | <span data-ttu-id="598a2-274">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="598a2-274">All providers</span></span> | <span data-ttu-id="598a2-275">Sistem</span><span class="sxs-lookup"><span data-stu-id="598a2-275">System</span></span>                                  | <span data-ttu-id="598a2-276">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-276">Debug</span></span>             |
| <span data-ttu-id="598a2-277">8</span><span class="sxs-lookup"><span data-stu-id="598a2-277">8</span></span>      | <span data-ttu-id="598a2-278">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-278">Debug</span></span>         | <span data-ttu-id="598a2-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="598a2-279">Microsoft</span></span>                               | <span data-ttu-id="598a2-280">İzleme</span><span class="sxs-lookup"><span data-stu-id="598a2-280">Trace</span></span>             |

<span data-ttu-id="598a2-281">Oluştururken bir `ILogger` günlüklerini, yazma için nesne `ILoggerFactory` nesne bu Günlükçü uygulamak için sağlayıcı başına tek bir kural seçer.</span><span class="sxs-lookup"><span data-stu-id="598a2-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="598a2-282">Tarafından yazılan tüm iletilerin `ILogger` nesne seçilen kuralları göre filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="598a2-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="598a2-283">Her bir sağlayıcı ve kategori çifti için olası en özel kural kullanılabilir kurallar seçilir.</span><span class="sxs-lookup"><span data-stu-id="598a2-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="598a2-284">Her sağlayıcı için kullanılan aşağıdaki algoritmayı olduğunda bir `ILogger` için belirli bir kategori oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="598a2-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="598a2-285">Sağlayıcı veya diğer adını eşleşen tüm kuralları'nı seçin.</span><span class="sxs-lookup"><span data-stu-id="598a2-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="598a2-286">Bulunursa, tüm kurallar ile boş bir sağlayıcı seçin.</span><span class="sxs-lookup"><span data-stu-id="598a2-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="598a2-287">Önceki adımı sonucundan select kurallarıyla en uzun kategori önek eşleştirme.</span><span class="sxs-lookup"><span data-stu-id="598a2-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="598a2-288">Bulunursa, bir kategori belirtmeyin tüm kuralları seçin.</span><span class="sxs-lookup"><span data-stu-id="598a2-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="598a2-289">Birden çok kural seçtiyseniz ele **son** biri.</span><span class="sxs-lookup"><span data-stu-id="598a2-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="598a2-290">Hiçbir kural seçtiyseniz, kullanın `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="598a2-290">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="598a2-291">Örneğin, önceki kurallar listesine sahip ve oluşturduğunuz varsayalım bir `ILogger` nesne kategori "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" için:</span><span class="sxs-lookup"><span data-stu-id="598a2-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="598a2-292">Hata ayıklama sağlayıcısı için 1, 6 ve 8 kurallar geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="598a2-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="598a2-293">Seçilen olacak şekilde 8 en belirgin kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="598a2-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="598a2-294">Konsolu sağlayıcısı için 3, 4, 5 ve 6 kurallar geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="598a2-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="598a2-295">Kural 3 en özeldir.</span><span class="sxs-lookup"><span data-stu-id="598a2-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="598a2-296">Günlükleri ile oluşturduğunuzda bir `ILogger` , "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" kategorisi için günlükleri `Trace` düzey ve hata ayıklama sağlayıcısı ve günlükleri için yukarıdaki gidecek `Debug` düzey ve yukarıdaki konsol sağlayıcıya geçer.</span><span class="sxs-lookup"><span data-stu-id="598a2-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="598a2-297">**Sağlayıcı diğer adlar**</span><span class="sxs-lookup"><span data-stu-id="598a2-297">**Provider aliases**</span></span>

<span data-ttu-id="598a2-298">Tür adı yapılandırmada bir sağlayıcısını belirtmek için kullanabilirsiniz, ancak her sağlayıcı daha kısa bir tanımlar *diğer* kullanmak daha kolay olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="598a2-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="598a2-299">Yerleşik sağlayıcılar için aşağıdaki diğer adlar kullanın:</span><span class="sxs-lookup"><span data-stu-id="598a2-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="598a2-300">Konsol</span><span class="sxs-lookup"><span data-stu-id="598a2-300">Console</span></span>
- <span data-ttu-id="598a2-301">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-301">Debug</span></span>
- <span data-ttu-id="598a2-302">Olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="598a2-302">EventLog</span></span>
- <span data-ttu-id="598a2-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="598a2-303">AzureAppServices</span></span>
- <span data-ttu-id="598a2-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="598a2-304">TraceSource</span></span>
- <span data-ttu-id="598a2-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="598a2-305">EventSource</span></span>

<span data-ttu-id="598a2-306">**Varsayılan en düşük düzeyi**</span><span class="sxs-lookup"><span data-stu-id="598a2-306">**Default minimum level**</span></span>

<span data-ttu-id="598a2-307">Yalnızca hiçbir kural yapılandırma veya kod verilen sağlayıcı ve kategori için uygularsanız, etkinleşir en az bir düzeyi ayarı yoktur.</span><span class="sxs-lookup"><span data-stu-id="598a2-307">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="598a2-308">Aşağıdaki örnekte, en düşük düzeyi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="598a2-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="598a2-309">En düşük düzey açıkça ayarlamazsanız, varsayılan değer: `Information`, anlamına `Trace` ve `Debug` günlükleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="598a2-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="598a2-310">**Filtre işlevleri**</span><span class="sxs-lookup"><span data-stu-id="598a2-310">**Filter functions**</span></span>

<span data-ttu-id="598a2-311">Filtreleme kurallarını uygulamak için bir filtre işlevi kod yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="598a2-312">Tüm sağlayıcılar ve yapılandırma veya kodu tarafından atanmış kural kategorileri için bir filtre işlevi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="598a2-312">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="598a2-313">İşlev kodda sağlayıcı türü, kategori ve günlük düzeyi bir ileti günlüğe olup olmadığına karar vermek için erişimi vardır.</span><span class="sxs-lookup"><span data-stu-id="598a2-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="598a2-314">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="598a2-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="598a2-316">Bazı günlük sağlayıcıları ne zaman günlükleri bir depolama ortamına yazılmış veya göz ardı belirtmenizi günlük düzeyi ve kategorisine göre sağlar.</span><span class="sxs-lookup"><span data-stu-id="598a2-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="598a2-317">`AddConsole` Ve `AddDebug` filtre ölçütüyle geçirmenize olanak tanıyan aşırı genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="598a2-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="598a2-318">Aşağıdaki örnek kod günlükleri aşağıdaki yoksaymak Konsolu sağlayıcısı neden `Warning` hata ayıklama sağlayıcısı çerçevesini oluşturur günlükleri yoksayar sırada düzey.</span><span class="sxs-lookup"><span data-stu-id="598a2-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="598a2-319">`AddEventLog` Yöntemi götüren bir aşırı sahip bir `EventLogSettings` filtreleme işlevinde içerebilir örneği kendi `Filter` özelliği.</span><span class="sxs-lookup"><span data-stu-id="598a2-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="598a2-320">Günlük düzeyi ve diğer parametreleri dayalı olduğundan TraceSource sağlayıcısı bu aşırı hiçbirini sağlamaz `SourceSwitch` ve `TraceListener` kullanır.</span><span class="sxs-lookup"><span data-stu-id="598a2-320">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="598a2-321">Filtreleme kuralları ile kayıtlı tüm sağlayıcılar için ayarlayabileceğiniz bir `ILoggerFactory` kullanarak örnek `WithFilter` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="598a2-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="598a2-322">Aşağıdaki örnek, hata ayıklama düzeyi uygulama günlüğüne izin verirken (kategori "Microsoft" veya "Sistem" ile başlayan) framework günlükleri uyarılar için sınırlar.</span><span class="sxs-lookup"><span data-stu-id="598a2-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="598a2-323">Filtreleme için belirli bir kategoriye yazılan tüm günlükleri önlemek için kullanmak istiyorsanız, belirtebilirsiniz `LogLevel.None` kategori için en küçük günlük düzeyi olarak.</span><span class="sxs-lookup"><span data-stu-id="598a2-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="598a2-324">Tamsayı değeri `LogLevel.None` değerinden yüksek olduğu 6 olduğu `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="598a2-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="598a2-325">`WithFilter` Genişletme yöntemi tarafından sağlanan [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="598a2-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="598a2-326">Yeni bir yöntem `ILoggerFactory` tüm Günlükçü sağlayıcılarına geçirilen günlüğü iletileri filtreler örnek ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="598a2-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="598a2-327">Diğer etkilemez `ILoggerFactory` örnekleri, özgün dahil olmak üzere `ILoggerFactory` örneği.</span><span class="sxs-lookup"><span data-stu-id="598a2-327">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="598a2-328">Günlük kapsamları</span><span class="sxs-lookup"><span data-stu-id="598a2-328">Log scopes</span></span>

<span data-ttu-id="598a2-329">Mantıksal işlemlerini kümesi gruplandırabilirsiniz bir *kapsam* aynı veri kümesinin bir parçası olarak oluşturulan her bir günlükteki iliştirmek için.</span><span class="sxs-lookup"><span data-stu-id="598a2-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="598a2-330">Örneğin, işlem kimliği eklemek için bir işlem işlenirken bir parçası olarak oluşturulan her günlük isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="598a2-331">Bir kapsam bir `IDisposable` tarafından döndürülen tür [ILogger.BeginScope&lt;TState&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) yöntemi ve bırakılana kadar lasts.</span><span class="sxs-lookup"><span data-stu-id="598a2-331">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="598a2-332">Kaydırma, Günlükçü çağrıları tarafından bir kapsamı kullanacak bir `using` , aşağıda gösterildiği gibi engelle:</span><span class="sxs-lookup"><span data-stu-id="598a2-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="598a2-333">Aşağıdaki kod Konsolu sağlayıcısı kapsamları etkinleştirir:</span><span class="sxs-lookup"><span data-stu-id="598a2-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="598a2-335">İçinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="598a2-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="598a2-336">Yapılandırma `IncludeScopes` kapsam tabanlı günlük kaydını etkinleştirmek için konsol Günlükçü seçeneği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="598a2-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="598a2-337">Yapılandırmasını `IncludeScopes` kullanarak *appsettings* yapılandırma dosyalarını ASP.NET Core 2.1 sürümünde kullanılabilir olacak.</span><span class="sxs-lookup"><span data-stu-id="598a2-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="598a2-339">İçinde *haline*:</span><span class="sxs-lookup"><span data-stu-id="598a2-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="598a2-340">Her günlük iletisi kapsamlı bilgiler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="598a2-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="598a2-341">Yerleşik günlük sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="598a2-341">Built-in logging providers</span></span>

<span data-ttu-id="598a2-342">ASP.NET Core aşağıdaki sağlayıcıları gelir:</span><span class="sxs-lookup"><span data-stu-id="598a2-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="598a2-343">Console</span><span class="sxs-lookup"><span data-stu-id="598a2-343">Console</span></span>](#console-provider)
* [<span data-ttu-id="598a2-344">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-344">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="598a2-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="598a2-345">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="598a2-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="598a2-346">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="598a2-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="598a2-347">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="598a2-348">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="598a2-348">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="598a2-349">Konsolu sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="598a2-349">Console provider</span></span>

<span data-ttu-id="598a2-350">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcısı paketi konsola günlük çıkış gönderir.</span><span class="sxs-lookup"><span data-stu-id="598a2-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="598a2-353">[AddConsole aşırı](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) geçirdiğiniz olanak sağlayan bir minimum günlük düzeyi, bir filtre işlevi ve kapsamları desteklenip desteklenmediğini belirten bir Boole değeri.</span><span class="sxs-lookup"><span data-stu-id="598a2-353">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="598a2-354">Başka bir seçenek de geçirmektir bir `IConfiguration` kapsamları desteği ve günlüğe kaydetme düzeylerini belirleyebilirsiniz nesnesi.</span><span class="sxs-lookup"><span data-stu-id="598a2-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="598a2-355">Üretim kullanımı için konsol sağlayıcısı düşünüyorsanız, performans üzerinde önemli bir etkisi olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="598a2-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="598a2-356">Visual Studio'da yeni bir proje oluşturduğunuzda `AddConsole` yöntemi şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="598a2-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="598a2-357">Bu kod başvurduğu `Logging` bölümünü *appSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="598a2-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="598a2-358">Hata ayıklama düzeyinde açıklandığı gibi oturum sınırı framework günlükleri uyarılar için uygulama izin verirken gösterilen ayarları [günlüğü filtreleme](#log-filtering) bölümü.</span><span class="sxs-lookup"><span data-stu-id="598a2-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="598a2-359">Daha fazla bilgi için bkz: [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="598a2-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

### <a name="debug-provider"></a><span data-ttu-id="598a2-360">Sağlayıcı hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="598a2-360">Debug provider</span></span>

<span data-ttu-id="598a2-361">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcısı paketi kullanarak bir günlük çıktısı Yazar [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) sınıfı (`Debug.WriteLine` yöntem çağrıları).</span><span class="sxs-lookup"><span data-stu-id="598a2-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="598a2-362">Linux üzerinde bu sağlayıcı için günlükler Yazar */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="598a2-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="598a2-365">[AddDebug aşırı](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) en küçük günlük düzeyi veya bir filtre işlevi geçirdiğiniz izin verir.</span><span class="sxs-lookup"><span data-stu-id="598a2-365">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

### <a name="eventsource-provider"></a><span data-ttu-id="598a2-366">EventSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="598a2-366">EventSource provider</span></span>

<span data-ttu-id="598a2-367">ASP.NET Core 1.1.0 hedef uygulamalar için veya daha yüksek [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcısı paketi olay izleme uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="598a2-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="598a2-368">Windows üzerinde kullandığı [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="598a2-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="598a2-369">Sağlayıcı için platformlar arası olsa da, toplama ve görüntüleme araçları Linux veya macOS için henüz hiç olay vardır.</span><span class="sxs-lookup"><span data-stu-id="598a2-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="598a2-372">Toplamak ve günlükleri görüntülemek için en iyi yolu kullanmaktır [PerfView yardımcı programı](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="598a2-372">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="598a2-373">ETW günlükleri görüntülemek için diğer araçları vardır, ancak PerfView ASP.NET tarafından gösterilen ETW olayları ile çalışmak için en iyi deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="598a2-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="598a2-374">Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView yapılandırmak için dizesi eklemek `*Microsoft-Extensions-Logging` için **ek sağlayıcılar** listesi.</span><span class="sxs-lookup"><span data-stu-id="598a2-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="598a2-375">(Dizenin başında yıldız kaçırmayın.)</span><span class="sxs-lookup"><span data-stu-id="598a2-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview ek sağlayıcılar](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="598a2-377">Windows olay günlüğü sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="598a2-377">Windows EventLog provider</span></span>

<span data-ttu-id="598a2-378">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcısı paketi için Windows olay günlüğü günlük çıkış gönderir.</span><span class="sxs-lookup"><span data-stu-id="598a2-378">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-379">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-379">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-380">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-380">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="598a2-381">[AddEventLog aşırı](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) geçirdiğiniz let `EventLogSettings` veya en küçük günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="598a2-381">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

### <a name="tracesource-provider"></a><span data-ttu-id="598a2-382">TraceSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="598a2-382">TraceSource provider</span></span>

<span data-ttu-id="598a2-383">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcısı paketi kullanan [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) kitaplıkları ve sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="598a2-383">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-384">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-384">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-385">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-385">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="598a2-386">[AddTraceSource aşırı](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) kaynak anahtarı ve İzleme dinleyicisi geçirdiğiniz izin verir.</span><span class="sxs-lookup"><span data-stu-id="598a2-386">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="598a2-387">Bu sağlayıcı kullanmak için .NET Framework (yerine .NET Core) çalıştırmak bir uygulama sahiptir.</span><span class="sxs-lookup"><span data-stu-id="598a2-387">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="598a2-388">Çeşitli için iletileri yönlendirmek sağlayıcı sağlar [dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners), gibi [olmalıdır](/dotnet/api/system.diagnostics.textwritertracelistenerr) örnek uygulama kullanılır.</span><span class="sxs-lookup"><span data-stu-id="598a2-388">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="598a2-389">Aşağıdaki örnek yapılandırır bir `TraceSource` oturum sağlayıcısı `Warning` ve konsol penceresine yüksek iletileri.</span><span class="sxs-lookup"><span data-stu-id="598a2-389">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="598a2-390">Azure uygulama hizmeti sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="598a2-390">Azure App Service provider</span></span>

<span data-ttu-id="598a2-391">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcısı paketi metin dosyalarına bir Azure uygulama hizmeti uygulamanın dosya sistemi ve çok günlükler Yazar [blob depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) bir Azure depolama hesabındaki.</span><span class="sxs-lookup"><span data-stu-id="598a2-391">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="598a2-392">Sağlayıcı veya üstünü ASP.NET Core 1.1 hedefleyen uygulamalar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="598a2-392">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="598a2-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="598a2-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="598a2-394">.NET Core hedefleme yok sağlayıcı paketini yüklemek veya açıkça çağırın [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="598a2-394">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="598a2-395">Azure App Service uygulama dağıtıldığında sağlayıcı uygulamaya otomatik olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="598a2-395">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="598a2-396">.NET Framework'ü hedefleme, sağlayıcı paketini projeye ekleyin ve çağırma `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="598a2-396">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="598a2-397">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="598a2-397">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="598a2-398">Bir [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) geçirdiğiniz sağlar aşırı [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) ile geçersiz kılabilirsiniz günlük çıkış şablonu, blob adı ve dosya gibi varsayılan ayarları boyut sınırını aştı.</span><span class="sxs-lookup"><span data-stu-id="598a2-398">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="598a2-399">(*Çıkış şablonu* çağırdığınızda sağlayan bir üstünde tüm günlükler için uygulanan bir ileti şablonu bir `ILogger` yöntemi.)</span><span class="sxs-lookup"><span data-stu-id="598a2-399">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="598a2-400">Bir uygulama hizmeti uygulama dağıttığınızda, uygulamayı ayarlarında yapılırken [tanılama günlüklerini](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) bölümünü **uygulama hizmeti** Azure portal sayfası.</span><span class="sxs-lookup"><span data-stu-id="598a2-400">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="598a2-401">Bu ayarları güncelleştirildiğinde, değişiklikler hemen geçerli bir yeniden başlatma veya yeniden dağıtım uygulamanın gerek kalmadan olur.</span><span class="sxs-lookup"><span data-stu-id="598a2-401">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure günlük ayarları](index/_static/azure-logging-settings.png)

<span data-ttu-id="598a2-403">Günlük dosyaları için varsayılan konum olarak *D:\\ev\\LogFiles\\uygulama* klasörü ve varsayılan dosya adı olan *tanılama yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="598a2-403">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="598a2-404">Varsayılan dosya boyutu sınırı 10 MB'tır ve korunan dosyaları varsayılan en yüksek sayısı 2'dir.</span><span class="sxs-lookup"><span data-stu-id="598a2-404">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="598a2-405">Varsayılan blob adı *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="598a2-405">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="598a2-406">Varsayılan davranış hakkında daha fazla bilgi için bkz: [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="598a2-406">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="598a2-407">Projeyi Azure ortamında çalıştığında sağlayıcısı yalnızca çalışır.</span><span class="sxs-lookup"><span data-stu-id="598a2-407">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="598a2-408">Projeyi yerel olarak çalıştırdığınızda hiçbir etkisi olmaz&mdash;yerel dosyalara veya yerel geliştirme depolama BLOB'lar için yazma değil.</span><span class="sxs-lookup"><span data-stu-id="598a2-408">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="598a2-409">Üçüncü taraf günlüğü sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="598a2-409">Third-party logging providers</span></span>

<span data-ttu-id="598a2-410">ASP.NET Core ile iş üçüncü taraf günlük altyapıları:</span><span class="sxs-lookup"><span data-stu-id="598a2-410">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="598a2-411">[elmah.io](https://elmah.io/) ([GitHub deposuna](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="598a2-411">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="598a2-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposuna](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="598a2-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="598a2-413">[JSNLog](http://jsnlog.com/) ([GitHub deposuna](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="598a2-413">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="598a2-414">[Loggr](http://loggr.net/) ([GitHub deposuna](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="598a2-414">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="598a2-415">[NLog](http://nlog-project.org/) ([GitHub deposuna](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="598a2-415">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="598a2-416">[Serilog](https://serilog.net/) ([GitHub deposuna](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="598a2-416">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="598a2-417">Bazı üçüncü taraf çerçeveleri gerçekleştirebilirsiniz [yapılandırılmış günlük olarak da bilinen semantik günlük](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="598a2-417">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="598a2-418">Bir üçüncü taraf framework kullanılarak yerleşik sağlayıcılar birini kullanmaya benzer:</span><span class="sxs-lookup"><span data-stu-id="598a2-418">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="598a2-419">NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="598a2-419">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="598a2-420">Bir genişletme yöntemi çağırmak `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="598a2-420">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="598a2-421">Daha fazla bilgi için her framework'ün belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="598a2-421">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="598a2-422">Akış azure günlük</span><span class="sxs-lookup"><span data-stu-id="598a2-422">Azure log streaming</span></span>

<span data-ttu-id="598a2-423">Azure günlük akış, gerçek zamanlı günlük etkinliği görüntülemenize olanak tanır:</span><span class="sxs-lookup"><span data-stu-id="598a2-423">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="598a2-424">Uygulama sunucusu</span><span class="sxs-lookup"><span data-stu-id="598a2-424">The application server</span></span>
* <span data-ttu-id="598a2-425">Web sunucusu</span><span class="sxs-lookup"><span data-stu-id="598a2-425">The web server</span></span>
* <span data-ttu-id="598a2-426">Başarısız istek izleme</span><span class="sxs-lookup"><span data-stu-id="598a2-426">Failed request tracing</span></span>

<span data-ttu-id="598a2-427">Azure günlük akış yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="598a2-427">To configure Azure log streaming:</span></span>

* <span data-ttu-id="598a2-428">Gidin **tanılama günlükleri** uygulamanızın portal sayfası sayfasından</span><span class="sxs-lookup"><span data-stu-id="598a2-428">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="598a2-429">Ayarlama **uygulama günlüğü (dosya sistemi)** için açık.</span><span class="sxs-lookup"><span data-stu-id="598a2-429">Set **Application Logging (Filesystem)** to on.</span></span>

![Azure portal tanılama günlüklerini sayfası](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="598a2-431">Gidin **günlük akış** uygulama iletilerini görüntülemek için sayfa.</span><span class="sxs-lookup"><span data-stu-id="598a2-431">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="598a2-432">Aracılığıyla uygulama tarafından oturum açtınız `ILogger` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="598a2-432">They're logged by application through the `ILogger` interface.</span></span>

![Akış azure portal uygulama günlüğü](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="598a2-434">Azure Application Insights izleme günlüğü</span><span class="sxs-lookup"><span data-stu-id="598a2-434">Azure Application Insights trace logging</span></span>

<span data-ttu-id="598a2-435">[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK ASP.NET Core günlük altyapısı oluşturulan günlüklerindeki izleme telemetri toplama özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="598a2-435">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="598a2-436">Daha fazla bilgi için bkz: [Applicationınsights/Microsoft-aspnetcore Wiki: günlüğü](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="598a2-436">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="598a2-437">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="598a2-437">Additional resources</span></span>

[<span data-ttu-id="598a2-438">Yüksek performans günlüğü LoggerMessage ile</span><span class="sxs-lookup"><span data-stu-id="598a2-438">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
