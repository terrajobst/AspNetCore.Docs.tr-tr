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
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="a9f4a-103">ASP.NET Core değişiklik belirteçleriyle değişiklikleri Algıla</span><span class="sxs-lookup"><span data-stu-id="a9f4a-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="a9f4a-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a9f4a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a9f4a-105">*Değişiklik belirteci* , durum değişikliklerini izlemek için kullanılan genel amaçlı, düşük düzey bir yapı taşıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="a9f4a-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9f4a-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="a9f4a-107">Ichangetoken arabirimi</span><span class="sxs-lookup"><span data-stu-id="a9f4a-107">IChangeToken interface</span></span>

<span data-ttu-id="a9f4a-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> bir değişikliğin gerçekleştiği bildirimleri yayar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="a9f4a-109">`IChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="a9f4a-110">[Microsoft. Extensions. Ilkel öğeler](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi, ASP.NET Core uygulamalarına örtük olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-110">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="a9f4a-111">`IChangeToken` iki özelliğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="a9f4a-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> belirtecin, geri çağırmaları etkin bir şekilde harekete geçirmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="a9f4a-113">`ActiveChangedCallbacks` `false`olarak ayarlanırsa, bir geri çağırma hiçbir şekilde çağrılmaz ve uygulamanın değişiklikler için `HasChanged` yoklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="a9f4a-114">Hiçbir değişiklik gerçekleşmüyorsa veya temeldeki değişiklik dinleyicisi atıldığı veya devre dışı bırakıldığında belirtecin hiçbir şekilde iptal edilmemesi de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="a9f4a-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğini gösteren bir değer alır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="a9f4a-116">`IChangeToken` arabirimi, belirteç değiştirildiğinde çağrılan bir geri aramayı kaydeden [Registerchangecallback (Action\<Object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemini içerir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="a9f4a-117">`HasChanged`, geri çağırma çağrılmadan önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="a9f4a-118">ChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="a9f4a-118">ChangeToken class</span></span>

<span data-ttu-id="a9f4a-119"><xref:Microsoft.Extensions.Primitives.ChangeToken>, bir değişikliğin gerçekleştiği bildirimleri yaymak için kullanılan statik bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="a9f4a-120">`ChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="a9f4a-121">[Microsoft. Extensions. Ilkel öğeler](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi, ASP.NET Core uygulamalarına örtük olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-121">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="a9f4a-122">[ChangeToken. OnChange (Func\<IChannel>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi, belirteç her değiştiğinde çağırmak için bir `Action` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="a9f4a-123">`Func<IChangeToken>` belirteci üretir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="a9f4a-124">belirteç değiştiğinde `Action` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="a9f4a-125">[ChangeToken. OnChange\<tstate > (Func\<ıhangetoken >, Action\<tstate >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı yüklemesi, token tüketicisi `TState` geçirilen ek bir `Action`parametresini alır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="a9f4a-126">`OnChange` <xref:System.IDisposable>döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="a9f4a-127"><xref:System.IDisposable.Dispose*> çağırmak, belirteci daha fazla değişiklik için dinlemeyi durdurup belirtecin kaynaklarını serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="a9f4a-128">ASP.NET Core değişiklik belirteçlerinin örnek kullanımları</span><span class="sxs-lookup"><span data-stu-id="a9f4a-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="a9f4a-129">Değişiklik belirteçleri, nesnelerde yapılan değişiklikleri izlemek için ASP.NET Core belirgin alanlarında kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="a9f4a-130">Dosyalarda yapılan değişiklikleri izlemek için, <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi, belirtilen dosya veya klasör için izlemek üzere bir `IChangeToken` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="a9f4a-131">değişiklik üzerine önbellek çıkarmaları tetiklemek için, `IChangeToken` belirteçleri önbellek girişlerine eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="a9f4a-132">`TOptions` değişiklikler için <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulamasının bir veya daha fazla <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örneğini kabul eden bir aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="a9f4a-133">Her örnek, izleme seçenekleri değişiklikleri için değişiklik bildirimi geri aramasını kaydetmek üzere bir `IChangeToken` döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="a9f4a-134">Yapılandırma değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="a9f4a-134">Monitor for configuration changes</span></span>

<span data-ttu-id="a9f4a-135">Varsayılan olarak, ASP.NET Core şablonlar [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, appSettings) kullanır *. Development. JSON*ve *appSettings. Üretim. JSON*), uygulama yapılandırma ayarlarını yükler.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="a9f4a-136">Bu dosyalar, bir `reloadOnChange` parametresini kabul eden <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> için [Addjsonfile (ıseationbuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="a9f4a-137">`reloadOnChange`, yapılandırmanın dosya değişikliklerinde yeniden yüklenmesi gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="a9f4a-138">Bu ayar <xref:Microsoft.Extensions.Hosting.Host> kullanışlı yöntemde görüntülenir <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-138">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="a9f4a-139">Dosya tabanlı yapılandırma <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>tarafından temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="a9f4a-140">`FileConfigurationSource` dosyaları izlemek için <xref:Microsoft.Extensions.FileProviders.IFileProvider> kullanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="a9f4a-141">Varsayılan olarak `IFileMonitor`, yapılandırma dosyası değişikliklerini izlemek için <xref:System.IO.FileSystemWatcher> kullanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="a9f4a-142">Örnek uygulama, yapılandırma değişikliklerini izlemek için iki uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="a9f4a-143">*AppSettings* dosyalarından herhangi biri değiştiğinde, dosya izleme uygulamalarının her ikisi de özel kod yürütür&mdash;örnek uygulama konsola bir ileti yazar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="a9f4a-144">Yapılandırma dosyası `FileSystemWatcher`, tek bir yapılandırma dosyası değişikliği için birden çok belirteç geri çağırmaları tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="a9f4a-145">Özel kodun, birden fazla belirteç geri çağırma işlemi tetiklendiğinde yalnızca bir kez çalıştığından emin olmak için, örnek uygulama dosya karmalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="a9f4a-146">Örnek, SHA1 dosya karma kullanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="a9f4a-147">Bir yeniden deneme, üstel geri dönme ile uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="a9f4a-148">Dosya kilitlemesi, geçici olarak bir dosyada yeni bir karma işlem yapılmasını önleyen dosya kilitleme gerçekleşebileceğinden, yeniden deneme vardır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="a9f4a-149">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="a9f4a-150">Basit başlangıç değiştirme belirteci</span><span class="sxs-lookup"><span data-stu-id="a9f4a-150">Simple startup change token</span></span>

<span data-ttu-id="a9f4a-151">Değişiklik bildirimleri için bir belirteç tüketicisi `Action` geri çağırma işlemini yapılandırma yeniden yükleme belirtecine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="a9f4a-152">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="a9f4a-153">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="a9f4a-154">Geri çağırma `InvokeChanged` yöntemidir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="a9f4a-155">Geri aramanın `state`, izlenecek doğru *appSettings* yapılandırma dosyasını (örneğin, appSettings) belirtmek için yararlı olan `IWebHostEnvironment`geçirmek için kullanılır *. Geliştirme ortamında geliştirme. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="a9f4a-155">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="a9f4a-156">Dosya karmaları, yapılandırma dosyası yalnızca bir kez değiştirildiğinde, birden çok belirteç geri çağırmaları nedeniyle `WriteConsole` deyimin birden çok kez çalışmasını engellemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="a9f4a-157">Uygulama çalıştığı sürece bu sistem çalışır ve Kullanıcı tarafından devre dışı bırakılamaz.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="a9f4a-158">Yapılandırma değişikliklerini hizmet olarak izle</span><span class="sxs-lookup"><span data-stu-id="a9f4a-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="a9f4a-159">Örnek şunları uygular:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-159">The sample implements:</span></span>

* <span data-ttu-id="a9f4a-160">Temel başlangıç belirteci izleme.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="a9f4a-161">Hizmet olarak izleme.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-161">Monitoring as a service.</span></span>
* <span data-ttu-id="a9f4a-162">İzlemeyi etkinleştirme ve devre dışı bırakma mekanizması.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="a9f4a-163">Örnek bir `IConfigurationMonitor` arabirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="a9f4a-164">*Uzantılar/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="a9f4a-165">Uygulanan sınıfın Oluşturucusu `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="a9f4a-166">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="a9f4a-167">`InvokeChanged` geri çağırma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="a9f4a-168">Bu örnekteki `state`, izleme durumuna erişmek için kullanılan `IConfigurationMonitor` örneğine bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="a9f4a-169">İki özellik kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-169">Two properties are used:</span></span>

* <span data-ttu-id="a9f4a-170">`MonitoringEnabled` &ndash;, geri aramanın özel kodunu çalıştırıp çalıştıramayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="a9f4a-171">`CurrentState` &ndash;, Kullanıcı arabirimindeki kullanım için geçerli izleme durumunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="a9f4a-172">`InvokeChanged` yöntemi önceki yaklaşımla benzerdir, bunun dışında:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="a9f4a-173">`MonitoringEnabled` `true`olmadığı müddetçe kodunu çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="a9f4a-174">Geçerli `state` `WriteConsole` çıktısındaki çıktısını verir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="a9f4a-175">Bir örnek `ConfigurationMonitor` `Startup.ConfigureServices`bir hizmet olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="a9f4a-176">Dizin sayfası, yapılandırma izleme üzerinde Kullanıcı denetimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="a9f4a-177">`IConfigurationMonitor` örneği `IndexModel`eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="a9f4a-178">*Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a9f4a-179">Yapılandırma İzleyicisi (`_monitor`), izlemeyi etkinleştirmek veya devre dışı bırakmak ve UI geri bildirimi için geçerli durumu ayarlamak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="a9f4a-180">`OnPostStartMonitoring` tetiklendiğinde, izleme etkinleştirilir ve geçerli durum temizlenir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="a9f4a-181">`OnPostStopMonitoring` tetiklendiğinde, izleme devre dışıdır ve durum, izlemenin gerçekleşmediğinden emin olmak üzere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="a9f4a-182">Kullanıcı arabirimindeki düğmeler izlemeyi etkinleştirir ve devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="a9f4a-183">*Sayfa/dizin. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="a9f4a-184">Önbelleğe alınmış dosya değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="a9f4a-184">Monitor cached file changes</span></span>

<span data-ttu-id="a9f4a-185">Dosya içeriği, <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>kullanarak bellek içi önbelleğe alınabilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="a9f4a-186">Bellek içi önbelleğe alma, [bellek Içi önbellek](xref:performance/caching/memory) konusunda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="a9f4a-187">Aşağıda açıklanan uygulama gibi ek adımlar uygulamadan, kaynak veriler değişirse önbellekten *eski* (eski) veriler döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="a9f4a-188">Örneğin, bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) dönemini yenilerken önbelleğe alınmış bir kaynak dosyanın durumu, eski önbelleğe alınmış dosya verilerine yol açar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="a9f4a-189">Verilerin her isteği, Kayan süre sonu süresini yeniler, ancak dosya hiçbir zaman önbelleğe yeniden yüklenmez.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="a9f4a-190">Dosyanın önbelleğe alınmış içeriğini kullanan tüm uygulama özellikleri, büyük olasılıkla eski içerik alınmasına tabidir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="a9f4a-191">Bir dosya önbelleğe alma senaryosunda değişiklik belirteçlerini kullanmak önbellekte eski dosya içeriğinin varlığını engeller.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="a9f4a-192">Örnek uygulama, yaklaşımın bir uygulamasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="a9f4a-193">Örnek, `GetFileContent` için şunu kullanır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="a9f4a-194">Dosya içeriğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-194">Return file content.</span></span>
* <span data-ttu-id="a9f4a-195">Bir dosya kilidinin geçici olarak bir dosya okumayı engellediği durumları kapsamak için üstel geri ile yeniden deneme algoritması uygulayın.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="a9f4a-196">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="a9f4a-197">Önbelleğe alınmış dosya aramalarını işlemek için bir `FileService` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="a9f4a-198">Hizmet `GetFileContent` yöntemi çağrısı, bellek içi önbellekten dosya içeriğini almaya çalışır ve bunu çağırana (*Services/FileService. cs*) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="a9f4a-199">Önbellek anahtarı kullanılarak önbelleğe alınmış içerik bulunamazsa, aşağıdaki eylemler gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="a9f4a-200">Dosya içeriği `GetFileContent`kullanılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="a9f4a-201">Dosya sağlayıcısından [ıfileproviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*)ile bir değişiklik belirteci elde edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="a9f4a-202">Dosya değiştirildiğinde belirtecin geri çağırması tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="a9f4a-203">Dosya içeriği bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresiyle önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="a9f4a-204">Değişiklik belirteci, önbelleğe alınmış durumdayken dosya değişirse önbellek girdisini çıkarmak için [Memorycacheentryextensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) ile birlikte eklenir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="a9f4a-205">Aşağıdaki örnekte, dosyalar uygulamanın [içerik kökünde](xref:fundamentals/index#content-root)saklanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-205">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="a9f4a-206">`IWebHostEnvironment.ContentRootFileProvider`, uygulamanın `IWebHostEnvironment.ContentRootPath`işaret eden bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-206">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="a9f4a-207">`filePath`, [ıfıleınfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath)ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="a9f4a-208">`FileService`, bellek önbelleği hizmeti ile birlikte hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="a9f4a-209">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="a9f4a-210">Sayfa modeli, hizmeti kullanarak dosyanın içeriğini yükler.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="a9f4a-211">Dizin sayfasının `OnGet` yönteminde (*Pages/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="a9f4a-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="a9f4a-212">CompositeChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="a9f4a-212">CompositeChangeToken class</span></span>

<span data-ttu-id="a9f4a-213">Tek bir nesnedeki bir veya daha fazla `IChangeToken` örneğini temsil etmek için <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="a9f4a-214">`HasChanged`, temsil edilen herhangi bir belirteç `HasChanged` `true`ise bileşik belirteç raporlarında `true`.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="a9f4a-215">`ActiveChangeCallbacks`, temsil edilen herhangi bir belirteç `ActiveChangeCallbacks` `true`ise bileşik belirteç raporlarında `true`.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="a9f4a-216">Birden çok eşzamanlı değişiklik olayı oluşursa, bileşik değişiklik geri çağırması bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a9f4a-217">*Değişiklik belirteci* , durum değişikliklerini izlemek için kullanılan genel amaçlı, düşük düzey bir yapı taşıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-217">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="a9f4a-218">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9f4a-218">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="a9f4a-219">Ichangetoken arabirimi</span><span class="sxs-lookup"><span data-stu-id="a9f4a-219">IChangeToken interface</span></span>

<span data-ttu-id="a9f4a-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> bir değişikliğin gerçekleştiği bildirimleri yayar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="a9f4a-221">`IChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-221">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="a9f4a-222">[Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmayan uygulamalar Için, [Microsoft. Extensions. ilkel](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi için bir paket başvurusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-222">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="a9f4a-223">`IChangeToken` iki özelliğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-223">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="a9f4a-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> belirtecin, geri çağırmaları etkin bir şekilde harekete geçirmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="a9f4a-225">`ActiveChangedCallbacks` `false`olarak ayarlanırsa, bir geri çağırma hiçbir şekilde çağrılmaz ve uygulamanın değişiklikler için `HasChanged` yoklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-225">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="a9f4a-226">Hiçbir değişiklik gerçekleşmüyorsa veya temeldeki değişiklik dinleyicisi atıldığı veya devre dışı bırakıldığında belirtecin hiçbir şekilde iptal edilmemesi de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-226">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="a9f4a-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğini gösteren bir değer alır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="a9f4a-228">`IChangeToken` arabirimi, belirteç değiştirildiğinde çağrılan bir geri aramayı kaydeden [Registerchangecallback (Action\<Object >, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemini içerir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-228">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="a9f4a-229">`HasChanged`, geri çağırma çağrılmadan önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-229">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="a9f4a-230">ChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="a9f4a-230">ChangeToken class</span></span>

<span data-ttu-id="a9f4a-231"><xref:Microsoft.Extensions.Primitives.ChangeToken>, bir değişikliğin gerçekleştiği bildirimleri yaymak için kullanılan statik bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="a9f4a-232">`ChangeToken`, <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-232">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="a9f4a-233">[Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmayan uygulamalar Için, [Microsoft. Extensions. ilkel](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi için bir paket başvurusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-233">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="a9f4a-234">[ChangeToken. OnChange (Func\<IChannel>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi, belirteç her değiştiğinde çağırmak için bir `Action` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-234">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="a9f4a-235">`Func<IChangeToken>` belirteci üretir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-235">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="a9f4a-236">belirteç değiştiğinde `Action` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-236">`Action` is called when the token changes.</span></span>

<span data-ttu-id="a9f4a-237">[ChangeToken. OnChange\<tstate > (Func\<ıhangetoken >, Action\<tstate >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı yüklemesi, token tüketicisi `TState` geçirilen ek bir `Action`parametresini alır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-237">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="a9f4a-238">`OnChange` <xref:System.IDisposable>döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-238">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="a9f4a-239"><xref:System.IDisposable.Dispose*> çağırmak, belirteci daha fazla değişiklik için dinlemeyi durdurup belirtecin kaynaklarını serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-239">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="a9f4a-240">ASP.NET Core değişiklik belirteçlerinin örnek kullanımları</span><span class="sxs-lookup"><span data-stu-id="a9f4a-240">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="a9f4a-241">Değişiklik belirteçleri, nesnelerde yapılan değişiklikleri izlemek için ASP.NET Core belirgin alanlarında kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-241">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="a9f4a-242">Dosyalarda yapılan değişiklikleri izlemek için, <xref:Microsoft.Extensions.FileProviders.IFileProvider><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi, belirtilen dosya veya klasör için izlemek üzere bir `IChangeToken` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-242">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="a9f4a-243">değişiklik üzerine önbellek çıkarmaları tetiklemek için, `IChangeToken` belirteçleri önbellek girişlerine eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-243">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="a9f4a-244">`TOptions` değişiklikler için <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulamasının bir veya daha fazla <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örneğini kabul eden bir aşırı yüklemesi vardır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-244">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="a9f4a-245">Her örnek, izleme seçenekleri değişiklikleri için değişiklik bildirimi geri aramasını kaydetmek üzere bir `IChangeToken` döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-245">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="a9f4a-246">Yapılandırma değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="a9f4a-246">Monitor for configuration changes</span></span>

<span data-ttu-id="a9f4a-247">Varsayılan olarak, ASP.NET Core şablonlar [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration-provider) (*appSettings. JSON*, appSettings) kullanır *. Development. JSON*ve *appSettings. Üretim. JSON*), uygulama yapılandırma ayarlarını yükler.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-247">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="a9f4a-248">Bu dosyalar, bir `reloadOnChange` parametresini kabul eden <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> için [Addjsonfile (ıseationbuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-248">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="a9f4a-249">`reloadOnChange`, yapılandırmanın dosya değişikliklerinde yeniden yüklenmesi gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-249">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="a9f4a-250">Bu ayar <xref:Microsoft.AspNetCore.WebHost> kullanışlı yöntemde görüntülenir <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-250">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="a9f4a-251">Dosya tabanlı yapılandırma <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>tarafından temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-251">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="a9f4a-252">`FileConfigurationSource` dosyaları izlemek için <xref:Microsoft.Extensions.FileProviders.IFileProvider> kullanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-252">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="a9f4a-253">Varsayılan olarak `IFileMonitor`, yapılandırma dosyası değişikliklerini izlemek için <xref:System.IO.FileSystemWatcher> kullanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-253">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="a9f4a-254">Örnek uygulama, yapılandırma değişikliklerini izlemek için iki uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-254">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="a9f4a-255">*AppSettings* dosyalarından herhangi biri değiştiğinde, dosya izleme uygulamalarının her ikisi de özel kod yürütür&mdash;örnek uygulama konsola bir ileti yazar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-255">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="a9f4a-256">Yapılandırma dosyası `FileSystemWatcher`, tek bir yapılandırma dosyası değişikliği için birden çok belirteç geri çağırmaları tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-256">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="a9f4a-257">Özel kodun, birden fazla belirteç geri çağırma işlemi tetiklendiğinde yalnızca bir kez çalıştığından emin olmak için, örnek uygulama dosya karmalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-257">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="a9f4a-258">Örnek, SHA1 dosya karma kullanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-258">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="a9f4a-259">Bir yeniden deneme, üstel geri dönme ile uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-259">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="a9f4a-260">Dosya kilitlemesi, geçici olarak bir dosyada yeni bir karma işlem yapılmasını önleyen dosya kilitleme gerçekleşebileceğinden, yeniden deneme vardır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-260">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="a9f4a-261">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-261">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="a9f4a-262">Basit başlangıç değiştirme belirteci</span><span class="sxs-lookup"><span data-stu-id="a9f4a-262">Simple startup change token</span></span>

<span data-ttu-id="a9f4a-263">Değişiklik bildirimleri için bir belirteç tüketicisi `Action` geri çağırma işlemini yapılandırma yeniden yükleme belirtecine kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-263">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="a9f4a-264">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-264">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="a9f4a-265">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-265">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="a9f4a-266">Geri çağırma `InvokeChanged` yöntemidir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-266">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="a9f4a-267">Geri aramanın `state`, izlenecek doğru *appSettings* yapılandırma dosyasını (örneğin, appSettings) belirtmek için yararlı olan `IHostingEnvironment`geçirmek için kullanılır *. Geliştirme ortamında geliştirme. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="a9f4a-267">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="a9f4a-268">Dosya karmaları, yapılandırma dosyası yalnızca bir kez değiştirildiğinde, birden çok belirteç geri çağırmaları nedeniyle `WriteConsole` deyimin birden çok kez çalışmasını engellemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-268">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="a9f4a-269">Uygulama çalıştığı sürece bu sistem çalışır ve Kullanıcı tarafından devre dışı bırakılamaz.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-269">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="a9f4a-270">Yapılandırma değişikliklerini hizmet olarak izle</span><span class="sxs-lookup"><span data-stu-id="a9f4a-270">Monitor configuration changes as a service</span></span>

<span data-ttu-id="a9f4a-271">Örnek şunları uygular:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-271">The sample implements:</span></span>

* <span data-ttu-id="a9f4a-272">Temel başlangıç belirteci izleme.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-272">Basic startup token monitoring.</span></span>
* <span data-ttu-id="a9f4a-273">Hizmet olarak izleme.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-273">Monitoring as a service.</span></span>
* <span data-ttu-id="a9f4a-274">İzlemeyi etkinleştirme ve devre dışı bırakma mekanizması.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-274">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="a9f4a-275">Örnek bir `IConfigurationMonitor` arabirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-275">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="a9f4a-276">*Uzantılar/ConfigurationMonitor. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-276">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="a9f4a-277">Uygulanan sınıfın Oluşturucusu `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-277">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="a9f4a-278">`config.GetReloadToken()` belirteci sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-278">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="a9f4a-279">`InvokeChanged` geri çağırma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-279">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="a9f4a-280">Bu örnekteki `state`, izleme durumuna erişmek için kullanılan `IConfigurationMonitor` örneğine bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-280">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="a9f4a-281">İki özellik kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-281">Two properties are used:</span></span>

* <span data-ttu-id="a9f4a-282">`MonitoringEnabled` &ndash;, geri aramanın özel kodunu çalıştırıp çalıştıramayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-282">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="a9f4a-283">`CurrentState` &ndash;, Kullanıcı arabirimindeki kullanım için geçerli izleme durumunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-283">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="a9f4a-284">`InvokeChanged` yöntemi önceki yaklaşımla benzerdir, bunun dışında:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-284">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="a9f4a-285">`MonitoringEnabled` `true`olmadığı müddetçe kodunu çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-285">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="a9f4a-286">Geçerli `state` `WriteConsole` çıktısındaki çıktısını verir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-286">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="a9f4a-287">Bir örnek `ConfigurationMonitor` `Startup.ConfigureServices`bir hizmet olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-287">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="a9f4a-288">Dizin sayfası, yapılandırma izleme üzerinde Kullanıcı denetimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-288">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="a9f4a-289">`IConfigurationMonitor` örneği `IndexModel`eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-289">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="a9f4a-290">*Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-290">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a9f4a-291">Yapılandırma İzleyicisi (`_monitor`), izlemeyi etkinleştirmek veya devre dışı bırakmak ve UI geri bildirimi için geçerli durumu ayarlamak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-291">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="a9f4a-292">`OnPostStartMonitoring` tetiklendiğinde, izleme etkinleştirilir ve geçerli durum temizlenir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-292">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="a9f4a-293">`OnPostStopMonitoring` tetiklendiğinde, izleme devre dışıdır ve durum, izlemenin gerçekleşmediğinden emin olmak üzere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-293">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="a9f4a-294">Kullanıcı arabirimindeki düğmeler izlemeyi etkinleştirir ve devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-294">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="a9f4a-295">*Sayfa/dizin. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-295">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="a9f4a-296">Önbelleğe alınmış dosya değişikliklerini izle</span><span class="sxs-lookup"><span data-stu-id="a9f4a-296">Monitor cached file changes</span></span>

<span data-ttu-id="a9f4a-297">Dosya içeriği, <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>kullanarak bellek içi önbelleğe alınabilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-297">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="a9f4a-298">Bellek içi önbelleğe alma, [bellek Içi önbellek](xref:performance/caching/memory) konusunda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-298">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="a9f4a-299">Aşağıda açıklanan uygulama gibi ek adımlar uygulamadan, kaynak veriler değişirse önbellekten *eski* (eski) veriler döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-299">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="a9f4a-300">Örneğin, bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) dönemini yenilerken önbelleğe alınmış bir kaynak dosyanın durumu, eski önbelleğe alınmış dosya verilerine yol açar.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-300">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="a9f4a-301">Verilerin her isteği, Kayan süre sonu süresini yeniler, ancak dosya hiçbir zaman önbelleğe yeniden yüklenmez.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-301">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="a9f4a-302">Dosyanın önbelleğe alınmış içeriğini kullanan tüm uygulama özellikleri, büyük olasılıkla eski içerik alınmasına tabidir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-302">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="a9f4a-303">Bir dosya önbelleğe alma senaryosunda değişiklik belirteçlerini kullanmak önbellekte eski dosya içeriğinin varlığını engeller.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-303">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="a9f4a-304">Örnek uygulama, yaklaşımın bir uygulamasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-304">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="a9f4a-305">Örnek, `GetFileContent` için şunu kullanır:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-305">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="a9f4a-306">Dosya içeriğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-306">Return file content.</span></span>
* <span data-ttu-id="a9f4a-307">Bir dosya kilidinin geçici olarak bir dosya okumayı engellediği durumları kapsamak için üstel geri ile yeniden deneme algoritması uygulayın.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-307">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="a9f4a-308">*Yardımcı programlar/yardımcı programlar. cs*:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-308">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="a9f4a-309">Önbelleğe alınmış dosya aramalarını işlemek için bir `FileService` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-309">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="a9f4a-310">Hizmet `GetFileContent` yöntemi çağrısı, bellek içi önbellekten dosya içeriğini almaya çalışır ve bunu çağırana (*Services/FileService. cs*) döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-310">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="a9f4a-311">Önbellek anahtarı kullanılarak önbelleğe alınmış içerik bulunamazsa, aşağıdaki eylemler gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-311">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="a9f4a-312">Dosya içeriği `GetFileContent`kullanılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-312">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="a9f4a-313">Dosya sağlayıcısından [ıfileproviders. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*)ile bir değişiklik belirteci elde edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-313">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="a9f4a-314">Dosya değiştirildiğinde belirtecin geri çağırması tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-314">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="a9f4a-315">Dosya içeriği bir [kayan süre sonu](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresiyle önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-315">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="a9f4a-316">Değişiklik belirteci, önbelleğe alınmış durumdayken dosya değişirse önbellek girdisini çıkarmak için [Memorycacheentryextensions. AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) ile birlikte eklenir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-316">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="a9f4a-317">Aşağıdaki örnekte, dosyalar uygulamanın [içerik kökünde](xref:fundamentals/index#content-root)saklanır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-317">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="a9f4a-318">[Ihostingenvironment. ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) , uygulamanın <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>işaret eden bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="a9f4a-319">`filePath`, [ıfıleınfo. PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath)ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-319">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="a9f4a-320">`FileService`, bellek önbelleği hizmeti ile birlikte hizmet kapsayıcısına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-320">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="a9f4a-321">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a9f4a-321">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="a9f4a-322">Sayfa modeli, hizmeti kullanarak dosyanın içeriğini yükler.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-322">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="a9f4a-323">Dizin sayfasının `OnGet` yönteminde (*Pages/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="a9f4a-323">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="a9f4a-324">CompositeChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="a9f4a-324">CompositeChangeToken class</span></span>

<span data-ttu-id="a9f4a-325">Tek bir nesnedeki bir veya daha fazla `IChangeToken` örneğini temsil etmek için <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-325">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="a9f4a-326">`HasChanged`, temsil edilen herhangi bir belirteç `HasChanged` `true`ise bileşik belirteç raporlarında `true`.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-326">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="a9f4a-327">`ActiveChangeCallbacks`, temsil edilen herhangi bir belirteç `ActiveChangeCallbacks` `true`ise bileşik belirteç raporlarında `true`.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-327">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="a9f4a-328">Birden çok eşzamanlı değişiklik olayı oluşursa, bileşik değişiklik geri çağırması bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a9f4a-328">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a9f4a-329">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a9f4a-329">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
