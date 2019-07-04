---
title: ASP.NET Core değişiklik belirteçleri ile değişiklikleri algılama
author: guardrex
description: Değişiklikleri izlemek için değişiklik belirteçleri kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/03/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: 8b73b72d093b33edeb91bc78080e05aa312579ec
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561656"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="5c36c-103">ASP.NET Core değişiklik belirteçleri ile değişiklikleri algılama</span><span class="sxs-lookup"><span data-stu-id="5c36c-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="5c36c-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5c36c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5c36c-105">A *belirteç değiştirme* bir genel amaçlı, alt düzey oluşturma durumu değişiklikleri izlemek için kullanılan blok.</span><span class="sxs-lookup"><span data-stu-id="5c36c-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="5c36c-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5c36c-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="5c36c-107">IChangeToken arabirimi</span><span class="sxs-lookup"><span data-stu-id="5c36c-107">IChangeToken interface</span></span>

<span data-ttu-id="5c36c-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> gerçekleşen bir değişikliği bildirimleri yayar.</span><span class="sxs-lookup"><span data-stu-id="5c36c-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="5c36c-109">`IChangeToken` bulunan <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="5c36c-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="5c36c-110">Kullanmayan uygulamalar için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), bir paket başvurusu için [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="5c36c-110">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="5c36c-111">`IChangeToken` iki özelliğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5c36c-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="5c36c-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> belirteç proaktif bir şekilde geri çağırmaları harekete geçirirse gösterir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="5c36c-113">Varsa `ActiveChangedCallbacks` ayarlanır `false`, bir geri çağırma hiçbir zaman çağrılır ve uygulama yoklama yapmalıdır `HasChanged` değişiklikler.</span><span class="sxs-lookup"><span data-stu-id="5c36c-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="5c36c-114">Ayrıca, herhangi bir değişiklik meydana veya temel alınan değişiklik dinleyicisi elden ya da devre dışı hiçbir zaman iptal edilmesi için bir belirteç de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5c36c-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="5c36c-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişiklik meydana geldiğini gösteren bir değer alır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="5c36c-116">`IChangeToken` Arabirimi içeren [RegisterChangeCallback (Eylem\<Nesne >, nesne)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) yöntemi belirteç değiştiğinde çağrılan bir geri çağırma kaydeder.</span><span class="sxs-lookup"><span data-stu-id="5c36c-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="5c36c-117">`HasChanged` geri çağırma çağrılmadan önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="5c36c-118">ChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="5c36c-118">ChangeToken class</span></span>

<span data-ttu-id="5c36c-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> statik sınıf gerçekleşen bir değişikliği bildirimleri yaymak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="5c36c-120">`ChangeToken` bulunan <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="5c36c-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="5c36c-121">Kullanmayan uygulamalar için [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), bir paket başvurusu için [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="5c36c-121">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="5c36c-122">[ChangeToken.OnChange (Func\<IChangeToken >, eylem)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) yöntemi kayıtları bir `Action` belirteç değiştiğinde çağrılacak:</span><span class="sxs-lookup"><span data-stu-id="5c36c-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="5c36c-123">`Func<IChangeToken>` belirteci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5c36c-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="5c36c-124">`Action` belirteç değiştiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="5c36c-125">[ChangeToken.OnChange\<TState > (Func\<IChangeToken >, eylem\<TState >, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) aşırı ek bir alan `TState` belirtece geçirilen parametre Tüketici `Action`.</span><span class="sxs-lookup"><span data-stu-id="5c36c-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="5c36c-126">`OnChange` döndürür bir <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="5c36c-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="5c36c-127">Çağırma <xref:System.IDisposable.Dispose*> ilerideki değişiklikler için dinleme gelen belirteç durdurur ve belirtecin kaynakları serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="5c36c-128">Örnek ASP.NET Core değişiklik belirteçler kullanır</span><span class="sxs-lookup"><span data-stu-id="5c36c-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="5c36c-129">Değişiklik belirteçleri, ASP.NET Core tanınmış alanlarında nesnelere değişiklikleri izlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5c36c-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="5c36c-130">Dosya değişiklikleri izleme <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> yöntemi oluşturur bir `IChangeToken` belirtilen dosyaların veya klasörlerin izlemek için.</span><span class="sxs-lookup"><span data-stu-id="5c36c-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="5c36c-131">`IChangeToken` belirteçleri önbelleğe çıkarmaları değişiklik tetiklemek için önbellek girişlerinin eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="5c36c-132">İçin `TOptions` değiştirir, varsayılan <xref:Microsoft.Extensions.Options.OptionsMonitor`1> uygulaması <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> bir veya daha fazla kabul eden aşırı yüklenmiş <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> örnekleri.</span><span class="sxs-lookup"><span data-stu-id="5c36c-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="5c36c-133">Her bir örnek döndürür bir `IChangeToken` değişiklikleri izleme seçenekleri için değişiklik bildirimi geri araması kaydetme.</span><span class="sxs-lookup"><span data-stu-id="5c36c-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="5c36c-134">Yapılandırma değişiklikleri izleyin</span><span class="sxs-lookup"><span data-stu-id="5c36c-134">Monitor for configuration changes</span></span>

<span data-ttu-id="5c36c-135">Varsayılan olarak, ASP.NET Core şablonları kullanın [JSON yapılandırma dosyaları](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings. Development.JSON*, ve *appsettings. Production.JSON*) uygulama yapılandırma ayarları yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="5c36c-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="5c36c-136">Bu dosya kullanılarak yapılandırılır [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> kabul eden bir `reloadOnChange` parametresi.</span><span class="sxs-lookup"><span data-stu-id="5c36c-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="5c36c-137">`reloadOnChange` yapılandırma dosya değişikliklerinde yüklenmesi durumunda gösterir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="5c36c-138">Bu ayar görünür <xref:Microsoft.AspNetCore.WebHost> kolaylık yöntemi <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="5c36c-138">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="5c36c-139">Dosya tabanlı yapılandırma tarafından temsil edilen <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="5c36c-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="5c36c-140">`FileConfigurationSource` kullanan <xref:Microsoft.Extensions.FileProviders.IFileProvider> dosyaları izlemek için.</span><span class="sxs-lookup"><span data-stu-id="5c36c-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="5c36c-141">Varsayılan olarak, `IFileMonitor` tarafından sağlanan bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, kullanan <xref:System.IO.FileSystemWatcher> yapılandırma dosya değişiklikleri izlemek için.</span><span class="sxs-lookup"><span data-stu-id="5c36c-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="5c36c-142">Örnek uygulamayı yapılandırma değişiklikleri izlemek için iki uygulamaları gösterir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="5c36c-143">Varsa *appsettings* dosyaları değiştirmek, uygulamaları izleme dosyasının hem de özel kod yürütme&mdash;örnek uygulama bir ileti konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="5c36c-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="5c36c-144">Bir yapılandırma dosyasının `FileSystemWatcher` tek bir yapılandırma dosyası değişikliği için birden fazla belirteç geri çağırmaları tetikleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5c36c-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="5c36c-145">Birden çok belirteç geri çağırmaları harekete geçirildiğinde sonra özel kod yalnızca çalıştığından emin olmak için dosya karmaları örnek'ın uygulama denetler.</span><span class="sxs-lookup"><span data-stu-id="5c36c-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="5c36c-146">Örnek dosya SHA1 karma kullanır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="5c36c-147">Bir üstel geri alma ile yeniden uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="5c36c-148">Yeniden deneme mevcut olduğundan dosya kilitleme geçici olarak bir dosya çubuğunda yeni karma hesaplaması engelleyen ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="5c36c-149">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="5c36c-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="5c36c-150">Basit başlangıç belirteci Değiştir</span><span class="sxs-lookup"><span data-stu-id="5c36c-150">Simple startup change token</span></span>

<span data-ttu-id="5c36c-151">Bir belirteç tüketici kaydetme `Action` geri çağırma için değişiklik bildirimleri yapılandırma yeniden yükleme belirteci.</span><span class="sxs-lookup"><span data-stu-id="5c36c-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="5c36c-152">İçinde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5c36c-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="5c36c-153">`config.GetReloadToken()` belirteç sağlar.</span><span class="sxs-lookup"><span data-stu-id="5c36c-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="5c36c-154">Aramasıdır `InvokeChanged` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5c36c-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="5c36c-155">`state` Geri çağırma içinde iletmek için kullanılan `IHostingEnvironment`, doğru olarak belirtmek için yararlı olan *appsettings* izlemek için yapılandırma dosyası (örneğin, *appsettings. Development.JSON* zaman geliştirme ortamında).</span><span class="sxs-lookup"><span data-stu-id="5c36c-155">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="5c36c-156">Dosya karmalarını önlemek için kullanılan `WriteConsole` yapılandırma dosyasının yalnızca bir kez değiştirdiği zaman birden çok kez birden fazla belirteç geri çağırmaları nedeniyle çalışmasını deyimi.</span><span class="sxs-lookup"><span data-stu-id="5c36c-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="5c36c-157">Bu sistem, uygulamayı çalıştıran ve kullanıcı tarafından devre dışı bırakılamaz sürece çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="5c36c-158">Hizmet olarak yapılandırma değişikliklerini izleyin</span><span class="sxs-lookup"><span data-stu-id="5c36c-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="5c36c-159">Örnek uygular:</span><span class="sxs-lookup"><span data-stu-id="5c36c-159">The sample implements:</span></span>

* <span data-ttu-id="5c36c-160">Temel başlangıç belirteci izleme.</span><span class="sxs-lookup"><span data-stu-id="5c36c-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="5c36c-161">Hizmet olarak izleme.</span><span class="sxs-lookup"><span data-stu-id="5c36c-161">Monitoring as a service.</span></span>
* <span data-ttu-id="5c36c-162">Enable ve disable izleme için bir mekanizma.</span><span class="sxs-lookup"><span data-stu-id="5c36c-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="5c36c-163">Örnek kurar bir `IConfigurationMonitor` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="5c36c-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="5c36c-164">*Extensions/ConfigurationMonitor.cs*:</span><span class="sxs-lookup"><span data-stu-id="5c36c-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="5c36c-165">Uygulanan sınıfının oluşturucusu, `ConfigurationMonitor`, değişiklik bildirimlerinin bir geri çağırma kaydeder:</span><span class="sxs-lookup"><span data-stu-id="5c36c-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="5c36c-166">`config.GetReloadToken()` belirteç sağlar.</span><span class="sxs-lookup"><span data-stu-id="5c36c-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="5c36c-167">`InvokeChanged` geri çağırma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="5c36c-168">`state` Bu örnekte bir başvurudur `IConfigurationMonitor` izleme durumuna erişmek için kullanılan bir örnek.</span><span class="sxs-lookup"><span data-stu-id="5c36c-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="5c36c-169">İki özellikleri kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5c36c-169">Two properties are used:</span></span>

* <span data-ttu-id="5c36c-170">`MonitoringEnabled` &ndash; geri çağırma kendi özel kod çalışması gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="5c36c-171">`CurrentState` &ndash; kullanıcı arabirimini kullanmak için geçerli izleme durumunu açıklar.</span><span class="sxs-lookup"><span data-stu-id="5c36c-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="5c36c-172">`InvokeChanged` Yöntemi, önceki yaklaşımı, BT'nin dışında benzerdir:</span><span class="sxs-lookup"><span data-stu-id="5c36c-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="5c36c-173">Kendi kod sürece çalışmaz `MonitoringEnabled` olduğu `true`.</span><span class="sxs-lookup"><span data-stu-id="5c36c-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="5c36c-174">Geçerli çıkarır `state` içinde kendi `WriteConsole` çıktı.</span><span class="sxs-lookup"><span data-stu-id="5c36c-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="5c36c-175">Bir örneği `ConfigurationMonitor` bir hizmet olarak kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5c36c-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="5c36c-176">Dizin Sayfası izleme yapılandırması üzerinde kullanıcı denetimi sunar.</span><span class="sxs-lookup"><span data-stu-id="5c36c-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="5c36c-177">Örneğini `IConfigurationMonitor` içine eklenen `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="5c36c-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="5c36c-178">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5c36c-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="5c36c-179">Yapılandırma İzleyicisi (`_monitor`) etkinleştirmek veya izlemeyi devre dışı bırakın ve UI geri bildirim için geçerli durumunu ayarlamak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5c36c-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="5c36c-180">Zaman `OnPostStartMonitoring` olan tetiklenen, izleme etkin olduğundan ve geçerli durum temizlenir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="5c36c-181">Zaman `OnPostStopMonitoring` olan tetiklenen, izleme devre dışıdır ve durum izleme gerçekleşen olmayan yansıtacak şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="5c36c-182">Kullanıcı arabiriminde düğmeleri etkinleştirin ve izlemeyi devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="5c36c-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="5c36c-183">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5c36c-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="5c36c-184">Önbelleğe alınan dosya değişikliklerini izleme</span><span class="sxs-lookup"><span data-stu-id="5c36c-184">Monitor cached file changes</span></span>

<span data-ttu-id="5c36c-185">Dosya içeriği, önbelleğe alınan bellek içi kullanarak olabilir <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="5c36c-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="5c36c-186">Bellek içi önbelleğe alma açıklanan [bellek içi önbelleğe alma](xref:performance/caching/memory) konu.</span><span class="sxs-lookup"><span data-stu-id="5c36c-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="5c36c-187">Uygulama, aşağıda açıklandığı gibi ek adımları almadan *eski* (eski) veri kaynak veriler değiştiğinde olursa bir önbellekten döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5c36c-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="5c36c-188">Örneğin, hesaba önbelleğe alınmış kaynak dosyası durumunu yenilenirken katılarak değil bir [kayan zaman aşımı](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süre eski önbelleğe alınmış dosya verileri için yol açar.</span><span class="sxs-lookup"><span data-stu-id="5c36c-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="5c36c-189">Kayan zaman aşımı süresi her istek için verileri yeniler, ancak dosya hiçbir zaman önbelleğine yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="5c36c-190">Önbelleğe alınan dosyanın içeriğini kullanan herhangi bir uygulama özelliği, büyük olasılıkla eski bir içerik alma tabi ' dir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="5c36c-191">Senaryo önbelleğe alma bir dosyada değişiklik belirteçleri kullanarak eski dosya içeriği önbelleğinde varlığını engeller.</span><span class="sxs-lookup"><span data-stu-id="5c36c-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="5c36c-192">Örnek uygulamayı yaklaşımın bir uygulamasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="5c36c-193">Örnek kullanır `GetFileContent` için:</span><span class="sxs-lookup"><span data-stu-id="5c36c-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="5c36c-194">Dosya içeriğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="5c36c-194">Return file content.</span></span>
* <span data-ttu-id="5c36c-195">Bir üstel geri alma burada dosya kilit geçici olarak bir dosyayı okuma engeller kapak çalışmaları için yeniden deneme algoritmasıyla uygulayın.</span><span class="sxs-lookup"><span data-stu-id="5c36c-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="5c36c-196">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="5c36c-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="5c36c-197">A `FileService` önbelleğe alınmış dosya aramaları işlenecek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5c36c-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="5c36c-198">`GetFileContent` Yöntem çağrısının hizmetin çalışır çağırana döndürmesi ve bellek içi Önbelleği'ndeki dosya içeriğini almak (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="5c36c-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="5c36c-199">Önbellek anahtarını kullanarak önbelleğe alınmış içerikleri bulunamazsa, şu işlemler uygulanır:</span><span class="sxs-lookup"><span data-stu-id="5c36c-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="5c36c-200">Dosya içeriğini kullanarak elde edilen `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="5c36c-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="5c36c-201">İle dosya sağlayıcısından alınan bir değişiklik belirteci [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="5c36c-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="5c36c-202">Belirtecin geri çağırma dosya değiştirildiğinde tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="5c36c-203">Dosya içeriği önbelleğe alınmış bir [kayan zaman aşımı](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) süresi.</span><span class="sxs-lookup"><span data-stu-id="5c36c-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="5c36c-204">Belirteci Değiştir iliştirilir [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) önbelleğe alınır ancak dosyayı değiştirirse, önbellek girişi çıkarmak için.</span><span class="sxs-lookup"><span data-stu-id="5c36c-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="5c36c-205">Aşağıdaki örnekte, dosyaları uygulamanın içerik kök dizininde depolanır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-205">In the following example, files are stored in the app's content root.</span></span> <span data-ttu-id="5c36c-206">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) elde etmek için kullanılan bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> uygulamanın işaret eden <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span><span class="sxs-lookup"><span data-stu-id="5c36c-206">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="5c36c-207">`filePath` İle elde edilen [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="5c36c-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="5c36c-208">`FileService` Hizmet kapsayıcı hizmeti önbelleğe alma bellek birlikte kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5c36c-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="5c36c-209">İçinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5c36c-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="5c36c-210">Sayfa modeli hizmetini kullanarak dosyanın içeriğini yükler.</span><span class="sxs-lookup"><span data-stu-id="5c36c-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="5c36c-211">Dizin sayfanın içinde `OnGet` yöntemi (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5c36c-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="5c36c-212">CompositeChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="5c36c-212">CompositeChangeToken class</span></span>

<span data-ttu-id="5c36c-213">Bir veya daha fazla temsil eden `IChangeToken` tek bir nesne örneğini kullanın <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5c36c-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

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

<span data-ttu-id="5c36c-214">`HasChanged` Birleşik belirteci raporlarda `true` herhangi bir belirteci temsil, `HasChanged` olduğu `true`.</span><span class="sxs-lookup"><span data-stu-id="5c36c-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="5c36c-215">`ActiveChangeCallbacks` Birleşik belirteci raporlarda `true` herhangi bir belirteci temsil, `ActiveChangeCallbacks` olduğu `true`.</span><span class="sxs-lookup"><span data-stu-id="5c36c-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="5c36c-216">Birden çok eş zamanlı değişikliği olayları meydana gelirse, bileşik bir değişikliği geri çağırma bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5c36c-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c36c-217">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5c36c-217">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
