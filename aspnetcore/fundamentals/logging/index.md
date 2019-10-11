---
title: .NET Core ve ASP.NET Core oturum açma
author: rick-anderson
description: Microsoft. Extensions. Logging NuGet paketi tarafından sunulan günlüğe kaydetme çerçevesini nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 697e6cf0cd1b51ad6c2942e21bc084d1fe6bfa4e
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259738"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="a2ba9-103">.NET Core ve ASP.NET Core oturum açma</span><span class="sxs-lookup"><span data-stu-id="a2ba9-103">Logging in .NET Core and ASP.NET Core</span></span>

<span data-ttu-id="a2ba9-104">[Tom Dykstra](https://github.com/tdykstra) ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="a2ba9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a2ba9-105">.NET Core, çeşitli yerleşik ve üçüncü taraf oturum açma sağlayıcılarıyla birlikte çalışarak bir günlüğe kaydetme API 'sini destekler.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="a2ba9-106">Bu makalede, yerleşik sağlayıcılarla günlüğe kaydetme API 'sinin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-106">This article shows how to use the logging API with built-in providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2ba9-107">Bu makalede gösterilen kod örneklerinin çoğu ASP.NET Core uygulamalardan oluşur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="a2ba9-108">Bu kod parçacıklarının günlüğe kaydetmeye özgü bölümleri, [genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanan tüm .NET Core uygulamaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="a2ba9-109">Genel konağın Web 'de olmayan uygulamalarda kullanımı hakkında bilgi için bkz. [barındırılan hizmetler](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-109">For information about how to use the Generic Host in non-web console apps, see [Hosted services](xref:fundamentals/host/hosted-services).</span></span>

<span data-ttu-id="a2ba9-110">Genel ana bilgisayarı olmayan uygulamalar için günlük kodu, [sağlayıcıların Eklenme](#add-providers) ve [günlükçülerin oluşturulma](#create-logs)biçiminde farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="a2ba9-111">Ana bilgisayar olmayan kod örnekleri, makalenin bu bölümlerinde gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-111">Non-host code examples are shown in those sections of the article.</span></span>

::: moniker-end

<span data-ttu-id="a2ba9-112">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="a2ba9-113">Sağlayıcı Ekle</span><span class="sxs-lookup"><span data-stu-id="a2ba9-113">Add providers</span></span>

<span data-ttu-id="a2ba9-114">Günlük sağlayıcısı günlükleri görüntüler veya depolar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="a2ba9-115">Örneğin, konsol sağlayıcısı günlükleri konsolunda görüntüler ve Azure Application Insights sağlayıcısı bunları Azure Application Insights depolar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="a2ba9-116">Birden çok sağlayıcı eklenerek Günlükler birden çok hedefe gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2ba9-117">Genel ana bilgisayar kullanan bir uygulamaya sağlayıcı eklemek için, *program.cs*' de sağlayıcının `Add{provider name}` genişletme yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="a2ba9-118">Konak olmayan bir konsol uygulamasında, `LoggerFactory` oluştururken sağlayıcının `Add{provider name}` genişletme metodunu çağırın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="a2ba9-119">`LoggerFactory` ve `AddConsole` `Microsoft.Extensions.Logging` için `using` bir ifade gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="a2ba9-120">Varsayılan ASP.NET Core proje şablonları, aşağıdaki günlük sağlayıcılarını ekleyen <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> ' ı çağırır:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="a2ba9-121">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-121">Console</span></span>
* <span data-ttu-id="a2ba9-122">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-122">Debug</span></span>
* <span data-ttu-id="a2ba9-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="a2ba9-123">EventSource</span></span>
* <span data-ttu-id="a2ba9-124">Olay günlüğü (yalnızca Windows üzerinde çalışırken)</span><span class="sxs-lookup"><span data-stu-id="a2ba9-124">EventLog (only when running on Windows)</span></span>

<span data-ttu-id="a2ba9-125">Varsayılan sağlayıcıları kendi seçimlerinizle değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="a2ba9-126">@No__t-0 ' ı çağırın ve istediğiniz sağlayıcıları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0 "

<span data-ttu-id="a2ba9-127">Bir sağlayıcı eklemek için sağlayıcının `Add{provider name}` genişletme yöntemini *program.cs*içinde çağırın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-127">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="a2ba9-128">Yukarıdaki kod `Microsoft.Extensions.Logging` ve `Microsoft.Extensions.Configuration` başvurularını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-128">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="a2ba9-129">Varsayılan proje şablonu, aşağıdaki günlük sağlayıcılarını ekleyen <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> ' ı çağırır:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-129">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="a2ba9-130">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-130">Console</span></span>
* <span data-ttu-id="a2ba9-131">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-131">Debug</span></span>
* <span data-ttu-id="a2ba9-132">EventSource (ASP.NET Core 2,2 ' den başlayarak)</span><span class="sxs-lookup"><span data-stu-id="a2ba9-132">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="a2ba9-133">@No__t-0 kullanıyorsanız, varsayılan sağlayıcıları kendi seçimlerinizle değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-133">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="a2ba9-134">@No__t-0 ' ı çağırın ve istediğiniz sağlayıcıları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-134">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

<span data-ttu-id="a2ba9-135">Makalenin ilerleyen kısımlarında [yerleşik günlük sağlayıcıları](#built-in-logging-providers) ve [üçüncü taraf günlüğü sağlayıcıları](#third-party-logging-providers) hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-135">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="a2ba9-136">Günlükleri oluştur</span><span class="sxs-lookup"><span data-stu-id="a2ba9-136">Create logs</span></span>

<span data-ttu-id="a2ba9-137">Günlükler oluşturmak için <xref:Microsoft.Extensions.Logging.ILogger%601> nesnesi kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-137">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="a2ba9-138">Bir Web uygulamasında veya barındırılan hizmette, bağımlılık ekleme (DI) öğesinden bir `ILogger` alın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-138">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="a2ba9-139">Konak olmayan konsol uygulamalarında, bir @no__t oluşturmak için `LoggerFactory` ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-139">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="a2ba9-140">Aşağıdaki ASP.NET Core örnek kategori olarak `TodoApiSample.Pages.AboutModel` ile bir günlükçü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-140">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="a2ba9-141">Günlük *kategorisi* , her günlük ile ilişkili bir dizedir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-141">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="a2ba9-142">Dı tarafından sunulan `ILogger<T>` örneği, kategori olarak `T` türünde tam nitelikli bir ada sahip Günlükler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-142">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="a2ba9-143">Aşağıdaki konak olmayan konsol uygulaması örneği, kategori olarak `LoggingConsoleApp.Program` ile bir günlükçü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-143">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

::: moniker-end

<span data-ttu-id="a2ba9-144">Aşağıdaki ASP.NET Core ve konsol uygulaması örneklerinde, günlükçü, düzey olarak `Information` ile Günlükler oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-144">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="a2ba9-145">Günlük *düzeyi* günlüğe kaydedilen etkinliğin önem derecesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-145">The Log *level* indicates the severity of the logged event.</span></span> 

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

<span data-ttu-id="a2ba9-146">[Düzeyler](#log-level) ve [Kategoriler](#log-category) Bu makalenin ilerleyen kısımlarında daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-146">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-3.0"

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="a2ba9-147">Program sınıfında Günlükler oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2ba9-147">Create logs in the Program class</span></span>

<span data-ttu-id="a2ba9-148">Bir ASP.NET Core uygulamasının `Program` sınıfında Günlükler yazmak için, konak oluşturulduktan sonra dı 'den bir `ILogger` örneği alın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-148">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="a2ba9-149">Başlangıç sınıfında Günlükler oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2ba9-149">Create logs in the Startup class</span></span>

<span data-ttu-id="a2ba9-150">ASP.NET Core uygulamasının `Startup.Configure` yönteminde Günlükler yazmak için, yöntem imzasına bir `ILogger` parametresi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-150">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="a2ba9-151">@No__t-0 yönteminde DI kapsayıcısı kurulumu tamamlanmadan önce Günlüklerin yazılması desteklenmez:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-151">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="a2ba9-152">@No__t-0 oluşturucusuna günlükçü ekleme desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-152">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="a2ba9-153">@No__t-0 Yöntem imzasına günlükçü ekleme işlemi desteklenmiyor</span><span class="sxs-lookup"><span data-stu-id="a2ba9-153">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="a2ba9-154">Bu kısıtlamanın nedeni, günlük kaydının dı ve yapılandırmaya göre değişir ve bu da, ara ' ya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-154">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="a2ba9-155">@No__t-0 bitene kadar DI kapsayıcısı ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-155">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="a2ba9-156">Web ana bilgisayarı için ayrı bir DI kapsayıcısı oluşturulduğundan, bir günlükçü @no__t ' a bir günlükçü ekleme ASP.NET Core önceki sürümlerinde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-156">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="a2ba9-157">Genel ana bilgisayar için yalnızca bir kapsayıcı oluşturma hakkında daha fazla bilgi için bkz. [Son değişiklik duyurusu](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-157">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="a2ba9-158">@No__t-0 ' a bağlı bir hizmet yapılandırmanız gerekiyorsa, bunu Oluşturucu ekleme veya bir fabrika yöntemi sağlayarak de yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-158">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="a2ba9-159">Fabrika yöntemi yaklaşımı yalnızca başka bir seçenek yoksa önerilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-159">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="a2ba9-160">Örneğin, bir özelliği kaynağından bir hizmete doldurmanız gerektiğini varsayalım:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-160">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="a2ba9-161">Önceki vurgulanan kod, DI kapsayıcısının bir `MyService` örneği oluşturmak için ilk kez çalışan `Func` ' dır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-161">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="a2ba9-162">Kayıtlı hizmetlerden herhangi birine bu şekilde erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-162">You can access any of the registered services in this way.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="a2ba9-163">Başlangıçta günlük oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2ba9-163">Create logs in Startup</span></span>

<span data-ttu-id="a2ba9-164">@No__t-0 sınıfında Günlükler yazmak için, Oluşturucu imzasında bir `ILogger` parametresi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-164">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="a2ba9-165">Program sınıfında Günlükler oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2ba9-165">Create logs in the Program class</span></span>

<span data-ttu-id="a2ba9-166">@No__t-0 sınıfında Günlükler yazmak için, DI 'den bir `ILogger` örneği alın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-166">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="a2ba9-167">Zaman uyumsuz günlükçü yöntemi yok</span><span class="sxs-lookup"><span data-stu-id="a2ba9-167">No asynchronous logger methods</span></span>

<span data-ttu-id="a2ba9-168">Günlüğe kaydetme, zaman uyumsuz kodun performans maliyetine değer olmaması kadar hızlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-168">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="a2ba9-169">Günlüğe kaydetme veri depoluizin yavaşsa, doğrudan buna yazmayın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-169">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="a2ba9-170">Başlangıç olarak günlük iletilerini hızlı bir mağazaya yazmayı ve sonra yavaş depoya daha sonra taşımayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-170">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="a2ba9-171">Örneğin, SQL Server için oturum açıyorsanız, `Log` yöntemleri zaman uyumlu olduğundan, bunu doğrudan bir `Log` yönteminde yapmak istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-171">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="a2ba9-172">Bunun yerine, günlük iletilerini bir bellek içi kuyruğa eşzamanlı olarak ekleyin ve bir arka plan çalışanı, SQL Server veri gönderme zaman uyumsuz çalışmasını sağlamak için iletileri kuyruktan çekin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-172">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="a2ba9-173">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a2ba9-173">Configuration</span></span>

<span data-ttu-id="a2ba9-174">Günlüğe kaydetme sağlayıcısı yapılandırması bir veya daha fazla yapılandırma sağlayıcısı tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-174">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="a2ba9-175">Dosya biçimleri (ıNı, JSON ve XML).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-175">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="a2ba9-176">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-176">Command-line arguments.</span></span>
* <span data-ttu-id="a2ba9-177">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-177">Environment variables.</span></span>
* <span data-ttu-id="a2ba9-178">Bellek içi .NET nesneleri.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-178">In-memory .NET objects.</span></span>
* <span data-ttu-id="a2ba9-179">Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolaması.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-179">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="a2ba9-180">[Azure Key Vault](xref:security/key-vault-configuration)gibi şifreli bir kullanıcı deposu.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-180">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="a2ba9-181">Özel sağlayıcılar (yüklü veya oluşturulmuş).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-181">Custom providers (installed or created).</span></span>

<span data-ttu-id="a2ba9-182">Örneğin, günlük yapılandırma genellikle uygulama ayarları dosyalarının `Logging` bölümü tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-182">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="a2ba9-183">Aşağıdaki örnek tipik bir appSettings 'in içeriğini gösterir *. Development. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-183">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="a2ba9-184">@No__t-0 özelliği, `LogLevel` ve günlük sağlayıcısı özelliklerine sahip olabilir (konsol gösterilir).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-184">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="a2ba9-185">@No__t-1 altındaki `LogLevel` özelliği seçili kategoriler için günlüğe kaydedilecek minimum [düzeyi](#log-level) belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-185">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="a2ba9-186">Örnekte, `System` ve `Microsoft` kategorileri `Information` düzeyinde günlüğe kaydedilir ve tüm diğerleri `Debug` düzeyinde günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-186">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="a2ba9-187">@No__t-0 altındaki diğer özellikler günlük sağlayıcılarını belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-187">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="a2ba9-188">Örnek, konsol sağlayıcısına yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-188">The example is for the Console provider.</span></span> <span data-ttu-id="a2ba9-189">Bir sağlayıcı [günlük kapsamlarını](#log-scopes)destekliyorsa `IncludeScopes` ' in etkinleştirilip etkinleştirilmeyeceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-189">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="a2ba9-190">Bir sağlayıcı özelliği (örnekteki `Console` gibi), bir `LogLevel` özelliği de belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-190">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="a2ba9-191">bir sağlayıcı altındaki `LogLevel`, bu sağlayıcının günlüğe kaydedilecek düzeyleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-191">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="a2ba9-192">@No__t-0 ' da düzeyler belirtilmişse, `Logging.LogLevel` ' de ayarlanan her şeyi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-192">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

<span data-ttu-id="a2ba9-193">Yapılandırma sağlayıcılarını uygulama hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-193">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="a2ba9-194">Örnek günlüğe kaydetme çıkışı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-194">Sample logging output</span></span>

<span data-ttu-id="a2ba9-195">Yukarıdaki bölümde gösterilen örnek kodla, uygulama komut satırından çalıştırıldığında Günlükler konsolunda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-195">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="a2ba9-196">Konsol çıkışının bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-196">Here's an example of console output:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 84.26180000000001ms 307
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/api/todo/0
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

<span data-ttu-id="a2ba9-197">Yukarıdaki Günlükler `http://localhost:5000/api/todo/0` ' da örnek uygulamaya HTTP GET isteği yapılarak oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-197">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="a2ba9-198">Visual Studio 'da örnek uygulamayı çalıştırdığınızda hata ayıklama penceresinde göründükleri günlüklere yönelik bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-198">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request starting HTTP/2.0 GET https://localhost:44328/api/todo/0  
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
TodoApiSample.Controllers.TodoController: Information: Getting item 0
TodoApiSample.Controllers.TodoController: Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult: Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 34.167ms
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request finished in 98.41300000000001ms 404
```

<span data-ttu-id="a2ba9-199">Yukarıdaki bölümde gösterilen `ILogger` çağrıları tarafından oluşturulan Günlükler "TodoApiSample" ile başlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-199">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="a2ba9-200">"Microsoft" kategorileri ile başlayan Günlükler ASP.NET Core Framework kodundan alınır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-200">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="a2ba9-201">ASP.NET Core ve uygulama kodu aynı günlük API 'sini ve sağlayıcılarını kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-201">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="a2ba9-202">Yukarıdaki bölümde gösterilen `ILogger` çağrıları tarafından oluşturulan Günlükler "TodoApi" ile başlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-202">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="a2ba9-203">"Microsoft" kategorileri ile başlayan Günlükler ASP.NET Core Framework kodundan alınır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-203">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="a2ba9-204">ASP.NET Core ve uygulama kodu aynı günlük API 'sini ve sağlayıcılarını kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-204">ASP.NET Core and application code are using the same logging API and providers.</span></span>

::: moniker-end

<span data-ttu-id="a2ba9-205">Bu makalenin geri kalanında günlüğe kaydetme için bazı ayrıntılar ve seçenekler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-205">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="a2ba9-206">NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="a2ba9-206">NuGet packages</span></span>

<span data-ttu-id="a2ba9-207">@No__t-0 ve `ILoggerFactory` arabirimleri [Microsoft. Extensions. Logging. soyutlamalar](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)içinde bulunur ve bunlar için varsayılan uygulamalar [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-207">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="a2ba9-208">Günlük kategorisi</span><span class="sxs-lookup"><span data-stu-id="a2ba9-208">Log category</span></span>

<span data-ttu-id="a2ba9-209">@No__t-0 nesnesi oluşturulduğunda, kategorisi için bir *Kategori* belirtilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-209">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="a2ba9-210">Bu kategori, `ILogger` ' ın bu örneği tarafından oluşturulan her günlük iletisine dahildir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-210">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="a2ba9-211">Kategori herhangi bir dize olabilir, ancak kural, "TodoApi. Controllers. TodoController" gibi sınıf adını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-211">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="a2ba9-212">Kategori olarak `T` ' nin tam nitelikli tür adını kullanan bir `ILogger` örneği almak için `ILogger<T>` kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-212">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="a2ba9-213">Kategoriyi açıkça belirtmek için `ILoggerFactory.CreateLogger` ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-213">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="a2ba9-214">`ILogger<T>`, `T` ' nin tam nitelikli tür adıyla `CreateLogger` çağırma ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-214">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="a2ba9-215">Günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="a2ba9-215">Log level</span></span>

<span data-ttu-id="a2ba9-216">Her günlük bir <xref:Microsoft.Extensions.Logging.LogLevel> değeri belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-216">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="a2ba9-217">Günlük düzeyi önem derecesini veya önemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-217">The log level indicates the severity or importance.</span></span> <span data-ttu-id="a2ba9-218">Örneğin, bir yöntem *404 olmayan* bir durum kodu döndürdüğünde bir yöntem normal olarak sona erdiğinde bir `Information` günlüğü ve bir `Warning` günlüğü yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-218">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="a2ba9-219">Aşağıdaki kod `Information` ve `Warning` günlükleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-219">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="a2ba9-220">Yukarıdaki kodda, ilk parametre [günlük olay kimliğidir](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-220">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="a2ba9-221">İkinci parametre, kalan Yöntem parametreleri tarafından belirtilen bağımsız değişken değerleri için yer tutucuları olan bir ileti şablonudur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-221">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="a2ba9-222">Yöntem parametreleri bu makalenin ilerleyen kısımlarında bulunan [ileti şablonu bölümünde](#log-message-template) açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-222">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="a2ba9-223">Yöntem adındaki düzeyi (örneğin, `LogInformation` ve `LogWarning`) içeren günlük yöntemleri, [ILogger için uzantı yöntemleridir](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-223">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="a2ba9-224">Bu yöntemler, bir `LogLevel` parametresi alan `Log` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-224">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="a2ba9-225">Bu uzantı yöntemlerinden biri yerine doğrudan `Log` yöntemini çağırabilirsiniz, ancak söz dizimi görece karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-225">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="a2ba9-226">Daha fazla bilgi için bkz. <xref:Microsoft.Extensions.Logging.ILogger> ve [günlükçü uzantıları kaynak kodu](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-226">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="a2ba9-227">ASP.NET Core, en küçükten en yüksek öneme doğru sıralanan aşağıdaki günlük düzeylerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-227">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="a2ba9-228">İzleme = 0</span><span class="sxs-lookup"><span data-stu-id="a2ba9-228">Trace = 0</span></span>

  <span data-ttu-id="a2ba9-229">Genellikle yalnızca hata ayıklama için değerli bilgiler için.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-229">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="a2ba9-230">Bu iletiler hassas uygulama verileri içerebilir, bu nedenle bir üretim ortamında etkinleştirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-230">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="a2ba9-231">*Varsayılan olarak devre dışıdır.*</span><span class="sxs-lookup"><span data-stu-id="a2ba9-231">*Disabled by default.*</span></span>

* <span data-ttu-id="a2ba9-232">Hata Ayıkla = 1</span><span class="sxs-lookup"><span data-stu-id="a2ba9-232">Debug = 1</span></span>

  <span data-ttu-id="a2ba9-233">Geliştirme ve hata ayıklama konusunda yararlı olabilecek bilgiler için.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-233">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="a2ba9-234">Örnek: `Entering method Configure with flag set to true.`, yalnızca sorun giderirken, en yüksek günlük hacimden kaynaklanan `Debug` düzeyindeki günlükleri etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-234">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="a2ba9-235">Bilgi = 2</span><span class="sxs-lookup"><span data-stu-id="a2ba9-235">Information = 2</span></span>

  <span data-ttu-id="a2ba9-236">Uygulamanın genel akışını izlemek için.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-236">For tracking the general flow of the app.</span></span> <span data-ttu-id="a2ba9-237">Bu günlüklerde genellikle uzun süreli bir değer vardır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-237">These logs typically have some long-term value.</span></span> <span data-ttu-id="a2ba9-238">Örnek: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="a2ba9-238">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="a2ba9-239">Uyarı = 3</span><span class="sxs-lookup"><span data-stu-id="a2ba9-239">Warning = 3</span></span>

  <span data-ttu-id="a2ba9-240">Uygulama akışında anormal veya beklenmedik olaylar için.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-240">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="a2ba9-241">Bunlar, uygulamanın durmasına neden olmayan ancak araştırılması gerekebilecek hataları veya diğer koşulları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-241">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="a2ba9-242">İşlenmiş özel durumlar, `Warning` günlük düzeyini kullanmak için yaygın bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-242">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="a2ba9-243">Örnek: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="a2ba9-243">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="a2ba9-244">Hata = 4</span><span class="sxs-lookup"><span data-stu-id="a2ba9-244">Error = 4</span></span>

  <span data-ttu-id="a2ba9-245">İşlenemeyen hatalar ve özel durumlar için.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-245">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="a2ba9-246">Bu iletiler, uygulama genelinde bir hata değil geçerli etkinlikte veya işlemde (geçerli HTTP isteği gibi) bir hata olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-246">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="a2ba9-247">Örnek günlük iletisi: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="a2ba9-247">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="a2ba9-248">Kritik = 5</span><span class="sxs-lookup"><span data-stu-id="a2ba9-248">Critical = 5</span></span>

  <span data-ttu-id="a2ba9-249">Anında ilgilenilmesi gereken hatalarda.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-249">For failures that require immediate attention.</span></span> <span data-ttu-id="a2ba9-250">Örnekler: veri kaybı senaryoları, disk alanı yetersiz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-250">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="a2ba9-251">Belirli bir depolama ortamında veya görüntüleme penceresinde ne kadar günlük çıkışının yazıldığını denetlemek için günlük düzeyini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-251">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="a2ba9-252">Örnek:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-252">For example:</span></span>

* <span data-ttu-id="a2ba9-253">Üretimde:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-253">In production:</span></span>
  * <span data-ttu-id="a2ba9-254">@No__t-0 ile `Information` düzeylerinde günlüğe kaydetme, yüksek hacimli ayrıntılı günlük iletileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-254">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="a2ba9-255">Maliyetleri denetlemek ve veri depolama sınırlarını aşmamak için `Trace` ' ı `Information` düzeyindeki iletileri yüksek hacimli, düşük maliyetli bir veri deposuna günlüğe kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-255">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="a2ba9-256">@No__t-0 ' da `Critical` düzeylerinde günlüğe kaydetme işlemi genellikle daha az, daha küçük günlük iletileri üretir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-256">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="a2ba9-257">Bu nedenle, maliyetler ve depolama sınırları genellikle bir sorun değildir ve bu da veri deposu seçiminden daha fazla esneklik elde etmez.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-257">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="a2ba9-258">Geliştirme sırasında:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-258">During development:</span></span>
  * <span data-ttu-id="a2ba9-259">@No__t-0 ' dan konsola `Critical` iletileri ile günlüğe kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-259">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="a2ba9-260">Sorun giderirken `Information` iletileri aracılığıyla `Trace` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-260">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="a2ba9-261">Bu makalede daha sonra bulunan [günlük filtreleme](#log-filtering) bölümünde, bir sağlayıcının hangi günlük düzeylerinin işlediğini nasıl denetleneceği açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-261">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="a2ba9-262">ASP.NET Core çerçeve olayları için günlükleri yazar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-262">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="a2ba9-263">Bu makalede daha önce gelen günlük örnekleri, `Information` düzeyi altında Günlükler hariç tutulur, bu nedenle `Debug` veya `Trace` düzeyi günlük oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-263">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="a2ba9-264">Aşağıda, `Debug` günlüklerini göstermek için yapılandırılmış örnek uygulama çalıştırılarak oluşturulan konsol günlüklerinin bir örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-264">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

::: moniker range=">= aspnetcore-3.0"

```console
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of authorization filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of resource filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of action filters (in the following order): Microsoft.AspNetCore.Mvc.Filters.ControllerActionFilter (Order: -2147483648), Microsoft.AspNetCore.Mvc.ModelBinding.UnsupportedContentTypeFilter (Order: -3000)
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of exception filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of result filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[22]
      Attempting to bind parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[44]
      Attempting to bind parameter 'id' of type 'System.String' using the name 'id' in request data ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[45]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[23]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[26]
      Attempting to validate the bound parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[27]
      Done attempting to validate the bound parameter 'id' of type 'System.String'.
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[2]
      Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 32.690400000000004ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 176.9103ms 404
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

## <a name="log-event-id"></a><span data-ttu-id="a2ba9-265">Günlüğe olay KIMLIĞI</span><span class="sxs-lookup"><span data-stu-id="a2ba9-265">Log event ID</span></span>

<span data-ttu-id="a2ba9-266">Her günlük bir *olay kimliği*belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-266">Each log can specify an *event ID*.</span></span> <span data-ttu-id="a2ba9-267">Örnek uygulama bunu yerel olarak tanımlanmış bir `LoggingEvents` sınıfı kullanarak yapar:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-267">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="a2ba9-268">Olay KIMLIĞI bir olay kümesini ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-268">An event ID associates a set of events.</span></span> <span data-ttu-id="a2ba9-269">Örneğin, bir sayfadaki öğelerin listesini görüntülemek için ilgili tüm Günlükler 1001 olabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-269">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="a2ba9-270">Günlüğe kaydetme sağlayıcısı, olay KIMLIĞINI günlüğe kaydetme iletisindeki kimlik alanında veya hiç değil, bir kimlik alanında saklayabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-270">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="a2ba9-271">Hata ayıklama sağlayıcısı olay kimliklerini göstermiyor.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-271">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="a2ba9-272">Konsol sağlayıcısı, etkinlik kimliklerini kategoriden sonra parantez içinde gösterir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-272">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="a2ba9-273">Günlük iletisi şablonu</span><span class="sxs-lookup"><span data-stu-id="a2ba9-273">Log message template</span></span>

<span data-ttu-id="a2ba9-274">Her günlük bir ileti şablonunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-274">Each log specifies a message template.</span></span> <span data-ttu-id="a2ba9-275">İleti şablonu, bağımsız değişkenlerin sağlandığı yer tutucuları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-275">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="a2ba9-276">Sayılar değil, yer tutucular için adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-276">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="a2ba9-277">Adlarının, değerlerinin sağlanması için hangi parametrelerin kullanılacağını belirleyen yer tutucular sırası.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-277">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="a2ba9-278">Aşağıdaki kodda, parametre adlarının ileti şablonunda sıra dışı olduğuna dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-278">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="a2ba9-279">Bu kod, sırasıyla parametre değerleriyle bir günlük iletisi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-279">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="a2ba9-280">Günlüğe kaydetme altyapısı bu şekilde çalışarak, günlük sağlayıcılarının [yapılandırılmış günlüğe yazma olarak da bilinen anlamsal günlüğü](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)uygulayabilmesini sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-280">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="a2ba9-281">Bağımsız değişkenler yalnızca biçimli ileti şablonuna değil, günlük sistemine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-281">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="a2ba9-282">Bu bilgiler, günlük sağlayıcılarının parametre değerlerini alan olarak depolamasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-282">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="a2ba9-283">Örneğin, günlükçü yönteminin şuna benzer şekilde göründüğünü varsayın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-283">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="a2ba9-284">Günlükleri Azure Tablo depolama alanına gönderiyorsanız, her bir Azure Tablo varlığının `ID` ve `RequestTime` özellikleri olabilir ve bu, günlük verilerinde sorguları basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-284">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="a2ba9-285">Bir sorgu belirli bir `RequestTime` aralığındaki tüm günlükleri metin iletisinden zaman aşımı olmadan bulabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-285">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="a2ba9-286">Günlüğe kaydetme özel durumları</span><span class="sxs-lookup"><span data-stu-id="a2ba9-286">Logging exceptions</span></span>

<span data-ttu-id="a2ba9-287">Günlükçü yöntemlerinin, aşağıdaki örnekte olduğu gibi bir özel durum iletmenizi sağlayan aşırı yüklemeleri vardır:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-287">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="a2ba9-288">Özel durum bilgileri farklı yollarla farklı sağlayıcılarda işler.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-288">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="a2ba9-289">Yukarıda gösterilen koddan hata ayıklama sağlayıcısı çıktısına bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-289">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="a2ba9-290">Günlük filtreleme</span><span class="sxs-lookup"><span data-stu-id="a2ba9-290">Log filtering</span></span>

<span data-ttu-id="a2ba9-291">Belirli bir sağlayıcı ve kategori için en az bir günlük düzeyi veya tüm sağlayıcılar ya da tüm kategoriler için belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-291">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="a2ba9-292">Minimum düzeyin altındaki tüm Günlükler bu sağlayıcıya aktarılmaz, bu nedenle görüntülenmez veya depolanmaz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-292">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="a2ba9-293">Tüm günlükleri gizlemek için en düşük günlük düzeyi olarak `LogLevel.None` belirtin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-293">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="a2ba9-294">@No__t-0 ' ın tamsayı değeri 6 ' dır ve `LogLevel.Critical` ' den (5) daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-294">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="a2ba9-295">Yapılandırmada filtre kuralları oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2ba9-295">Create filter rules in configuration</span></span>

<span data-ttu-id="a2ba9-296">Proje şablonu kodu, konsol ve hata ayıklama sağlayıcılarının günlüğünü ayarlamak için `CreateDefaultBuilder` ' yı çağırır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-296">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="a2ba9-297">@No__t-0 yöntemi, [Bu makalenin önceki kısımlarında](#configuration)açıklandığı gibi bir `Logging` bölümünde yapılandırma aramak için günlüğe kaydetmeyi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-297">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="a2ba9-298">Yapılandırma verileri aşağıdaki örnekte olduğu gibi sağlayıcıya ve kategoriye göre en düşük günlük düzeylerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-298">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

::: moniker-end

<span data-ttu-id="a2ba9-299">Bu JSON altı filtre kuralı oluşturur: biri hata ayıklama sağlayıcısı, konsol sağlayıcısı için dört ve diğeri tüm sağlayıcılar için.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-299">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="a2ba9-300">@No__t-0 nesnesi oluşturulduğunda her sağlayıcı için tek bir kural seçilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-300">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="a2ba9-301">Koddaki filtre kuralları</span><span class="sxs-lookup"><span data-stu-id="a2ba9-301">Filter rules in code</span></span>

<span data-ttu-id="a2ba9-302">Aşağıdaki örnek, koddaki filtre kurallarının nasıl kaydedileceği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-302">The following example shows how to register filter rules in code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

::: moniker-end

<span data-ttu-id="a2ba9-303">İkinci `AddFilter`, tür adını kullanarak hata ayıklama sağlayıcısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-303">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="a2ba9-304">İlk `AddFilter` bir sağlayıcı türü belirtmediğinden tüm sağlayıcılar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-304">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="a2ba9-305">Filtreleme kuralları nasıl uygulanır</span><span class="sxs-lookup"><span data-stu-id="a2ba9-305">How filtering rules are applied</span></span>

<span data-ttu-id="a2ba9-306">Yukarıdaki örneklerde gösterilen yapılandırma verileri ve `AddFilter` kodu, aşağıdaki tabloda gösterilen kuralları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-306">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="a2ba9-307">İlk altı yapılandırma örneğinde ve son iki ise kod örneğinde gelir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-307">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="a2ba9-308">Sayı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-308">Number</span></span> | <span data-ttu-id="a2ba9-309">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-309">Provider</span></span>      | <span data-ttu-id="a2ba9-310">Şununla başlayan Kategoriler...</span><span class="sxs-lookup"><span data-stu-id="a2ba9-310">Categories that begin with ...</span></span>          | <span data-ttu-id="a2ba9-311">En düşük günlük düzeyi</span><span class="sxs-lookup"><span data-stu-id="a2ba9-311">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="a2ba9-312">1</span><span class="sxs-lookup"><span data-stu-id="a2ba9-312">1</span></span>      | <span data-ttu-id="a2ba9-313">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-313">Debug</span></span>         | <span data-ttu-id="a2ba9-314">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="a2ba9-314">All categories</span></span>                          | <span data-ttu-id="a2ba9-315">Bilgi</span><span class="sxs-lookup"><span data-stu-id="a2ba9-315">Information</span></span>       |
| <span data-ttu-id="a2ba9-316">2</span><span class="sxs-lookup"><span data-stu-id="a2ba9-316">2</span></span>      | <span data-ttu-id="a2ba9-317">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-317">Console</span></span>       | <span data-ttu-id="a2ba9-318">Microsoft. AspNetCore. Mvc. Razor. Internal</span><span class="sxs-lookup"><span data-stu-id="a2ba9-318">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="a2ba9-319">Uyarı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-319">Warning</span></span>           |
| <span data-ttu-id="a2ba9-320">3</span><span class="sxs-lookup"><span data-stu-id="a2ba9-320">3</span></span>      | <span data-ttu-id="a2ba9-321">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-321">Console</span></span>       | <span data-ttu-id="a2ba9-322">Microsoft. AspNetCore. Mvc. Razor. Razor</span><span class="sxs-lookup"><span data-stu-id="a2ba9-322">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="a2ba9-323">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-323">Debug</span></span>             |
| <span data-ttu-id="a2ba9-324">4</span><span class="sxs-lookup"><span data-stu-id="a2ba9-324">4</span></span>      | <span data-ttu-id="a2ba9-325">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-325">Console</span></span>       | <span data-ttu-id="a2ba9-326">Microsoft. AspNetCore. Mvc. Razor</span><span class="sxs-lookup"><span data-stu-id="a2ba9-326">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="a2ba9-327">Hata</span><span class="sxs-lookup"><span data-stu-id="a2ba9-327">Error</span></span>             |
| <span data-ttu-id="a2ba9-328">5</span><span class="sxs-lookup"><span data-stu-id="a2ba9-328">5</span></span>      | <span data-ttu-id="a2ba9-329">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-329">Console</span></span>       | <span data-ttu-id="a2ba9-330">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="a2ba9-330">All categories</span></span>                          | <span data-ttu-id="a2ba9-331">Bilgi</span><span class="sxs-lookup"><span data-stu-id="a2ba9-331">Information</span></span>       |
| <span data-ttu-id="a2ba9-332">6</span><span class="sxs-lookup"><span data-stu-id="a2ba9-332">6</span></span>      | <span data-ttu-id="a2ba9-333">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="a2ba9-333">All providers</span></span> | <span data-ttu-id="a2ba9-334">Tüm kategoriler</span><span class="sxs-lookup"><span data-stu-id="a2ba9-334">All categories</span></span>                          | <span data-ttu-id="a2ba9-335">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-335">Debug</span></span>             |
| <span data-ttu-id="a2ba9-336">7</span><span class="sxs-lookup"><span data-stu-id="a2ba9-336">7</span></span>      | <span data-ttu-id="a2ba9-337">Tüm sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="a2ba9-337">All providers</span></span> | <span data-ttu-id="a2ba9-338">Sistem</span><span class="sxs-lookup"><span data-stu-id="a2ba9-338">System</span></span>                                  | <span data-ttu-id="a2ba9-339">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-339">Debug</span></span>             |
| <span data-ttu-id="a2ba9-340">8</span><span class="sxs-lookup"><span data-stu-id="a2ba9-340">8</span></span>      | <span data-ttu-id="a2ba9-341">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-341">Debug</span></span>         | <span data-ttu-id="a2ba9-342">Microsoft</span><span class="sxs-lookup"><span data-stu-id="a2ba9-342">Microsoft</span></span>                               | <span data-ttu-id="a2ba9-343">İzleme</span><span class="sxs-lookup"><span data-stu-id="a2ba9-343">Trace</span></span>             |

<span data-ttu-id="a2ba9-344">@No__t-0 nesnesi oluşturulduğunda `ILoggerFactory` nesnesi, bu günlükçü için uygulanacak her sağlayıcı için tek bir kural seçer.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-344">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="a2ba9-345">Bir @no__t 0 örneği tarafından yazılan tüm iletiler, seçilen kurallara göre filtrelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-345">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="a2ba9-346">Her sağlayıcı ve kategori çifti için mümkün olan en özel kural kullanılabilir kurallardan seçilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-346">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="a2ba9-347">Belirli bir kategori için `ILogger` oluşturulduğunda her bir sağlayıcı için aşağıdaki algoritma kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-347">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="a2ba9-348">Sağlayıcı veya diğer adıyla eşleşen tüm kuralları seçin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-348">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="a2ba9-349">Hiçbir eşleşme bulunmazsa, boş bir sağlayıcıya sahip tüm kurallar ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-349">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="a2ba9-350">Önceki adımın sonucunda, en uzun eşleşen kategori ön ekine sahip kurallar ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-350">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="a2ba9-351">Eşleşme bulunmazsa, kategori belirtmeyen tüm kuralları seçin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-351">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="a2ba9-352">Birden çok kural seçilirse, **son** olanı götürün.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-352">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="a2ba9-353">Hiçbir kural seçilmezse `MinimumLevel` kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-353">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="a2ba9-354">Yukarıdaki kurallar listesinde, "Microsoft. AspNetCore. Mvc. Razor. RazorViewEngine" kategorisi için `ILogger` nesnesi oluşturduğunuzu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-354">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="a2ba9-355">Hata ayıklama sağlayıcısı, kurallar 1, 6 ve 8 için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-355">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="a2ba9-356">Kural 8 ' i en özeldir, yani seçili olanı seçilidir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-356">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="a2ba9-357">Konsol sağlayıcısı için, kurallar 3, 4, 5 ve 6 geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-357">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="a2ba9-358">Kural 3 en özeldir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-358">Rule 3 is most specific.</span></span>

<span data-ttu-id="a2ba9-359">Elde edilen `ILogger` örneği, hata ayıklama sağlayıcısına `Trace` düzeyi ve üzeri Günlükler gönderir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-359">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="a2ba9-360">@No__t-0 düzeyi ve üzeri Günlükler konsol sağlayıcısına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-360">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="a2ba9-361">Sağlayıcı diğer adları</span><span class="sxs-lookup"><span data-stu-id="a2ba9-361">Provider aliases</span></span>

<span data-ttu-id="a2ba9-362">Her sağlayıcı, tam nitelikli tür adı yerine yapılandırmada kullanılabilecek bir *diğer ad* tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-362">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="a2ba9-363">Yerleşik sağlayıcılar için aşağıdaki diğer adları kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-363">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="a2ba9-364">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-364">Console</span></span>
* <span data-ttu-id="a2ba9-365">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-365">Debug</span></span>
* <span data-ttu-id="a2ba9-366">EventSource</span><span class="sxs-lookup"><span data-stu-id="a2ba9-366">EventSource</span></span>
* <span data-ttu-id="a2ba9-367">EventLog</span><span class="sxs-lookup"><span data-stu-id="a2ba9-367">EventLog</span></span>
* <span data-ttu-id="a2ba9-368">TraceSource</span><span class="sxs-lookup"><span data-stu-id="a2ba9-368">TraceSource</span></span>
* <span data-ttu-id="a2ba9-369">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="a2ba9-369">AzureAppServicesFile</span></span>
* <span data-ttu-id="a2ba9-370">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="a2ba9-370">AzureAppServicesBlob</span></span>
* <span data-ttu-id="a2ba9-371">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="a2ba9-371">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="a2ba9-372">Varsayılan en düşük düzey</span><span class="sxs-lookup"><span data-stu-id="a2ba9-372">Default minimum level</span></span>

<span data-ttu-id="a2ba9-373">Yalnızca belirli bir sağlayıcı ve kategori için yapılandırma veya koddan kural uygulanmaz geçerli olan en düşük düzey ayar vardır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-373">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="a2ba9-374">Aşağıdaki örnekte, en düşük düzeyin nasıl ayarlanacağı gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-374">The following example shows how to set the minimum level:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

::: moniker-end

<span data-ttu-id="a2ba9-375">En düşük düzeyi açıkça ayarlamazsanız, varsayılan değer `Information` ' dır. Bu, `Trace` ve `Debug` günlüklerinin yoksayıldığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-375">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="a2ba9-376">Filtre işlevleri</span><span class="sxs-lookup"><span data-stu-id="a2ba9-376">Filter functions</span></span>

<span data-ttu-id="a2ba9-377">Configuration veya Code tarafından kendisine atanmış kuralları olmayan tüm sağlayıcılar ve kategoriler için bir filtre işlevi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-377">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="a2ba9-378">İşlevindeki kodun sağlayıcı türü, kategorisi ve günlük düzeyine erişimi vardır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-378">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="a2ba9-379">Örnek:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-379">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="a2ba9-380">Sistem kategorileri ve Düzeyler</span><span class="sxs-lookup"><span data-stu-id="a2ba9-380">System categories and levels</span></span>

<span data-ttu-id="a2ba9-381">ASP.NET Core ve Entity Framework Core tarafından kullanılan bazı kategoriler şunlardır ve bunlardan beklenen Günlükler hakkında notlar bulunur:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-381">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="a2ba9-382">Kategori</span><span class="sxs-lookup"><span data-stu-id="a2ba9-382">Category</span></span>                            | <span data-ttu-id="a2ba9-383">Notlar</span><span class="sxs-lookup"><span data-stu-id="a2ba9-383">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="a2ba9-384">Microsoft. AspNetCore</span><span class="sxs-lookup"><span data-stu-id="a2ba9-384">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="a2ba9-385">Genel ASP.NET Core tanılama.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-385">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="a2ba9-386">Microsoft. AspNetCore. DataProtection</span><span class="sxs-lookup"><span data-stu-id="a2ba9-386">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="a2ba9-387">Hangi anahtarların kabul edildiği, bulunduğu ve kullanıldığı.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-387">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="a2ba9-388">Microsoft. AspNetCore. HostFiltering</span><span class="sxs-lookup"><span data-stu-id="a2ba9-388">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="a2ba9-389">İzin verilen konaklar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-389">Hosts allowed.</span></span> |
| <span data-ttu-id="a2ba9-390">Microsoft. AspNetCore. Hosting</span><span class="sxs-lookup"><span data-stu-id="a2ba9-390">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="a2ba9-391">HTTP isteklerinin tamamlanması için geçen süre ve ne zaman başladıkları.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-391">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="a2ba9-392">Hangi barındırma başlangıç derlemeleri yüklendi.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-392">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="a2ba9-393">Microsoft. AspNetCore. Mvc</span><span class="sxs-lookup"><span data-stu-id="a2ba9-393">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="a2ba9-394">MVC ve Razor tanılama.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-394">MVC and Razor diagnostics.</span></span> <span data-ttu-id="a2ba9-395">Model bağlama, filtre yürütme, derlemeyi görüntüleme, eylem seçimi.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-395">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="a2ba9-396">Microsoft. AspNetCore. Routing</span><span class="sxs-lookup"><span data-stu-id="a2ba9-396">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="a2ba9-397">Eşleşen bilgileri yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-397">Route matching information.</span></span> |
| <span data-ttu-id="a2ba9-398">Microsoft. AspNetCore. Server</span><span class="sxs-lookup"><span data-stu-id="a2ba9-398">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="a2ba9-399">Bağlantı başlatın, durdurun ve canlı yanıtları koruyun.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-399">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="a2ba9-400">HTTPS sertifika bilgileri.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-400">HTTPS certificate information.</span></span> |
| <span data-ttu-id="a2ba9-401">Microsoft. AspNetCore. StaticFiles</span><span class="sxs-lookup"><span data-stu-id="a2ba9-401">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="a2ba9-402">Sunulan dosyalar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-402">Files served.</span></span> |
| <span data-ttu-id="a2ba9-403">Microsoft. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="a2ba9-403">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="a2ba9-404">Genel Entity Framework Core tanılama.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-404">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="a2ba9-405">Veritabanı etkinliği ve yapılandırması, değişiklik algılama, geçişler.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-405">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="a2ba9-406">Günlük kapsamları</span><span class="sxs-lookup"><span data-stu-id="a2ba9-406">Log scopes</span></span>

 <span data-ttu-id="a2ba9-407">*Kapsam* bir mantıksal işlemler kümesini gruplandırabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-407">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="a2ba9-408">Bu gruplandırma, kümenin bir parçası olarak oluşturulan her günlüğe aynı verileri eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-408">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="a2ba9-409">Örneğin, bir işlemin işlenmesi kapsamında oluşturulan her günlük işlem KIMLIĞI içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-409">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="a2ba9-410">Kapsam, <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> yöntemi tarafından döndürülen `IDisposable` türüdür ve atılana kadar sürer.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-410">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="a2ba9-411">@No__t-0 bloğunda günlükçü çağrılarını sarmalayarak kapsam kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-411">Use a scope by wrapping logger calls in a `using` block:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

::: moniker-end

<span data-ttu-id="a2ba9-412">Aşağıdaki kod konsol sağlayıcısı için kapsamları etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-412">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="a2ba9-413">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-413">*Program.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="a2ba9-414">@No__t-0 konsol günlükçü seçeneğini yapılandırmak, kapsam tabanlı günlüğe kaydetmeyi etkinleştirmek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-414">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="a2ba9-415">Yapılandırma hakkında bilgi için [yapılandırma](#configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-415">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="a2ba9-416">Her günlük iletisi kapsamlı bilgiler içerir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-416">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="a2ba9-417">Yerleşik günlük oluşturma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="a2ba9-417">Built-in logging providers</span></span>

<span data-ttu-id="a2ba9-418">ASP.NET Core aşağıdaki sağlayıcıları sevk eder:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-418">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="a2ba9-419">Console</span><span class="sxs-lookup"><span data-stu-id="a2ba9-419">Console</span></span>](#console-provider)
* [<span data-ttu-id="a2ba9-420">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2ba9-420">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="a2ba9-421">EventSource</span><span class="sxs-lookup"><span data-stu-id="a2ba9-421">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="a2ba9-422">EventLog</span><span class="sxs-lookup"><span data-stu-id="a2ba9-422">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="a2ba9-423">TraceSource</span><span class="sxs-lookup"><span data-stu-id="a2ba9-423">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="a2ba9-424">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="a2ba9-424">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="a2ba9-425">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="a2ba9-425">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="a2ba9-426">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="a2ba9-426">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="a2ba9-427">ASP.NET Core modülüyle stdout ve hata ayıklama günlüğü hakkında daha fazla bilgi için, bkz. <xref:test/troubleshoot-azure-iis> ve <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-427">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="a2ba9-428">Konsol sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-428">Console provider</span></span>

<span data-ttu-id="a2ba9-429">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sağlayıcı paketi, günlük çıktısını konsola gönderir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-429">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="a2ba9-430">Konsol günlüğü çıkışını görmek için proje klasöründe bir komut istemi açın ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-430">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="a2ba9-431">Hata ayıklama sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-431">Debug provider</span></span>

<span data-ttu-id="a2ba9-432">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) sağlayıcı paketi, [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug) sınıfını (`Debug.WriteLine` Yöntem çağrıları) kullanarak günlük çıktısını yazar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-432">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="a2ba9-433">Linux 'ta, bu sağlayıcı günlükleri */var/log/Message*dosyasına yazar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-433">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="eventsource-provider"></a><span data-ttu-id="a2ba9-434">EventSource sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-434">EventSource provider</span></span>

<span data-ttu-id="a2ba9-435">ASP.NET Core 1.1.0 veya üzeri bir sürümü hedefleyen uygulamalar için, [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) sağlayıcı paketi olay izlemeyi uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-435">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="a2ba9-436">Windows üzerinde [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-436">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="a2ba9-437">Sağlayıcı platformlar arası, ancak Linux veya macOS için henüz bir olay koleksiyonu ve görüntüleme aracı yok.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-437">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="a2ba9-438">Günlükleri toplamanın ve görüntülemenin iyi bir yolu, [PerfView yardımcı programını](https://github.com/Microsoft/perfview)kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-438">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="a2ba9-439">ETW günlüklerini görüntülemeye yönelik başka araçlar da mevcuttur, ancak PerfView, ASP.NET Core tarafından yayınlanan ETW olaylarıyla çalışmak için en iyi deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-439">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="a2ba9-440">Bu sağlayıcı tarafından günlüğe kaydedilen olayları toplamak için PerfView 'ı yapılandırmak için, **ek sağlayıcılar** listesine `*Microsoft-Extensions-Logging` dizesini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-440">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="a2ba9-441">(Dizenin başlangıcında yıldız işaretini kaçırmayın.)</span><span class="sxs-lookup"><span data-stu-id="a2ba9-441">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView ek sağlayıcıları](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="a2ba9-443">Windows olay günlüğü sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-443">Windows EventLog provider</span></span>

<span data-ttu-id="a2ba9-444">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sağlayıcı paketi, Windows olay günlüğüne günlük çıktısı gönderir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-444">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="a2ba9-445">[AddEventLog aşırı yüklemeler](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings> ' de geçiş yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-445">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span>

### <a name="tracesource-provider"></a><span data-ttu-id="a2ba9-446">TraceSource sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-446">TraceSource provider</span></span>

<span data-ttu-id="a2ba9-447">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) sağlayıcı paketi <xref:System.Diagnostics.TraceSource> kitaplıklarını ve sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-447">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="a2ba9-448">[Addtracesource aşırı yüklemeleri](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) , bir kaynak anahtarı ve bir izleme dinleyicisi geçirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-448">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="a2ba9-449">Bu sağlayıcıyı kullanmak için, bir uygulamanın .NET Framework çalışması gerekir (.NET Core yerine).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-449">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="a2ba9-450">Sağlayıcı, iletileri örnek uygulamada kullanılan <xref:System.Diagnostics.TextWriterTraceListener> gibi çeşitli [dinleyicilerine](/dotnet/framework/debug-trace-profile/trace-listeners)yönlendirebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-450">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="a2ba9-451">Azure App Service sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-451">Azure App Service provider</span></span>

<span data-ttu-id="a2ba9-452">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcı paketi, günlükleri bir Azure App Service uygulamasının dosya sistemindeki metin dosyalarına ve bir Azure depolama hesabındaki [BLOB depolama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) alanına yazar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-452">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2ba9-453">Sağlayıcı paketi, paylaşılan çerçeveye dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-453">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="a2ba9-454">Sağlayıcıyı kullanmak için sağlayıcı paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-454">To use the provider, add the provider package to the project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="a2ba9-455">Sağlayıcı paketi [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-455">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="a2ba9-456">.NET Framework veya `Microsoft.AspNetCore.App` metapackage ile başvurulduğunda, sağlayıcı paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-456">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2ba9-457">Sağlayıcı ayarlarını yapılandırmak için aşağıdaki örnekte gösterildiği gibi <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> ve <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions> kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-457">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a2ba9-458">Sağlayıcı ayarlarını yapılandırmak için aşağıdaki örnekte gösterildiği gibi <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> ve <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions> kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="a2ba9-459">@No__t-0 aşırı yüklemesi, <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings> ' de geçiş yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-459">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="a2ba9-460">Ayarlar nesnesi, günlük çıkış şablonu, blob adı ve dosya boyutu sınırı gibi varsayılan ayarları geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-460">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="a2ba9-461">(*Çıktı şablonu* , `ILogger` yöntem çağrısıyla birlikte sunulan tümüne ek olarak tüm günlüklere uygulanan bir ileti şablonudur.)</span><span class="sxs-lookup"><span data-stu-id="a2ba9-461">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

<span data-ttu-id="a2ba9-462">App Service uygulamasına dağıtırken, uygulama, Azure portal **App Service** sayfasının [App Service Günlükler](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) bölümündeki ayarları kabul eder.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-462">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="a2ba9-463">Aşağıdaki ayarlar güncelleştirilirken, değişiklikler uygulamanın yeniden başlatılmasını veya yeniden dağıtımı gerekmeden hemen etkili olur.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-463">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="a2ba9-464">**Uygulama günlüğü (dosya sistemi)**</span><span class="sxs-lookup"><span data-stu-id="a2ba9-464">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="a2ba9-465">**Uygulama günlüğü (blob)**</span><span class="sxs-lookup"><span data-stu-id="a2ba9-465">**Application Logging (Blob)**</span></span>

<span data-ttu-id="a2ba9-466">Günlük dosyaları için varsayılan konum *D: \\home @ no__t-2LogFiles @ no__t-3Application* klasöründedir ve varsayılan dosya adı *Diagnostics-YYYYMMDD. txt*' dir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-466">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="a2ba9-467">Varsayılan dosya boyutu sınırı 10 MB 'tır ve tutulan varsayılan en fazla dosya sayısı 2 ' dir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-467">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="a2ba9-468">Varsayılan blob adı *{app-name} {timestamp}/yyyy/mm/dd/ss/{Guid}-ApplicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-468">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="a2ba9-469">Sağlayıcı yalnızca proje Azure ortamında çalıştırıldığında çalışır.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-469">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="a2ba9-470">Proje yerel olarak çalıştırıldığında herhangi bir etkisi yoktur @ no__t-0bu, yerel dosyalara veya blob 'lar için yerel geliştirme depolamasına yazmaz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-470">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="a2ba9-471">Azure günlük akışı</span><span class="sxs-lookup"><span data-stu-id="a2ba9-471">Azure log streaming</span></span>

<span data-ttu-id="a2ba9-472">Azure günlük akışı, günlük etkinliklerini gerçek zamanlı olarak görüntülemenize izin verir:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-472">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="a2ba9-473">Uygulama sunucusu</span><span class="sxs-lookup"><span data-stu-id="a2ba9-473">The app server</span></span>
* <span data-ttu-id="a2ba9-474">Web sunucusu</span><span class="sxs-lookup"><span data-stu-id="a2ba9-474">The web server</span></span>
* <span data-ttu-id="a2ba9-475">Başarısız istek izleme</span><span class="sxs-lookup"><span data-stu-id="a2ba9-475">Failed request tracing</span></span>

<span data-ttu-id="a2ba9-476">Azure günlük akışını yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-476">To configure Azure log streaming:</span></span>

* <span data-ttu-id="a2ba9-477">Uygulamanızın Portal sayfasından **App Service günlükleri** sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-477">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="a2ba9-478">**Uygulama günlüğünü (FileSystem)** **Açık**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-478">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="a2ba9-479">Günlük **düzeyini**seçin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-479">Choose the log **Level**.</span></span>

<span data-ttu-id="a2ba9-480">Uygulama iletilerini görüntülemek için **günlük akışı** sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-480">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="a2ba9-481">@No__t-0 arabirimi aracılığıyla uygulama tarafından günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-481">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="a2ba9-482">Azure Application Insights izleme günlüğü</span><span class="sxs-lookup"><span data-stu-id="a2ba9-482">Azure Application Insights trace logging</span></span>

<span data-ttu-id="a2ba9-483">[Microsoft. Extensions. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) sağlayıcı paketi günlükleri Azure Application Insights yazar.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-483">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="a2ba9-484">Application Insights, bir Web uygulamasını izleyen ve telemetri verilerini sorgulamak ve analiz etmek için araçlar sağlayan bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-484">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="a2ba9-485">Bu sağlayıcıyı kullanıyorsanız, Application Insights araçlarını kullanarak günlüklerinizi sorgulayabilir ve analiz edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-485">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="a2ba9-486">Günlüğe kaydetme sağlayıcısı, ASP.NET Core için tüm kullanılabilir telemetri sağlayan paket olan [Microsoft. ApplicationInsights. AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore)'un bağımlılığı olarak eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-486">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="a2ba9-487">Bu paketi kullanırsanız, sağlayıcı paketini yüklemek zorunda kalmazsınız.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-487">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="a2ba9-488">ASP.NET 4. x için olan [Microsoft. ApplicationInsights. Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) paketi @ no__t-1' i kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-488">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="a2ba9-489">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-489">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a2ba9-490">Application Insights genel bakış</span><span class="sxs-lookup"><span data-stu-id="a2ba9-490">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="a2ba9-491">[ASP.NET Core uygulamalar için Application Insights](/azure/azure-monitor/app/asp-net-core) -günlük kaydı ile birlikte Application Insights telemetrinin tam aralığını uygulamak istiyorsanız buraya başlayın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-491">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="a2ba9-492">[.NET Core ILogger günlükleri Için Applicationınsightsloggerprovider](/azure/azure-monitor/app/ilogger) -günlük sağlayıcısını Application Insights telemetri olmadan uygulamak istiyorsanız buraya başlayın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-492">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="a2ba9-493">[Günlüğe kaydetme bağdaştırıcılarını Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="a2ba9-493">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="a2ba9-494">Microsoft Learn sitede Application Insights SDK-etkileşimli öğreticisini [yükleyip başlatın](/learn/modules/instrument-web-app-code-with-application-insights) .</span><span class="sxs-lookup"><span data-stu-id="a2ba9-494">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="a2ba9-495">Üçüncü taraf günlük oluşturma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="a2ba9-495">Third-party logging providers</span></span>

<span data-ttu-id="a2ba9-496">ASP.NET Core ile birlikte çalışan üçüncü taraf günlük çerçeveleri:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-496">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="a2ba9-497">[ELMAH.io](https://elmah.io/) ([GitHub deposu](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-497">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="a2ba9-498">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub deposu](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-498">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="a2ba9-499">[Jsnlog](https://jsnlog.com/) ([GitHub deposu](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-499">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="a2ba9-500">[KissLog.net](https://kisslog.net/) ([GitHub deposu](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-500">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="a2ba9-501">[Log4Net](https://logging.apache.org/log4net/) ([GitHub deposu](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-501">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="a2ba9-502">[Loggr](https://loggr.net/) ([GitHub deposu](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-502">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="a2ba9-503">[NLog](https://nlog-project.org/) ([GitHub deposu](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-503">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="a2ba9-504">[Sentry](https://sentry.io/welcome/) ([GitHub deposu](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-504">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="a2ba9-505">[Serilog](https://serilog.net/) ([GitHub deposu](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-505">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="a2ba9-506">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub deposu](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="a2ba9-506">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="a2ba9-507">Bazı üçüncü taraf çerçeveler [, yapılandırılmış günlük olarak da bilinen anlam günlüğe kaydetme](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)işlemini gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-507">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="a2ba9-508">Bir üçüncü taraf çerçevesinin kullanılması, yerleşik sağlayıcılardan birini kullanmaya benzer:</span><span class="sxs-lookup"><span data-stu-id="a2ba9-508">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="a2ba9-509">Projenize bir NuGet paketi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-509">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="a2ba9-510">Günlüğe kaydetme çerçevesi tarafından sağlanmış bir `ILoggerFactory` genişletme yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-510">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="a2ba9-511">Daha fazla bilgi için bkz. her sağlayıcının belgeleri.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-511">For more information, see each provider's documentation.</span></span> <span data-ttu-id="a2ba9-512">Üçüncü taraf günlüğü sağlayıcıları Microsoft tarafından desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a2ba9-512">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2ba9-513">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a2ba9-513">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
