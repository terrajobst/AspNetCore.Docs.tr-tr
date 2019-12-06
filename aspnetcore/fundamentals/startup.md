---
title: ASP.NET Core 'de uygulama başlatma
author: rick-anderson
description: ASP.NET Core ' deki başlangıç sınıfının Hizmetleri ve uygulamanın istek ardışık düzenini nasıl yapılandırdığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: fundamentals/startup
ms.openlocfilehash: 2468c685850f74b8dafb3e0abea6d7b83c417af0
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880515"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="ac660-103">ASP.NET Core 'de uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="ac660-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="ac660-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex)ve [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="ac660-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="ac660-105">`Startup` sınıfı Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ac660-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="ac660-106">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="ac660-106">The Startup class</span></span>

<span data-ttu-id="ac660-107">ASP.NET Core uygulamalar, kuralına göre `Startup` adlı bir `Startup` sınıfını kullanır.</span><span class="sxs-lookup"><span data-stu-id="ac660-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="ac660-108">`Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ac660-108">The `Startup` class:</span></span>

* <span data-ttu-id="ac660-109">İsteğe bağlı olarak, uygulamanın *hizmetlerini*yapılandırmak için bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="ac660-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="ac660-110">Hizmet, uygulama işlevselliği sağlayan yeniden kullanılabilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="ac660-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="ac660-111">Hizmetler `ConfigureServices` *kaydedilir* ve [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>aracılığıyla uygulama genelinde tüketilebilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-111">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="ac660-112">Uygulamanın istek işleme ardışık düzenini oluşturmak için bir <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="ac660-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="ac660-113">`ConfigureServices` ve `Configure` uygulama başlatıldığında ASP.NET Core çalışma zamanı tarafından çağrılır:</span><span class="sxs-lookup"><span data-stu-id="ac660-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="ac660-114">Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.</span><span class="sxs-lookup"><span data-stu-id="ac660-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

<span data-ttu-id="ac660-115">`Startup` sınıfı, uygulamanın [ana bilgisayarı](xref:fundamentals/index#host) yapılandırıldığında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="ac660-116">`Startup` sınıfı genellikle konak Oluşturucu 'da [Webhostbuilderextensions. UseStartup\<tstartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) yöntemi çağırarak belirtilir:</span><span class="sxs-lookup"><span data-stu-id="ac660-116">The `Startup` class is typically specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="ac660-117">Ana bilgisayar `Startup` sınıfı Oluşturucusu tarafından kullanılabilen hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ac660-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="ac660-118">Uygulama `ConfigureServices`aracılığıyla ek hizmetler ekler.</span><span class="sxs-lookup"><span data-stu-id="ac660-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="ac660-119">Hem konak hem de uygulama hizmetleri `Configure` ve uygulama genelinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="ac660-120">[Genel ana bilgisayar](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) kullanılırken `Startup` oluşturucusuna yalnızca aşağıdaki hizmet türleri eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="ac660-120">Only the following service types can be injected into the `Startup` constructor when using the [Generic Host](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="ac660-121">`Configure` yöntemi çağrılana kadar çoğu hizmet kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="ac660-121">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac660-122">Ana bilgisayar `Startup` sınıfı Oluşturucusu tarafından kullanılabilen hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ac660-122">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="ac660-123">Uygulama `ConfigureServices`aracılığıyla ek hizmetler ekler.</span><span class="sxs-lookup"><span data-stu-id="ac660-123">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="ac660-124">Hem konak hem de uygulama hizmetleri `Configure` ve uygulama genelinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-124">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="ac660-125">`Startup` sınıfa [bağımlılık ekleme](xref:fundamentals/dependency-injection) 'nin yaygın bir kullanımı, şu ekleme işlemini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="ac660-125">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="ac660-126">Hizmetleri ortama göre yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="ac660-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="ac660-127">yapılandırmayı okumak için <xref:Microsoft.Extensions.Configuration.IConfiguration>.</span><span class="sxs-lookup"><span data-stu-id="ac660-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="ac660-128">`Startup.ConfigureServices`bir günlükçü oluşturmak için <xref:Microsoft.Extensions.Logging.ILoggerFactory>.</span><span class="sxs-lookup"><span data-stu-id="ac660-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="ac660-129">`Configure` yöntemi çağrılana kadar çoğu hizmet kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="ac660-129">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

### <a name="multiple-startup"></a><span data-ttu-id="ac660-130">Çoklu başlangıç</span><span class="sxs-lookup"><span data-stu-id="ac660-130">Multiple Startup</span></span>

<span data-ttu-id="ac660-131">Uygulama farklı ortamlarda ayrı `Startup` sınıfları tanımladığında (örneğin, `StartupDevelopment`), çalışma zamanında uygun `Startup` sınıfı seçilidir.</span><span class="sxs-lookup"><span data-stu-id="ac660-131">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="ac660-132">Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-132">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="ac660-133">Uygulama geliştirme ortamında çalıştırıldıysanız ve hem bir `Startup` sınıfını hem de bir `StartupDevelopment` sınıfını içeriyorsa `StartupDevelopment` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac660-133">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="ac660-134">Daha fazla bilgi için bkz. [birden çok ortam kullanma](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="ac660-134">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="ac660-135">Konak hakkında daha fazla bilgi için [konağa](xref:fundamentals/index#host) bakın.</span><span class="sxs-lookup"><span data-stu-id="ac660-135">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="ac660-136">Başlatma sırasında hataları işleme hakkında daha fazla bilgi için bkz. [Başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="ac660-136">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="ac660-137">ConfigureServices yöntemi</span><span class="sxs-lookup"><span data-stu-id="ac660-137">The ConfigureServices method</span></span>

<span data-ttu-id="ac660-138"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ac660-138">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="ac660-139">İsteğe bağlı.</span><span class="sxs-lookup"><span data-stu-id="ac660-139">Optional.</span></span>
* <span data-ttu-id="ac660-140">Uygulamanın hizmetlerini yapılandırmak için `Configure` yönteminden önce ana bilgisayar tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="ac660-140">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="ac660-141">[Yapılandırma seçeneklerinin](xref:fundamentals/configuration/index) kurala göre ayarlandığı yer.</span><span class="sxs-lookup"><span data-stu-id="ac660-141">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="ac660-142">Konak `Startup` Yöntemler çağrılmadan önce bazı hizmetleri yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-142">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="ac660-143">Daha fazla bilgi için bkz. [ana bilgisayar](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="ac660-143">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="ac660-144">Önemli kurulum gerektiren özellikler için <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>`Add{Service}` uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ac660-144">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="ac660-145">Örneğin, DbContext **ekleyin** **, defaultıdentity ekleyin,** entityframeworkmağazalarını **ekleyin ve**RazorPages **ekleyin**:</span><span class="sxs-lookup"><span data-stu-id="ac660-145">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

<span data-ttu-id="ac660-146">Hizmet kapsayıcısına hizmetleri eklemek, uygulama içinde ve `Configure` yönteminde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="ac660-146">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="ac660-147">Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>üzerinden çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="ac660-147">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac660-148">`SetCompatibilityVersion`hakkında daha fazla bilgi için bkz. [Setcompatibilityversion](xref:mvc/compatibility-version) .</span><span class="sxs-lookup"><span data-stu-id="ac660-148">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

::: moniker-end

## <a name="the-configure-method"></a><span data-ttu-id="ac660-149">Configure yöntemi</span><span class="sxs-lookup"><span data-stu-id="ac660-149">The Configure method</span></span>

<span data-ttu-id="ac660-150"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> yöntemi, uygulamanın HTTP isteklerine nasıl yanıt verdiğini belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac660-150">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="ac660-151">İstek ardışık düzeni, bir <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> örneğine [Ara yazılım](xref:fundamentals/middleware/index) bileşenleri eklenerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ac660-151">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="ac660-152">`IApplicationBuilder` `Configure` yöntemi tarafından kullanılabilir, ancak hizmet kapsayıcısında kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="ac660-152">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="ac660-153">Barındırma bir `IApplicationBuilder` oluşturur ve doğrudan `Configure`geçirir.</span><span class="sxs-lookup"><span data-stu-id="ac660-153">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="ac660-154">[ASP.NET Core şablonlar](/dotnet/core/tools/dotnet-new) işlem hattını desteğiyle birlikte yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="ac660-154">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="ac660-155">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="ac660-155">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="ac660-156">Özel durum işleyicisi</span><span class="sxs-lookup"><span data-stu-id="ac660-156">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="ac660-157">HTTP katı taşıma güvenliği (HSTS)</span><span class="sxs-lookup"><span data-stu-id="ac660-157">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="ac660-158">HTTPS yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="ac660-158">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="ac660-159">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="ac660-159">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="ac660-160">ASP.NET Core [MVC](xref:mvc/overview) ve [Razor Pages](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="ac660-160">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="ac660-161">Genel Veri Koruma Yönetmeliği (GDPR)</span><span class="sxs-lookup"><span data-stu-id="ac660-161">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="ac660-162">Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.</span><span class="sxs-lookup"><span data-stu-id="ac660-162">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

<span data-ttu-id="ac660-163">Her `Use` uzantısı yöntemi, istek ardışık düzenine bir veya daha fazla ara yazılım bileşeni ekler.</span><span class="sxs-lookup"><span data-stu-id="ac660-163">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="ac660-164">Örneğin, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>, [ara yazılımı](xref:fundamentals/middleware/index) [statik dosyaları](xref:fundamentals/static-files)sunacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ac660-164">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="ac660-165">İstek ardışık düzeninde bulunan her bir ara yazılım bileşeni, uygun olduğunda, zincirdeki bir sonraki bileşeni çağırmaktan veya zincirde kısa bir süre sonra sağlanmasından sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ac660-165">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac660-166">`IWebHostEnvironment`, `ILoggerFactory`veya `ConfigureServices`tanımlı herhangi bir şey gibi ek hizmetler `Configure` yöntemi imzasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-166">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="ac660-167">Bu hizmetler varsa eklenir.</span><span class="sxs-lookup"><span data-stu-id="ac660-167">These services are injected if they're available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac660-168">`IHostingEnvironment` ve `ILoggerFactory`gibi ek hizmetler veya `ConfigureServices`tanımlı herhangi bir şey `Configure` yöntemi imzasında belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-168">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="ac660-169">Bu hizmetler varsa eklenir.</span><span class="sxs-lookup"><span data-stu-id="ac660-169">These services are injected if they're available.</span></span>

::: moniker-end

<span data-ttu-id="ac660-170">`IApplicationBuilder` kullanımı ve ara yazılım işleme sırası hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="ac660-170">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="ac660-171">Hizmetleri başlatmadan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ac660-171">Configure services without Startup</span></span>

<span data-ttu-id="ac660-172">`Startup` sınıf kullanmadan Hizmetleri ve istek işleme işlem hattını yapılandırmak için, `ConfigureServices` çağırın ve konak Oluşturucu üzerinde `Configure` kullanışlı yöntemler kullanın.</span><span class="sxs-lookup"><span data-stu-id="ac660-172">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="ac660-173">`ConfigureServices` birden çok çağrısı birbirine eklenir.</span><span class="sxs-lookup"><span data-stu-id="ac660-173">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="ac660-174">Birden çok `Configure` yöntemi varsa, son `Configure` çağrısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac660-174">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="ac660-175">Başlangıç filtreleriyle başlatmayı Genişlet</span><span class="sxs-lookup"><span data-stu-id="ac660-175">Extend Startup with startup filters</span></span>

<span data-ttu-id="ac660-176"><xref:Microsoft.AspNetCore.Hosting.IStartupFilter>kullanın:</span><span class="sxs-lookup"><span data-stu-id="ac660-176">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="ac660-177">Ara yazılımı, bir uygulamanın başlangıcında veya sonunda `Use{Middleware}`açık bir çağrı olmadan [Ara yazılım ardışık düzeni yapılandırma.](#the-configure-method)</span><span class="sxs-lookup"><span data-stu-id="ac660-177">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="ac660-178">`IStartupFilter`, ASP.NET Core tarafından, uygulama yazarının varsayılan ara yazılımı açıkça kaydetmesini sağlamak zorunda kalmadan, işlem hattının başlangıcına varsayılanlar eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac660-178">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="ac660-179">`IStartupFilter`, uygulama yazarı adına farklı bir bileşen çağrısının `Use{Middleware}` izin verir.</span><span class="sxs-lookup"><span data-stu-id="ac660-179">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="ac660-180">`Configure` yöntemlerinin bir işlem hattı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="ac660-180">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="ac660-181">[Itartupfilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) , bir ara yazılımı kitaplıklar tarafından eklenen bir veya daha sonra çalışacak şekilde ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-181">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="ac660-182">`IStartupFilter`, `Action<IApplicationBuilder>`alan ve döndüren <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>uygular.</span><span class="sxs-lookup"><span data-stu-id="ac660-182">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="ac660-183"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>, bir uygulamanın istek işlem hattını yapılandırmak için bir sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ac660-183">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="ac660-184">Daha fazla bilgi için bkz. [IApplicationBuilder ile bir ara yazılım işlem hattı oluşturma](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="ac660-184">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="ac660-185">Her `IStartupFilter`, istek ardışık düzeninde bir veya daha fazla middlewares ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-185">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="ac660-186">Filtreler, hizmet kapsayıcısına eklendikleri sırada çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ac660-186">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="ac660-187">Filtreler bir sonraki filtreye denetimi geçirmeden önce veya sonra bir ara yazılım ekleyebilir, böylece uygulama işlem hattının başına veya sonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="ac660-187">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="ac660-188">Aşağıdaki örnek, `IStartupFilter`bir ara yazılımı nasıl kaydedeceğinizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="ac660-188">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="ac660-189">`RequestSetOptionsMiddleware` ara yazılım bir sorgu dizesi parametresinden bir seçenek değeri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="ac660-189">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="ac660-190">`RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` sınıfında yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ac660-190">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="ac660-191">`RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` sınıfında yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ac660-191">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

<span data-ttu-id="ac660-192">`IStartupFilter`, <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-192">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

<span data-ttu-id="ac660-193">`option` için bir sorgu dizesi parametresi sağlandığında, ASP.NET Core ara yazılımı yanıtı oluşturmadan önce, ara yazılım değer atamasını işler.</span><span class="sxs-lookup"><span data-stu-id="ac660-193">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="ac660-194">Ara yazılım yürütme sırası `IStartupFilter` kayıt sırasıyla ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="ac660-194">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="ac660-195">Birden çok `IStartupFilter` uygulaması aynı nesnelerle etkileşime geçebilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-195">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="ac660-196">Sıralama önemliyse, kendi `IStartupFilter` hizmet kayıtlarını, middlewares çalıştırmaları sırasıyla eşleşecek şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="ac660-196">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="ac660-197">Kitaplıklar, `IStartupFilter`kayıtlı olan diğer uygulama ara yazılımı ile önce veya sonra çalışan bir veya daha fazla `IStartupFilter` uygulaması olan ara yazılım ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="ac660-197">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="ac660-198">Bir `IStartupFilter` ara yazılım bir kitaplık `IStartupFilter`tarafından eklenmeden önce çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="ac660-198">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="ac660-199">Kitaplık hizmet kapsayıcısına eklenmeden önce hizmet kaydını konumlandırın.</span><span class="sxs-lookup"><span data-stu-id="ac660-199">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="ac660-200">Daha sonra çağırmak için, kitaplık eklendikten sonra hizmet kaydını konumlandırın.</span><span class="sxs-lookup"><span data-stu-id="ac660-200">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="ac660-201">Başlangıçta bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="ac660-201">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="ac660-202"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ac660-202">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="ac660-203">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="ac660-203">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac660-204">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ac660-204">Additional resources</span></span>

* [<span data-ttu-id="ac660-205">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="ac660-205">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
