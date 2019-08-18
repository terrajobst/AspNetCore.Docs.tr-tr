---
title: ASP.NET Core 'de uygulama başlatma
author: tdykstra
description: ASP.NET Core ' deki başlangıç sınıfının Hizmetleri ve uygulamanın istek ardışık düzenini nasıl yapılandırdığını öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 8/7/2019
uid: fundamentals/startup
ms.openlocfilehash: 2ce4f2093c11066370160c62949888837065f129
ms.sourcegitcommit: 2fa0ffe82a47c7317efc9ea908365881cbcb8ed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69572842"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="276ca-103">ASP.NET Core 'de uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="276ca-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="276ca-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex)ve [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="276ca-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="276ca-105">`Startup` Sınıfı Hizmetleri ve uygulamanın istek ardışık düzenini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="276ca-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="276ca-106">Başlangıç sınıfı</span><span class="sxs-lookup"><span data-stu-id="276ca-106">The Startup class</span></span>

<span data-ttu-id="276ca-107">ASP.NET Core uygulamalar, kural `Startup` tarafından adlandırılan `Startup` bir sınıfı kullanır.</span><span class="sxs-lookup"><span data-stu-id="276ca-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="276ca-108">`Startup` Sınıf:</span><span class="sxs-lookup"><span data-stu-id="276ca-108">The `Startup` class:</span></span>

* <span data-ttu-id="276ca-109">İsteğe bağlı olarak <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> , uygulamanın *hizmetlerini*yapılandırmak için bir yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="276ca-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="276ca-110">Hizmet, uygulama işlevselliği sağlayan yeniden kullanılabilir bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="276ca-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="276ca-111">`ConfigureServices`&mdash; <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> [](xref:fundamentals/dependency-injection) Hizmetler Ayrıca, bağımlılık ekleme (dı) veya aracılığıyla uygulama genelinde kullanılan ve tüketilen şekilde de açıklanır.&mdash;</span><span class="sxs-lookup"><span data-stu-id="276ca-111">Services are configured&mdash;also described as *registered*&mdash;in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="276ca-112">Uygulamanın istek <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> işleme ardışık düzenini oluşturmak için bir yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="276ca-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="276ca-113">`ConfigureServices`ve `Configure` uygulama başlatıldığında ASP.NET Core çalışma zamanı tarafından çağrılır:</span><span class="sxs-lookup"><span data-stu-id="276ca-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="276ca-114">Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.</span><span class="sxs-lookup"><span data-stu-id="276ca-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

<span data-ttu-id="276ca-115">Sınıf, uygulamanın [ana makinesi](xref:fundamentals/index#host) yapılandırıldığında belirtilir. `Startup`</span><span class="sxs-lookup"><span data-stu-id="276ca-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="276ca-116">Sınıfı, genellikle [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) yöntemi ana bilgisayar Oluşturucu üzerinde çağırarak belirtilir: `Startup`</span><span class="sxs-lookup"><span data-stu-id="276ca-116">The `Startup` class is typically specified by calling the [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="276ca-117">Ana bilgisayar, `Startup` sınıf oluşturucusunun kullanabildiği hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="276ca-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="276ca-118">Uygulama aracılığıyla `ConfigureServices`ek hizmetler ekler.</span><span class="sxs-lookup"><span data-stu-id="276ca-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="276ca-119">Hem konak hem de uygulama Hizmetleri uygulama içinde ve `Configure` üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="276ca-120">Şu kullanıldığında `Startup` oluşturucuya <xref:Microsoft.Extensions.Hosting.IHostBuilder>yalnızca aşağıdaki hizmet türleri eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="276ca-120">Only the following service types can be injected into the `Startup` constructor when using <xref:Microsoft.Extensions.Hosting.IHostBuilder>:</span></span>

* `IWebHostEnvironment`
* `IHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="276ca-121">Çoğu hizmet, `Configure` Yöntem çağrılana kadar kullanılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="276ca-121">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="276ca-122">Ana bilgisayar, `Startup` sınıf oluşturucusunun kullanabildiği hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="276ca-122">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="276ca-123">Uygulama aracılığıyla `ConfigureServices`ek hizmetler ekler.</span><span class="sxs-lookup"><span data-stu-id="276ca-123">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="276ca-124">Hem konak hem de uygulama Hizmetleri uygulama içinde ve üzerinde `Configure` kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-124">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="276ca-125">`Startup` Sınıfa [bağımlılık ekleme](xref:fundamentals/dependency-injection) 'nin yaygın bir kullanımı, şu ekleme yapmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="276ca-125">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="276ca-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>Hizmetleri ortama göre yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="276ca-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="276ca-127"><xref:Microsoft.Extensions.Configuration.IConfiguration>yapılandırmasını okuyun.</span><span class="sxs-lookup"><span data-stu-id="276ca-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="276ca-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory>içinde `Startup.ConfigureServices`bir günlükçü oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="276ca-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

::: moniker-end
<span data-ttu-id="276ca-129">Ekleme `IWebHostEnvironment` için bir alternatif, kural tabanlı bir yaklaşım kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="276ca-129">An alternative to injecting `IWebHostEnvironment` is to use a conventions-based approach.</span></span>
::: moniker range=">= aspnetcore-3.0"

::: moniker-end

::: moniker range="< aspnetcore-3.0"
<span data-ttu-id="276ca-130">Ekleme `IHostingEnvironment` için bir alternatif, kural tabanlı bir yaklaşım kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="276ca-130">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span>
::: moniker-end

<span data-ttu-id="276ca-131">Uygulama farklı ortamlar için ayrı `Startup` sınıflar tanımladığında (örneğin, `StartupDevelopment`), çalışma zamanında uygun `Startup` sınıf seçilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-131">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="276ca-132">Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-132">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="276ca-133">Uygulama geliştirme ortamında çalıştırıldıysanız ve hem `Startup` sınıf `StartupDevelopment` hem de sınıf içeriyorsa, `StartupDevelopment` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="276ca-133">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="276ca-134">Daha fazla bilgi için bkz. [birden çok ortam kullanma](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="276ca-134">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="276ca-135">Konak hakkında daha fazla bilgi için [konağa](xref:fundamentals/index#host) bakın.</span><span class="sxs-lookup"><span data-stu-id="276ca-135">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="276ca-136">Başlatma sırasında hataları işleme hakkında daha fazla bilgi için bkz. [Başlangıç özel durum işleme](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="276ca-136">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="276ca-137">ConfigureServices yöntemi</span><span class="sxs-lookup"><span data-stu-id="276ca-137">The ConfigureServices method</span></span>

<span data-ttu-id="276ca-138"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="276ca-138">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="276ca-139">İsteğe bağlı.</span><span class="sxs-lookup"><span data-stu-id="276ca-139">Optional.</span></span>
* <span data-ttu-id="276ca-140">Uygulamanın hizmetlerini yapılandırma `Configure` yönteminden önce ana bilgisayar tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="276ca-140">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="276ca-141">[Yapılandırma seçeneklerinin](xref:fundamentals/configuration/index) kurala göre ayarlandığı yer.</span><span class="sxs-lookup"><span data-stu-id="276ca-141">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="276ca-142">Konak, Yöntemler çağrılmadan önce `Startup` bazı hizmetleri yapılandırabilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-142">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="276ca-143">Daha fazla bilgi için bkz. [ana bilgisayar](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="276ca-143">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="276ca-144">Önemli kurulum `Add{Service}` gerektiren özellikler için üzerinde <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="276ca-144">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="276ca-145">Örneğin, DbContext **ekleyin**, defaultıdentity ekleyin, entityframeworkmağazalarını ekleyin ve RazorPages **ekleyin**:</span><span class="sxs-lookup"><span data-stu-id="276ca-145">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

<span data-ttu-id="276ca-146">Hizmet kapsayıcısına hizmet eklemek, bunları uygulama içinde ve `Configure` yönteminde kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="276ca-146">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="276ca-147">Hizmetler, [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya konumundan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="276ca-147">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="276ca-148">Hakkında`SetCompatibilityVersion`daha fazla bilgi Için bkz. [setcompatibilityversion](xref:mvc/compatibility-version) .</span><span class="sxs-lookup"><span data-stu-id="276ca-148">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

::: moniker-end

## <a name="the-configure-method"></a><span data-ttu-id="276ca-149">Configure yöntemi</span><span class="sxs-lookup"><span data-stu-id="276ca-149">The Configure method</span></span>

<span data-ttu-id="276ca-150"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Yöntemi, uygulamanın http isteklerine nasıl yanıt verdiğini belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="276ca-150">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="276ca-151">İstek ardışık düzeni, bir <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> örneğe [Ara yazılım](xref:fundamentals/middleware/index) bileşenleri eklenerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="276ca-151">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="276ca-152">`IApplicationBuilder`, `Configure` yöntemi için kullanılabilir, ancak hizmet kapsayıcısına kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="276ca-152">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="276ca-153">Barındırma bir `IApplicationBuilder` oluşturur ve doğrudan öğesine `Configure`geçirir.</span><span class="sxs-lookup"><span data-stu-id="276ca-153">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="276ca-154">[ASP.NET Core şablonlar](/dotnet/core/tools/dotnet-new) işlem hattını desteğiyle birlikte yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="276ca-154">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="276ca-155">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="276ca-155">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="276ca-156">Özel durum işleyicisi</span><span class="sxs-lookup"><span data-stu-id="276ca-156">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="276ca-157">HTTP katı taşıma güvenliği (HSTS)</span><span class="sxs-lookup"><span data-stu-id="276ca-157">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="276ca-158">HTTPS yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="276ca-158">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="276ca-159">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="276ca-159">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="276ca-160">ASP.NET Core [MVC](xref:mvc/overview) ve [Razor Pages](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="276ca-160">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="276ca-161">Genel Veri Koruma Yönetmeliği (GDPR)</span><span class="sxs-lookup"><span data-stu-id="276ca-161">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="276ca-162">Yukarıdaki örnek [Razor Pages](xref:razor-pages/index)içindir; MVC sürümü benzerdir.</span><span class="sxs-lookup"><span data-stu-id="276ca-162">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

<span data-ttu-id="276ca-163">Her `Use` genişletme yöntemi, istek ardışık düzenine bir veya daha fazla ara yazılım bileşeni ekler.</span><span class="sxs-lookup"><span data-stu-id="276ca-163">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="276ca-164">Örneğin, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> [ara yazılımı](xref:fundamentals/middleware/index) [statik dosyaları](xref:fundamentals/static-files)sunacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="276ca-164">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="276ca-165">İstek ardışık düzeninde bulunan her bir ara yazılım bileşeni, uygun olduğunda, zincirdeki bir sonraki bileşeni çağırmaktan veya zincirde kısa bir süre sonra sağlanmasından sorumludur.</span><span class="sxs-lookup"><span data-stu-id="276ca-165">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="276ca-166">`IWebHostEnvironment`, Veya `ILoggerFactory` `Configure` gibi ek hizmetler, yöntem imzasında belirtilebilir. `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="276ca-166">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="276ca-167">Bu hizmetler varsa eklenir.</span><span class="sxs-lookup"><span data-stu-id="276ca-167">These services are injected if they're available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="276ca-168">`IHostingEnvironment` Ve `ConfigureServices` `Configure` gibi ek hizmetler veya ' de tanımlı herhangi bir şey, yöntem imzasında belirtilebilir. `ILoggerFactory`</span><span class="sxs-lookup"><span data-stu-id="276ca-168">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="276ca-169">Bu hizmetler varsa eklenir.</span><span class="sxs-lookup"><span data-stu-id="276ca-169">These services are injected if they're available.</span></span>

::: moniker-end

<span data-ttu-id="276ca-170">Kullanımı `IApplicationBuilder` ve ara yazılım işleme sırası hakkında daha fazla bilgi için bkz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="276ca-170">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="276ca-171">Hizmetleri başlatmadan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="276ca-171">Configure services without Startup</span></span>

<span data-ttu-id="276ca-172">Hizmetleri ve istek işleme işlem hattını, ana bilgisayar Oluşturucu üzerinde `Startup` bir sınıf, `ConfigureServices` çağrı `Configure` ve kullanışlı yöntemler kullanmadan yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="276ca-172">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="276ca-173">Birbirine eklenecek birden `ConfigureServices` çok çağrı.</span><span class="sxs-lookup"><span data-stu-id="276ca-173">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="276ca-174">Birden çok `Configure` yöntem varsa, son `Configure` çağrı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="276ca-174">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="276ca-175">Başlangıç filtreleriyle başlatmayı Genişlet</span><span class="sxs-lookup"><span data-stu-id="276ca-175">Extend Startup with startup filters</span></span>

<span data-ttu-id="276ca-176">Bir <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> uygulamanın, ara yazılım [yapılandırma](#the-configure-method) işlem hattının başlangıcında veya sonunda ara yazılımı yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="276ca-176">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="276ca-177">`IStartupFilter``Configure` Yöntem işlem hattı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="276ca-177">`IStartupFilter` is used to create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="276ca-178">[Itartupfilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) , bir ara yazılımı kitaplıklar tarafından eklenen bir veya daha sonra çalışacak şekilde ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-178">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="276ca-179">`IStartupFilter`öğesini alır ve `Action<IApplicationBuilder>`döndürür. <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*></span><span class="sxs-lookup"><span data-stu-id="276ca-179">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="276ca-180">Bir uygulamanın istek ardışık düzenini yapılandırmak için bir sınıf tanımlar.<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder></span><span class="sxs-lookup"><span data-stu-id="276ca-180">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="276ca-181">Daha fazla bilgi için bkz. [IApplicationBuilder ile bir ara yazılım işlem hattı oluşturma](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="276ca-181">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="276ca-182">Her `IStartupFilter` biri, istek ardışık düzeninde bir veya daha fazla middlewares ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-182">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="276ca-183">Filtreler, hizmet kapsayıcısına eklendikleri sırada çağrılır.</span><span class="sxs-lookup"><span data-stu-id="276ca-183">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="276ca-184">Filtreler bir sonraki filtreye denetimi geçirmeden önce veya sonra bir ara yazılım ekleyebilir, böylece uygulama işlem hattının başına veya sonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="276ca-184">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="276ca-185">Aşağıdaki örnek, ile `IStartupFilter`bir ara yazılımın nasıl kaydettirildiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="276ca-185">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="276ca-186">`RequestSetOptionsMiddleware` Ara yazılım bir sorgu dizesi parametresinden bir seçenek değeri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="276ca-186">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="276ca-187">, `RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` Sınıfında yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="276ca-187">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="276ca-188">, `RequestSetOptionsMiddleware` `RequestSetOptionsStartupFilter` Sınıfında yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="276ca-188">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

<span data-ttu-id="276ca-189">, `IStartupFilter` İçindeki<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-189">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

<span data-ttu-id="276ca-190">İçin `option` bir sorgu dizesi parametresi sağlandığında, ara yazılım ASP.NET Core ara yazılım tarafından yanıt oluşturmadan önce değer atamasını işler.</span><span class="sxs-lookup"><span data-stu-id="276ca-190">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="276ca-191">Ara yazılım yürütme sırası, `IStartupFilter` kayıt sırasıyla ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="276ca-191">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="276ca-192">Birden `IStartupFilter` çok uygulama aynı nesnelerle etkileşime geçebilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-192">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="276ca-193">Sıralama önemliyse, `IStartupFilter` hizmet kayıtlarını middlewares 'in çalıştırması gereken sırayla eşleşecek şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="276ca-193">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="276ca-194">Kitaplıklar, ile `IStartupFilter`kaydolmadan önce veya sonra `IStartupFilter` çalışan bir veya daha fazla uygulamayla bir ara yazılım ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="276ca-194">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="276ca-195">Bir ara yazılımı `IStartupFilter` , bir `IStartupFilter`kitaplık tarafından eklenen bir ara yazılımı çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="276ca-195">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="276ca-196">Kitaplık hizmet kapsayıcısına eklenmeden önce hizmet kaydını konumlandırın.</span><span class="sxs-lookup"><span data-stu-id="276ca-196">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="276ca-197">Daha sonra çağırmak için, kitaplık eklendikten sonra hizmet kaydını konumlandırın.</span><span class="sxs-lookup"><span data-stu-id="276ca-197">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="276ca-198">Başlangıçta bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="276ca-198">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="276ca-199"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Uygulama ,`Startup` uygulamanın sınıfı dışında bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler eklenmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="276ca-199">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="276ca-200">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="276ca-200">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="276ca-201">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="276ca-201">Additional resources</span></span>

* [<span data-ttu-id="276ca-202">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="276ca-202">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
