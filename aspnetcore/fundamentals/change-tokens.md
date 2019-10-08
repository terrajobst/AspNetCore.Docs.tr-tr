---
title: ASP.NET Core değişiklik belirteçleriyle değişiklikleri Algıla
author: guardrex
description: Değişiklikleri izlemek için değişiklik belirteçlerini nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: bb30d7a4c7dc82200821c60a49c314b246562111
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007213"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="37146-103">ASP.NET Core değişiklik belirteçleriyle değişiklikleri Algıla</span><span class="sxs-lookup"><span data-stu-id="37146-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="37146-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37146-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="37146-105">*Değişiklik belirteci* , durum değişikliklerini izlemek için kullanılan genel amaçlı, düşük düzey bir yapı taşıdır.</span><span class="sxs-lookup"><span data-stu-id="37146-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="37146-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37146-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="37146-107">Ichangetoken arabirimi</span><span class="sxs-lookup"><span data-stu-id="37146-107">IChangeToken interface</span></span>

<span data-ttu-id="37146-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> bir değişikliğin gerçekleştiği bildirimleri yayar.</span><span class="sxs-lookup"><span data-stu-id="37146-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="37146-109">`IChangeToken` <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="37146-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37146-110">[Microsoft. Extensions. Ilkel öğeler](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi, ASP.NET Core uygulamalarına örtük olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="37146-110">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="37146-111">`IChangeToken` iki özelliğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="37146-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="37146-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks>, belirtecin etkin olmayan geri çağırmaları harekete geçirmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="37146-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="37146-113">@No__t-0 `false` olarak ayarlanırsa, bir geri çağırma hiçbir şekilde çağrılmaz ve uygulamanın değişiklikler için `HasChanged` ' yi yoklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="37146-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="37146-114">Hiçbir değişiklik gerçekleşmüyorsa veya temeldeki değişiklik dinleyicisi atıldığı veya devre dışı bırakıldığında belirtecin hiçbir şekilde iptal edilmemesi de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="37146-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="37146-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğini gösteren bir değer alır.</span><span class="sxs-lookup"><span data-stu-id="37146-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="37146-116">@No__t-0 arabirimi, belirteç değiştirildiğinde çağrılan bir geri aramayı kaydeden [Registerchangecallback (Action @ no__t-2object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemini içerir.</span><span class="sxs-lookup"><span data-stu-id="37146-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="37146-117">`HasChanged`, geri çağırma çağrılmadan önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="37146-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="37146-118">ChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="37146-118">ChangeToken class</span></span>

<span data-ttu-id="37146-119"><xref:Microsoft.Extensions.Primitives.ChangeToken>, bir değişikliğin gerçekleştiği bildirimleri yaymak için kullanılan statik bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="37146-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="37146-120">`ChangeToken` <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="37146-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37146-121">[Microsoft. Extensions. Ilkel öğeler](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi, ASP.NET Core uygulamalarına örtük olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="37146-121">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="37146-122">[ChangeToken. OnChange (Func @ no__t-1IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi, belirteç her değiştiğinde çağırmak için bir `Action` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="37146-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="37146-123">`Func<IChangeToken>` belirteci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37146-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="37146-124">belirteç değiştiğinde `Action` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="37146-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="37146-125">[ChangeToken. OnChange @ no__t-1TState > (Func @ no__t-2IChangeToken >, Action @ no__t-3TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı yüklemesi, token tüketicisi `Action` ' e geçirilen ek bir `TState` parametresi alır.</span><span class="sxs-lookup"><span data-stu-id="37146-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="37146-126">`OnChange` <xref:System.IDisposable> döndürür.</span><span class="sxs-lookup"><span data-stu-id="37146-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="37146-127">@No__t çağrılması-0, daha fazla değişiklik için dinlemeyi dinlemeden ve belirtecin kaynaklarını yayınlarından bırakır.</span><span class="sxs-lookup"><span data-stu-id="37146-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="37146-128">ASP.NET Core değişiklik belirteçlerinin örnek kullanımları</span><span class="sxs-lookup"><span data-stu-id="37146-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="37146-129">Değişiklik belirteçleri, nesnelerde yapılan değişiklikleri izlemek için ASP.NET Core belirgin alanlarında kullanılır:</span><span class="sxs-lookup"><span data-stu-id="37146-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="37146-130">Dosyalarda yapılan değişiklikleri izlemek için, <xref:Microsoft.Extensions.FileProviders.IFileProvider> ' ı <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi, belirtilen dosya veya klasör için bir `IChangeToken` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37146-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="37146-131">`IChangeToken` belirteçleri, değişiklik üzerine önbellek çıkarmaları tetiklemek için önbellek girişlerine eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="37146-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="37146-132">@No__t 0 değişiklikleri için, <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ' nin varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulamasının bir veya daha fazla <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örneğini kabul eden bir aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="37146-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="37146-133">Her örnek, izleme seçenekleri değişiklikleri için değişiklik bildirimi geri aramasını kaydetmek üzere bir @no__t döndürür.</span><span class="sxs-lookup"><span data-stu-id="37146-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="37146-134">Yapılandırma değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="37146-134">Monitor for configuration changes</span></span>

<span data-ttu-id="37146-135">Varsayılan olarak, ASP.NET Core şablonlar [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, appSettings) kullanır *. Development. JSON*ve *appSettings. Üretim. JSON*), uygulama yapılandırma ayarlarını yükler.</span><span class="sxs-lookup"><span data-stu-id="37146-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="37146-136">Bu dosyalar, `reloadOnChange` parametresini kabul eden <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ' de [Addjsonfile (ıseationbuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="37146-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="37146-137">`reloadOnChange`, yapılandırmanın dosya değişikliklerinde yeniden yüklenmesi gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="37146-138">Bu ayar <xref:Microsoft.Extensions.Hosting.Host> kullanışlı yöntemi <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> ' de görünür:</span><span class="sxs-lookup"><span data-stu-id="37146-138">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="37146-139">Dosya tabanlı yapılandırma <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="37146-140">`FileConfigurationSource`, dosyaları izlemek için <xref:Microsoft.Extensions.FileProviders.IFileProvider> kullanır.</span><span class="sxs-lookup"><span data-stu-id="37146-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="37146-141">Varsayılan olarak `IFileMonitor`, yapılandırma dosyası değişikliklerini izlemek için <xref:System.IO.FileSystemWatcher> kullanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="37146-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="37146-142">Örnek uygulama, yapılandırma değişikliklerini izlemek için iki uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="37146-143">*AppSettings* dosyalarından herhangi biri değiştiğinde, dosya izleme uygulamalarının her ikisi de özel kod yürütür @ no__t-1örnek uygulama konsola bir ileti yazar.</span><span class="sxs-lookup"><span data-stu-id="37146-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="37146-144">Yapılandırma dosyası `FileSystemWatcher`, tek bir yapılandırma dosyası değişikliği için birden çok belirteç geri çağırmaları tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="37146-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="37146-145">Özel kodun, birden fazla belirteç geri çağırma işlemi tetiklendiğinde yalnızca bir kez çalıştığından emin olmak için, örnek uygulama dosya karmalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="37146-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="37146-146">Örnek, SHA1 dosya karma kullanır.</span><span class="sxs-lookup"><span data-stu-id="37146-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="37146-147">Bir yeniden deneme, üstel geri dönme ile uygulanır.</span><span class="sxs-lookup"><span data-stu-id="37146-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="37146-148">Dosya kilitlemesi, geçici olarak bir dosyada yeni bir karma işlem yapılmasını önleyen dosya kilitleme gerçekleşebileceğinden, yeniden deneme vardır.</span><span class="sxs-lookup"><span data-stu-id="37146-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="37146-149">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="37146-150">Basit başlangıç değiştirme belirteci</span><span class="sxs-lookup"><span data-stu-id="37146-150">Simple startup change token</span></span>

<span data-ttu-id="37146-151">Yapılandırma yeniden yükleme belirtecine değişiklik bildirimleri için bir belirteç tüketicisi `Action` geri araması kaydedin.</span><span class="sxs-lookup"><span data-stu-id="37146-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="37146-152">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="37146-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="37146-153">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="37146-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="37146-154">Geri çağırma `InvokeChanged` yöntemidir:</span><span class="sxs-lookup"><span data-stu-id="37146-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="37146-155">Geri aramanın `state` ' ı, izlenecek doğru *appSettings* yapılandırma dosyasını (örneğin, appSettings) belirtmek için yararlı olan `IWebHostEnvironment` ' e geçirmek için kullanılır *. Geliştirme ortamında geliştirme. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="37146-155">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="37146-156">Dosya karmaları, yapılandırma dosyası yalnızca bir kez değiştirildiğinde, birden çok belirteç geri çağırmaları nedeniyle `WriteConsole` ifadesinin birden çok kez çalışmasını engellemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37146-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="37146-157">Uygulama çalıştığı sürece bu sistem çalışır ve Kullanıcı tarafından devre dışı bırakılamaz.</span><span class="sxs-lookup"><span data-stu-id="37146-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="37146-158">Yapılandırma değişikliklerini hizmet olarak izle</span><span class="sxs-lookup"><span data-stu-id="37146-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="37146-159">Örnek şunları uygular:</span><span class="sxs-lookup"><span data-stu-id="37146-159">The sample implements:</span></span>

* <span data-ttu-id="37146-160">Temel başlangıç belirteci izleme.</span><span class="sxs-lookup"><span data-stu-id="37146-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="37146-161">Hizmet olarak izleme.</span><span class="sxs-lookup"><span data-stu-id="37146-161">Monitoring as a service.</span></span>
* <span data-ttu-id="37146-162">İzlemeyi etkinleştirme ve devre dışı bırakma mekanizması.</span><span class="sxs-lookup"><span data-stu-id="37146-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="37146-163">Örnek, `IConfigurationMonitor` arabirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37146-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="37146-164">*Uzantılar/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="37146-165">Uygulanan sınıfın Oluşturucusu `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:</span><span class="sxs-lookup"><span data-stu-id="37146-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="37146-166">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="37146-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="37146-167">`InvokeChanged`, geri çağırma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="37146-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="37146-168">Bu örnekteki `state`, izleme durumuna erişmek için kullanılan `IConfigurationMonitor` örneğine bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="37146-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="37146-169">İki özellik kullanılır:</span><span class="sxs-lookup"><span data-stu-id="37146-169">Two properties are used:</span></span>

* <span data-ttu-id="37146-170">`MonitoringEnabled` &ndash;, geri aramanın özel kodunu çalıştırması gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="37146-171">`CurrentState` &ndash; Kullanıcı arabiriminde kullanım için geçerli izleme durumunu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="37146-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="37146-172">@No__t-0 yöntemi önceki yaklaşımla benzerdir, bunun dışında:</span><span class="sxs-lookup"><span data-stu-id="37146-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="37146-173">@No__t-0 `true` olmadığı müddetçe kodunu çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="37146-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="37146-174">Geçerli `state` ' ı `WriteConsole` çıkışındaki çıkarır.</span><span class="sxs-lookup"><span data-stu-id="37146-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="37146-175">Bir örnek `ConfigurationMonitor` `Startup.ConfigureServices` ' de hizmet olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="37146-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="37146-176">Dizin sayfası, yapılandırma izleme üzerinde Kullanıcı denetimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="37146-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="37146-177">@No__t-0 ' ın örneği, `IndexModel` ' e eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="37146-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="37146-178">*Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="37146-179">Yapılandırma İzleyicisi (`_monitor`), izlemeyi etkinleştirmek veya devre dışı bırakmak ve UI geri bildirimi için geçerli durumu ayarlamak üzere kullanılır:</span><span class="sxs-lookup"><span data-stu-id="37146-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="37146-180">@No__t-0 tetiklendiğinde, izleme etkinleştirilir ve geçerli durum temizlenir.</span><span class="sxs-lookup"><span data-stu-id="37146-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="37146-181">@No__t-0 tetiklendiğinde, izleme devre dışıdır ve durum, izlemenin gerçekleşmediğinden emin olmak üzere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="37146-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="37146-182">Kullanıcı arabirimindeki düğmeler izlemeyi etkinleştirir ve devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="37146-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="37146-183">*Sayfa/dizin. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37146-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="37146-184">Önbelleğe alınmış dosya değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="37146-184">Monitor cached file changes</span></span>

<span data-ttu-id="37146-185">Dosya içeriği, <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> kullanılarak bellek içinde önbelleğe alınabilir.</span><span class="sxs-lookup"><span data-stu-id="37146-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="37146-186">Bellek içi önbelleğe alma, [bellek Içi önbellek](xref:performance/caching/memory) konusunda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="37146-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="37146-187">Aşağıda açıklanan uygulama gibi ek adımlar uygulamadan, kaynak veriler değişirse önbellekten *eski* (eski) veriler döndürülür.</span><span class="sxs-lookup"><span data-stu-id="37146-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="37146-188">Örneğin, bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) dönemini yenilerken önbelleğe alınmış bir kaynak dosyanın durumu, eski önbelleğe alınmış dosya verilerine yol açar.</span><span class="sxs-lookup"><span data-stu-id="37146-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="37146-189">Verilerin her isteği, Kayan süre sonu süresini yeniler, ancak dosya hiçbir zaman önbelleğe yeniden yüklenmez.</span><span class="sxs-lookup"><span data-stu-id="37146-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="37146-190">Dosyanın önbelleğe alınmış içeriğini kullanan tüm uygulama özellikleri, büyük olasılıkla eski içerik alınmasına tabidir.</span><span class="sxs-lookup"><span data-stu-id="37146-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="37146-191">Bir dosya önbelleğe alma senaryosunda değişiklik belirteçlerini kullanmak önbellekte eski dosya içeriğinin varlığını engeller.</span><span class="sxs-lookup"><span data-stu-id="37146-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="37146-192">Örnek uygulama, yaklaşımın bir uygulamasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="37146-193">Örnek, `GetFileContent` ' i kullanır:</span><span class="sxs-lookup"><span data-stu-id="37146-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="37146-194">Dosya içeriğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="37146-194">Return file content.</span></span>
* <span data-ttu-id="37146-195">Bir dosya kilidinin geçici olarak bir dosya okumayı engellediği durumları kapsamak için üstel geri ile yeniden deneme algoritması uygulayın.</span><span class="sxs-lookup"><span data-stu-id="37146-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="37146-196">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="37146-197">Önbelleğe alınmış dosya aramalarını işlemek için bir `FileService` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="37146-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="37146-198">Hizmetin `GetFileContent` Yöntem çağrısı, bellek içi önbellekten dosya içeriğini almaya çalışır ve bunu çağırana (*Services/FileService. cs*) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="37146-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="37146-199">Önbellek anahtarı kullanılarak önbelleğe alınmış içerik bulunamazsa, aşağıdaki eylemler gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="37146-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="37146-200">Dosya içeriği `GetFileContent` kullanılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="37146-201">Dosya sağlayıcısından [ıfileproviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*)ile bir değişiklik belirteci elde edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="37146-202">Dosya değiştirildiğinde belirtecin geri çağırması tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="37146-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="37146-203">Dosya içeriği bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresiyle önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="37146-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="37146-204">Değişiklik belirteci, önbelleğe alınmış durumdayken dosya değişirse önbellek girdisini çıkarmak için [Memorycacheentryextensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) ile birlikte eklenir.</span><span class="sxs-lookup"><span data-stu-id="37146-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="37146-205">Aşağıdaki örnekte, dosyalar uygulamanın [içerik kökünde](xref:fundamentals/index#content-root)saklanır.</span><span class="sxs-lookup"><span data-stu-id="37146-205">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="37146-206">`IWebHostEnvironment.ContentRootFileProvider`, uygulamanın `IWebHostEnvironment.ContentRootPath` ' ye işaret eden bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37146-206">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="37146-207">@No__t-0, [ıfıleınfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath)ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="37146-208">@No__t-0, bellek önbelleği hizmeti ile birlikte hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="37146-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="37146-209">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="37146-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="37146-210">Sayfa modeli, hizmeti kullanarak dosyanın içeriğini yükler.</span><span class="sxs-lookup"><span data-stu-id="37146-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="37146-211">Dizin sayfasının `OnGet` yönteminde (*Pages/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="37146-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="37146-212">CompositeChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="37146-212">CompositeChangeToken class</span></span>

<span data-ttu-id="37146-213">Tek bir nesnedeki bir veya daha fazla `IChangeToken` örneğini temsil etmek için <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="37146-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="37146-214">`HasChanged` bileşik belirteç raporlarında, `HasChanged` ' nin temsil edilen herhangi bir belirteç `true` ise-1 @no__t.</span><span class="sxs-lookup"><span data-stu-id="37146-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="37146-215">`ActiveChangeCallbacks` bileşik belirteç raporlarında, `ActiveChangeCallbacks` ' nin temsil edilen herhangi bir belirteç `true` ise-1 @no__t.</span><span class="sxs-lookup"><span data-stu-id="37146-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="37146-216">Birden çok eşzamanlı değişiklik olayı oluşursa, bileşik değişiklik geri çağırması bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="37146-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="37146-217">*Değişiklik belirteci* , durum değişikliklerini izlemek için kullanılan genel amaçlı, düşük düzey bir yapı taşıdır.</span><span class="sxs-lookup"><span data-stu-id="37146-217">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="37146-218">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37146-218">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="37146-219">Ichangetoken arabirimi</span><span class="sxs-lookup"><span data-stu-id="37146-219">IChangeToken interface</span></span>

<span data-ttu-id="37146-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> bir değişikliğin gerçekleştiği bildirimleri yayar.</span><span class="sxs-lookup"><span data-stu-id="37146-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="37146-221">`IChangeToken` <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="37146-221">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37146-222">[Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmayan uygulamalar Için, [Microsoft. Extensions. ilkel](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi için bir paket başvurusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="37146-222">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="37146-223">`IChangeToken` iki özelliğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="37146-223">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="37146-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks>, belirtecin etkin olmayan geri çağırmaları harekete geçirmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="37146-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="37146-225">@No__t-0 `false` olarak ayarlanırsa, bir geri çağırma hiçbir şekilde çağrılmaz ve uygulamanın değişiklikler için `HasChanged` ' yi yoklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="37146-225">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="37146-226">Hiçbir değişiklik gerçekleşmüyorsa veya temeldeki değişiklik dinleyicisi atıldığı veya devre dışı bırakıldığında belirtecin hiçbir şekilde iptal edilmemesi de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="37146-226">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="37146-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğini gösteren bir değer alır.</span><span class="sxs-lookup"><span data-stu-id="37146-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="37146-228">@No__t-0 arabirimi, belirteç değiştirildiğinde çağrılan bir geri aramayı kaydeden [Registerchangecallback (Action @ no__t-2object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemini içerir.</span><span class="sxs-lookup"><span data-stu-id="37146-228">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="37146-229">`HasChanged`, geri çağırma çağrılmadan önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="37146-229">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="37146-230">ChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="37146-230">ChangeToken class</span></span>

<span data-ttu-id="37146-231"><xref:Microsoft.Extensions.Primitives.ChangeToken>, bir değişikliğin gerçekleştiği bildirimleri yaymak için kullanılan statik bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="37146-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="37146-232">`ChangeToken` <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="37146-232">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="37146-233">[Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmayan uygulamalar Için, [Microsoft. Extensions. ilkel](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi için bir paket başvurusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="37146-233">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="37146-234">[ChangeToken. OnChange (Func @ no__t-1IChangeToken >, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi, belirteç her değiştiğinde çağırmak için bir `Action` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="37146-234">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="37146-235">`Func<IChangeToken>` belirteci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37146-235">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="37146-236">belirteç değiştiğinde `Action` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="37146-236">`Action` is called when the token changes.</span></span>

<span data-ttu-id="37146-237">[ChangeToken. OnChange @ no__t-1TState > (Func @ no__t-2IChangeToken >, Action @ no__t-3TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı yüklemesi, token tüketicisi `Action` ' e geçirilen ek bir `TState` parametresi alır.</span><span class="sxs-lookup"><span data-stu-id="37146-237">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="37146-238">`OnChange` <xref:System.IDisposable> döndürür.</span><span class="sxs-lookup"><span data-stu-id="37146-238">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="37146-239">@No__t çağrılması-0, daha fazla değişiklik için dinlemeyi dinlemeden ve belirtecin kaynaklarını yayınlarından bırakır.</span><span class="sxs-lookup"><span data-stu-id="37146-239">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="37146-240">ASP.NET Core değişiklik belirteçlerinin örnek kullanımları</span><span class="sxs-lookup"><span data-stu-id="37146-240">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="37146-241">Değişiklik belirteçleri, nesnelerde yapılan değişiklikleri izlemek için ASP.NET Core belirgin alanlarında kullanılır:</span><span class="sxs-lookup"><span data-stu-id="37146-241">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="37146-242">Dosyalarda yapılan değişiklikleri izlemek için, <xref:Microsoft.Extensions.FileProviders.IFileProvider> ' ı <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi, belirtilen dosya veya klasör için bir `IChangeToken` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37146-242">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="37146-243">`IChangeToken` belirteçleri, değişiklik üzerine önbellek çıkarmaları tetiklemek için önbellek girişlerine eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="37146-243">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="37146-244">@No__t 0 değişiklikleri için, <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ' nin varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulamasının bir veya daha fazla <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örneğini kabul eden bir aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="37146-244">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="37146-245">Her örnek, izleme seçenekleri değişiklikleri için değişiklik bildirimi geri aramasını kaydetmek üzere bir @no__t döndürür.</span><span class="sxs-lookup"><span data-stu-id="37146-245">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="37146-246">Yapılandırma değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="37146-246">Monitor for configuration changes</span></span>

<span data-ttu-id="37146-247">Varsayılan olarak, ASP.NET Core şablonlar [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, appSettings) kullanır *. Development. JSON*ve *appSettings. Üretim. JSON*), uygulama yapılandırma ayarlarını yükler.</span><span class="sxs-lookup"><span data-stu-id="37146-247">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="37146-248">Bu dosyalar, `reloadOnChange` parametresini kabul eden <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ' de [Addjsonfile (ıseationbuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="37146-248">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="37146-249">`reloadOnChange`, yapılandırmanın dosya değişikliklerinde yeniden yüklenmesi gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-249">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="37146-250">Bu ayar <xref:Microsoft.AspNetCore.WebHost> kullanışlı yöntemi <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ' de görünür:</span><span class="sxs-lookup"><span data-stu-id="37146-250">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="37146-251">Dosya tabanlı yapılandırma <xref:Microsoft.Extensions.Configuration.FileConfigurationSource> ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-251">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="37146-252">`FileConfigurationSource`, dosyaları izlemek için <xref:Microsoft.Extensions.FileProviders.IFileProvider> kullanır.</span><span class="sxs-lookup"><span data-stu-id="37146-252">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="37146-253">Varsayılan olarak `IFileMonitor`, yapılandırma dosyası değişikliklerini izlemek için <xref:System.IO.FileSystemWatcher> kullanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="37146-253">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="37146-254">Örnek uygulama, yapılandırma değişikliklerini izlemek için iki uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-254">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="37146-255">*AppSettings* dosyalarından herhangi biri değiştiğinde, dosya izleme uygulamalarının her ikisi de özel kod yürütür @ no__t-1örnek uygulama konsola bir ileti yazar.</span><span class="sxs-lookup"><span data-stu-id="37146-255">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="37146-256">Yapılandırma dosyası `FileSystemWatcher`, tek bir yapılandırma dosyası değişikliği için birden çok belirteç geri çağırmaları tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="37146-256">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="37146-257">Özel kodun, birden fazla belirteç geri çağırma işlemi tetiklendiğinde yalnızca bir kez çalıştığından emin olmak için, örnek uygulama dosya karmalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="37146-257">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="37146-258">Örnek, SHA1 dosya karma kullanır.</span><span class="sxs-lookup"><span data-stu-id="37146-258">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="37146-259">Bir yeniden deneme, üstel geri dönme ile uygulanır.</span><span class="sxs-lookup"><span data-stu-id="37146-259">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="37146-260">Dosya kilitlemesi, geçici olarak bir dosyada yeni bir karma işlem yapılmasını önleyen dosya kilitleme gerçekleşebileceğinden, yeniden deneme vardır.</span><span class="sxs-lookup"><span data-stu-id="37146-260">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="37146-261">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-261">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="37146-262">Basit başlangıç değiştirme belirteci</span><span class="sxs-lookup"><span data-stu-id="37146-262">Simple startup change token</span></span>

<span data-ttu-id="37146-263">Yapılandırma yeniden yükleme belirtecine değişiklik bildirimleri için bir belirteç tüketicisi `Action` geri araması kaydedin.</span><span class="sxs-lookup"><span data-stu-id="37146-263">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="37146-264">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="37146-264">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="37146-265">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="37146-265">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="37146-266">Geri çağırma `InvokeChanged` yöntemidir:</span><span class="sxs-lookup"><span data-stu-id="37146-266">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="37146-267">Geri aramanın `state` ' ı, izlenecek doğru *appSettings* yapılandırma dosyasını (örneğin, appSettings) belirtmek için yararlı olan `IHostingEnvironment` ' e geçirmek için kullanılır *. Geliştirme ortamında geliştirme. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="37146-267">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="37146-268">Dosya karmaları, yapılandırma dosyası yalnızca bir kez değiştirildiğinde, birden çok belirteç geri çağırmaları nedeniyle `WriteConsole` ifadesinin birden çok kez çalışmasını engellemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37146-268">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="37146-269">Uygulama çalıştığı sürece bu sistem çalışır ve Kullanıcı tarafından devre dışı bırakılamaz.</span><span class="sxs-lookup"><span data-stu-id="37146-269">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="37146-270">Yapılandırma değişikliklerini hizmet olarak izle</span><span class="sxs-lookup"><span data-stu-id="37146-270">Monitor configuration changes as a service</span></span>

<span data-ttu-id="37146-271">Örnek şunları uygular:</span><span class="sxs-lookup"><span data-stu-id="37146-271">The sample implements:</span></span>

* <span data-ttu-id="37146-272">Temel başlangıç belirteci izleme.</span><span class="sxs-lookup"><span data-stu-id="37146-272">Basic startup token monitoring.</span></span>
* <span data-ttu-id="37146-273">Hizmet olarak izleme.</span><span class="sxs-lookup"><span data-stu-id="37146-273">Monitoring as a service.</span></span>
* <span data-ttu-id="37146-274">İzlemeyi etkinleştirme ve devre dışı bırakma mekanizması.</span><span class="sxs-lookup"><span data-stu-id="37146-274">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="37146-275">Örnek, `IConfigurationMonitor` arabirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37146-275">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="37146-276">*Uzantılar/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-276">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="37146-277">Uygulanan sınıfın Oluşturucusu `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:</span><span class="sxs-lookup"><span data-stu-id="37146-277">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="37146-278">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="37146-278">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="37146-279">`InvokeChanged`, geri çağırma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="37146-279">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="37146-280">Bu örnekteki `state`, izleme durumuna erişmek için kullanılan `IConfigurationMonitor` örneğine bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="37146-280">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="37146-281">İki özellik kullanılır:</span><span class="sxs-lookup"><span data-stu-id="37146-281">Two properties are used:</span></span>

* <span data-ttu-id="37146-282">`MonitoringEnabled` &ndash;, geri aramanın özel kodunu çalıştırması gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-282">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="37146-283">`CurrentState` &ndash; Kullanıcı arabiriminde kullanım için geçerli izleme durumunu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="37146-283">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="37146-284">@No__t-0 yöntemi önceki yaklaşımla benzerdir, bunun dışında:</span><span class="sxs-lookup"><span data-stu-id="37146-284">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="37146-285">@No__t-0 `true` olmadığı müddetçe kodunu çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="37146-285">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="37146-286">Geçerli `state` ' ı `WriteConsole` çıkışındaki çıkarır.</span><span class="sxs-lookup"><span data-stu-id="37146-286">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="37146-287">Bir örnek `ConfigurationMonitor` `Startup.ConfigureServices` ' de hizmet olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="37146-287">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="37146-288">Dizin sayfası, yapılandırma izleme üzerinde Kullanıcı denetimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="37146-288">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="37146-289">@No__t-0 ' ın örneği, `IndexModel` ' e eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="37146-289">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="37146-290">*Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-290">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="37146-291">Yapılandırma İzleyicisi (`_monitor`), izlemeyi etkinleştirmek veya devre dışı bırakmak ve UI geri bildirimi için geçerli durumu ayarlamak üzere kullanılır:</span><span class="sxs-lookup"><span data-stu-id="37146-291">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="37146-292">@No__t-0 tetiklendiğinde, izleme etkinleştirilir ve geçerli durum temizlenir.</span><span class="sxs-lookup"><span data-stu-id="37146-292">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="37146-293">@No__t-0 tetiklendiğinde, izleme devre dışıdır ve durum, izlemenin gerçekleşmediğinden emin olmak üzere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="37146-293">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="37146-294">Kullanıcı arabirimindeki düğmeler izlemeyi etkinleştirir ve devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="37146-294">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="37146-295">*Sayfa/dizin. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37146-295">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="37146-296">Önbelleğe alınmış dosya değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="37146-296">Monitor cached file changes</span></span>

<span data-ttu-id="37146-297">Dosya içeriği, <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> kullanılarak bellek içinde önbelleğe alınabilir.</span><span class="sxs-lookup"><span data-stu-id="37146-297">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="37146-298">Bellek içi önbelleğe alma, [bellek Içi önbellek](xref:performance/caching/memory) konusunda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="37146-298">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="37146-299">Aşağıda açıklanan uygulama gibi ek adımlar uygulamadan, kaynak veriler değişirse önbellekten *eski* (eski) veriler döndürülür.</span><span class="sxs-lookup"><span data-stu-id="37146-299">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="37146-300">Örneğin, bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) dönemini yenilerken önbelleğe alınmış bir kaynak dosyanın durumu, eski önbelleğe alınmış dosya verilerine yol açar.</span><span class="sxs-lookup"><span data-stu-id="37146-300">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="37146-301">Verilerin her isteği, Kayan süre sonu süresini yeniler, ancak dosya hiçbir zaman önbelleğe yeniden yüklenmez.</span><span class="sxs-lookup"><span data-stu-id="37146-301">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="37146-302">Dosyanın önbelleğe alınmış içeriğini kullanan tüm uygulama özellikleri, büyük olasılıkla eski içerik alınmasına tabidir.</span><span class="sxs-lookup"><span data-stu-id="37146-302">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="37146-303">Bir dosya önbelleğe alma senaryosunda değişiklik belirteçlerini kullanmak önbellekte eski dosya içeriğinin varlığını engeller.</span><span class="sxs-lookup"><span data-stu-id="37146-303">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="37146-304">Örnek uygulama, yaklaşımın bir uygulamasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="37146-304">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="37146-305">Örnek, `GetFileContent` ' i kullanır:</span><span class="sxs-lookup"><span data-stu-id="37146-305">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="37146-306">Dosya içeriğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="37146-306">Return file content.</span></span>
* <span data-ttu-id="37146-307">Bir dosya kilidinin geçici olarak bir dosya okumayı engellediği durumları kapsamak için üstel geri ile yeniden deneme algoritması uygulayın.</span><span class="sxs-lookup"><span data-stu-id="37146-307">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="37146-308">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="37146-308">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="37146-309">Önbelleğe alınmış dosya aramalarını işlemek için bir `FileService` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="37146-309">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="37146-310">Hizmetin `GetFileContent` Yöntem çağrısı, bellek içi önbellekten dosya içeriğini almaya çalışır ve bunu çağırana (*Services/FileService. cs*) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="37146-310">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="37146-311">Önbellek anahtarı kullanılarak önbelleğe alınmış içerik bulunamazsa, aşağıdaki eylemler gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="37146-311">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="37146-312">Dosya içeriği `GetFileContent` kullanılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-312">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="37146-313">Dosya sağlayıcısından [ıfileproviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*)ile bir değişiklik belirteci elde edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-313">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="37146-314">Dosya değiştirildiğinde belirtecin geri çağırması tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="37146-314">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="37146-315">Dosya içeriği bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresiyle önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="37146-315">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="37146-316">Değişiklik belirteci, önbelleğe alınmış durumdayken dosya değişirse önbellek girdisini çıkarmak için [Memorycacheentryextensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) ile birlikte eklenir.</span><span class="sxs-lookup"><span data-stu-id="37146-316">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="37146-317">Aşağıdaki örnekte, dosyalar uygulamanın [içerik kökünde](xref:fundamentals/index#content-root)saklanır.</span><span class="sxs-lookup"><span data-stu-id="37146-317">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="37146-318">[Ihostingenvironment. ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) , uygulamanın <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> ' ye işaret eden <xref:Microsoft.Extensions.FileProviders.IFileProvider> almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37146-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="37146-319">@No__t-0, [ıfıleınfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath)ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="37146-319">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="37146-320">@No__t-0, bellek önbelleği hizmeti ile birlikte hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="37146-320">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="37146-321">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="37146-321">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="37146-322">Sayfa modeli, hizmeti kullanarak dosyanın içeriğini yükler.</span><span class="sxs-lookup"><span data-stu-id="37146-322">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="37146-323">Dizin sayfasının `OnGet` yönteminde (*Pages/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="37146-323">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="37146-324">CompositeChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="37146-324">CompositeChangeToken class</span></span>

<span data-ttu-id="37146-325">Tek bir nesnedeki bir veya daha fazla `IChangeToken` örneğini temsil etmek için <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="37146-325">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="37146-326">`HasChanged` bileşik belirteç raporlarında, `HasChanged` ' nin temsil edilen herhangi bir belirteç `true` ise-1 @no__t.</span><span class="sxs-lookup"><span data-stu-id="37146-326">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="37146-327">`ActiveChangeCallbacks` bileşik belirteç raporlarında, `ActiveChangeCallbacks` ' nin temsil edilen herhangi bir belirteç `true` ise-1 @no__t.</span><span class="sxs-lookup"><span data-stu-id="37146-327">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="37146-328">Birden çok eşzamanlı değişiklik olayı oluşursa, bileşik değişiklik geri çağırması bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="37146-328">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="37146-329">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="37146-329">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
