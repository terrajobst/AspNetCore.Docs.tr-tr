---
title: ASP.NET Core değişiklik belirteçleri değişikliklerle Algıla
author: guardrex
description: Değişiklik belirteçleri değişiklikleri izlemek için nasıl kullanılacağını öğrenin.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 9fd76ea1c6d746ecf1897c70ee719ea4f4517872
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729161"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="a4771-103">ASP.NET Core değişiklik belirteçleri değişikliklerle Algıla</span><span class="sxs-lookup"><span data-stu-id="a4771-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="a4771-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a4771-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a4771-105">A *belirteci değiştirme* olan değişiklikleri izlemek için kullanılan genel amaçlı, alt düzey Yapı bloğu.</span><span class="sxs-lookup"><span data-stu-id="a4771-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="a4771-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a4771-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="a4771-107">IChangeToken arabirimi</span><span class="sxs-lookup"><span data-stu-id="a4771-107">IChangeToken interface</span></span>

<span data-ttu-id="a4771-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) gerçekleşen bir değişikliği bildirimleri yayar.</span><span class="sxs-lookup"><span data-stu-id="a4771-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="a4771-109">`IChangeToken` bulunan [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) ad alanı.</span><span class="sxs-lookup"><span data-stu-id="a4771-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="a4771-110">Kullanmayan uygulamalar için [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, başvuru [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) proje dosyasında NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="a4771-110">For apps that don't use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="a4771-111">`IChangeToken` iki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a4771-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="a4771-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) belirteç proaktif olarak geri çağırmaları başlatır, belirtin.</span><span class="sxs-lookup"><span data-stu-id="a4771-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="a4771-113">Varsa `ActiveChangedCallbacks` ayarlanır `false`, bir geri çağırma hiçbir zaman olarak adlandırılır ve uygulama yoklaması gerekir `HasChanged` değişiklikleri.</span><span class="sxs-lookup"><span data-stu-id="a4771-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="a4771-114">Ayrıca, bir belirteç herhangi bir değişiklik meydana veya temel alınan değişiklik dinleyicisi atıldı ya da devre dışı hiçbir zaman iptal edilecek de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="a4771-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="a4771-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) bir değişiklik oluşup oluşmadığını belirten bir değer alır.</span><span class="sxs-lookup"><span data-stu-id="a4771-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="a4771-116">Bir yöntem arabirimi olan [RegisterChangeCallback (Eylem&lt;nesne&gt;, nesne)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), belirteç değiştiğinde çağrılan bir geri çağırma kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a4771-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="a4771-117">`HasChanged` geri çağırma çağrılmadan önce ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a4771-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="a4771-118">ChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="a4771-118">ChangeToken class</span></span>

<span data-ttu-id="a4771-119">`ChangeToken` statik sınıf gerçekleşen bir değişikliği bildirimleri yaymak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a4771-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="a4771-120">`ChangeToken` bulunan [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) ad alanı.</span><span class="sxs-lookup"><span data-stu-id="a4771-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="a4771-121">Kullanmayan uygulamalar için [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, başvuru [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) proje dosyasında NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="a4771-121">For apps that don't use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="a4771-122">`ChangeToken` [Değiştiğinde (Func&lt;IChangeToken&gt;, eylem)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) yöntemi yazmaçlar bir `Action` belirteci değiştiğinde çağırmak için:</span><span class="sxs-lookup"><span data-stu-id="a4771-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="a4771-123">`Func<IChangeToken>` belirteç oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4771-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="a4771-124">`Action` belirteç değiştiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a4771-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="a4771-125">`ChangeToken` sahip bir [değiştiğinde&lt;TState&gt;(Func&lt;IChangeToken&gt;, eylem&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) ek bir alan aşırı `TState` belirteç tüketici geçirilen parametre `Action`.</span><span class="sxs-lookup"><span data-stu-id="a4771-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="a4771-126">`OnChange` döndüren bir [IDisposable](/dotnet/api/system.idisposable).</span><span class="sxs-lookup"><span data-stu-id="a4771-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="a4771-127">Çağırma [Dispose](/dotnet/api/system.idisposable.dispose) ilerideki değişiklikler için dinleme gelen belirtecin durdurur ve belirtecin kaynakları serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="a4771-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="a4771-128">Örnek ASP.NET Core değişiklik belirteçleri kullanır</span><span class="sxs-lookup"><span data-stu-id="a4771-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="a4771-129">Değişiklik belirteçleri ASP.NET çekirdek nesnelerdeki değişiklikleri izleme belirgin alanlarında kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a4771-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="a4771-130">Dosyaları, yapılan değişiklikleri izleme için [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [izleme](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi oluşturur bir `IChangeToken` belirli dosyaları veya klasörleri izlemek için.</span><span class="sxs-lookup"><span data-stu-id="a4771-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="a4771-131">`IChangeToken` belirteçleri önbellek çıkarmaları değişiklik tetiklemek için önbellek girişlerinin eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="a4771-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="a4771-132">İçin `TOptions` değişikliği, varsayılan [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) uyarlamasını [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) bir veya daha fazla kabul eden bir aşırı sahip [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)örnekleri.</span><span class="sxs-lookup"><span data-stu-id="a4771-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="a4771-133">Her oluşumu döndüren bir `IChangeToken` değişiklikleri izleme seçenekleri için değişiklik bildirimi geri araması kaydetme.</span><span class="sxs-lookup"><span data-stu-id="a4771-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="a4771-134">Yapılandırma değişikliklerini izleme</span><span class="sxs-lookup"><span data-stu-id="a4771-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="a4771-135">Varsayılan olarak, ASP.NET Core şablonlarını kullanma [JSON yapılandırma dosyalarını](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings. Development.JSON*, ve *appsettings. Production.JSON*) uygulama yapılandırma ayarlarını yüklenemiyor.</span><span class="sxs-lookup"><span data-stu-id="a4771-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="a4771-136">Bu dosyaları kullanılarak yapılandırılmış olan [AddJsonFile (IConfigurationBuilder, dize, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) genişletme yöntemi [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) kabul eden bir `reloadOnChange` parametresi (ASP.NET Çekirdek 1.1 ve üzeri).</span><span class="sxs-lookup"><span data-stu-id="a4771-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="a4771-137">`reloadOnChange` yapılandırma dosyası değişiklikleri yeniden olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4771-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="a4771-138">Bu ayarda bkz [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) kolaylık yöntemi [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span><span class="sxs-lookup"><span data-stu-id="a4771-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="a4771-139">Dosya tabanlı yapılandırma ile temsil edilir [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span><span class="sxs-lookup"><span data-stu-id="a4771-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="a4771-140">`FileConfigurationSource` kullanan [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) dosyaları izlemek için.</span><span class="sxs-lookup"><span data-stu-id="a4771-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="a4771-141">Varsayılan olarak, `IFileMonitor` tarafından sağlanan bir [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), kullanan [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) için yapılandırma dosyası değişiklikleri izlemek için.</span><span class="sxs-lookup"><span data-stu-id="a4771-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="a4771-142">Örnek uygulamayı yapılandırma değişikliklerini izleme için iki uygulamaları gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4771-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="a4771-143">Her iki *appsettings.json* dosya değişiklikleri veya dosyanın ortamı sürümü değişiklikler, her uygulama özel kodu yürütür.</span><span class="sxs-lookup"><span data-stu-id="a4771-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="a4771-144">Örnek uygulaması bir ileti konsola yazar.</span><span class="sxs-lookup"><span data-stu-id="a4771-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="a4771-145">Bir yapılandırma dosyasının `FileSystemWatcher` tek yapılandırma dosyası değişikliği için birden çok belirteç geri aramalar tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="a4771-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="a4771-146">Örnek 's uygulama yapılandırma dosyaları dosya karmalarını denetleyerek bu soruna karşı korur.</span><span class="sxs-lookup"><span data-stu-id="a4771-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="a4771-147">Dosya karmalarını denetimi yapılandırma dosyalarını en az biri özel kod çalıştırmadan önce değişti sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4771-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="a4771-148">Örnek dosya SHA1 karma kullanır (*Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="a4771-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="a4771-149">Yeniden deneme bir üstel geri alma ile uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a4771-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="a4771-150">Dosya kilitleme dosyalardan birinde yeni bir karma hesaplama geçici olarak engelleyen oluşabilir olduğundan yeniden deneyin mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="a4771-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="a4771-151">Basit başlangıç değişiklik simgesi</span><span class="sxs-lookup"><span data-stu-id="a4771-151">Simple startup change token</span></span>

<span data-ttu-id="a4771-152">Bir belirteç tüketici kaydetmek `Action` için geri çağırma değişiklik bildirimleri yapılandırma yeniden yükleme belirtece (*haline*):</span><span class="sxs-lookup"><span data-stu-id="a4771-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="a4771-153">`config.GetReloadToken()` belirteç sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4771-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="a4771-154">Geri çağırma olduğu `InvokeChanged` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a4771-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="a4771-155">`state` Geri çağırma içinde geçirmek için kullanılan `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="a4771-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="a4771-156">Bu doğru belirlemek kullanışlıdır *appsettings* izlemek için yapılandırma JSON dosyasını *appsettings.&lt; Ortam&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="a4771-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="a4771-157">Dosya karmalarını önlemek için kullanılan `WriteConsole` yapılandırma dosyasının yalnızca bir kez değiştiğinde birden çok kez birden çok belirteç geri aramalar nedeniyle çalışmasını deyimi.</span><span class="sxs-lookup"><span data-stu-id="a4771-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="a4771-158">Bu sistem uygulaması çalıştıran ve kullanıcı tarafından devre dışı bırakılamaz sürece çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="a4771-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="a4771-159">Bir hizmet olarak yapılandırma değişikliklerini izleme</span><span class="sxs-lookup"><span data-stu-id="a4771-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="a4771-160">Örnek uygular:</span><span class="sxs-lookup"><span data-stu-id="a4771-160">The sample implements:</span></span>

* <span data-ttu-id="a4771-161">Temel başlangıç belirteci izleme.</span><span class="sxs-lookup"><span data-stu-id="a4771-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="a4771-162">Bir hizmet olarak izleme.</span><span class="sxs-lookup"><span data-stu-id="a4771-162">Monitoring as a service.</span></span>
* <span data-ttu-id="a4771-163">Etkinleştirip izleme devre dışı bırakmak için bir mekanizma.</span><span class="sxs-lookup"><span data-stu-id="a4771-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="a4771-164">Örnek kurar bir `IConfigurationMonitor` arabirimi (*Extensions/ConfigurationMonitor.cs*):</span><span class="sxs-lookup"><span data-stu-id="a4771-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="a4771-165">Uygulanan sınıf `ConfigurationMonitor`, değişiklik bildirimleri için bir geri çağırma kaydeder:</span><span class="sxs-lookup"><span data-stu-id="a4771-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="a4771-166">`config.GetReloadToken()` belirteç sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4771-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="a4771-167">`InvokeChanged` geri çağırma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="a4771-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="a4771-168">`state` Bu örneğinde başvurusudur `IConfigurationMonitor` izleme durumu erişmek için kullanılan örnek.</span><span class="sxs-lookup"><span data-stu-id="a4771-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that is used to access the monitoring state.</span></span> <span data-ttu-id="a4771-169">İki özellik kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a4771-169">Two properties are used:</span></span>

* <span data-ttu-id="a4771-170">`MonitoringEnabled` geri arama özel kod çalışması gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4771-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="a4771-171">`CurrentState` kullanıcı arabirimini kullanmak için geçerli izleme durumu açıklar.</span><span class="sxs-lookup"><span data-stu-id="a4771-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="a4771-172">`InvokeChanged` Yöntemi, önceki yaklaşım, BT'nin dışında benzerdir:</span><span class="sxs-lookup"><span data-stu-id="a4771-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="a4771-173">Kendi kod sürece çalışmaz `MonitoringEnabled` olan `true`.</span><span class="sxs-lookup"><span data-stu-id="a4771-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="a4771-174">Geçerli Notlar `state` içinde kendi `WriteConsole` çıktı.</span><span class="sxs-lookup"><span data-stu-id="a4771-174">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="a4771-175">Bir örneği `ConfigurationMonitor` bir hizmet olarak kayıtlı `ConfigureServices` , *haline*:</span><span class="sxs-lookup"><span data-stu-id="a4771-175">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="a4771-176">Dizin Sayfası yapılandırma izleme üzerinden kullanıcı denetimi sunar.</span><span class="sxs-lookup"><span data-stu-id="a4771-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="a4771-177">Örneğinin `IConfigurationMonitor` içine eklenen `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="a4771-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a4771-178">Bir düğme sağlar ve izlemeyi devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="a4771-178">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="a4771-179">Zaman `OnPostStartMonitoring` olan tetiklenen, izleme etkinleştirilmişse ve geçerli durumu temizlenir.</span><span class="sxs-lookup"><span data-stu-id="a4771-179">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="a4771-180">Zaman `OnPostStopMonitoring` olan tetiklenen, izleme devre dışı bırakılır ve durumunu izleme gerçekleşen değil yansıtacak şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a4771-180">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="a4771-181">Önbelleğe alınmış dosya değişiklikleri izleme</span><span class="sxs-lookup"><span data-stu-id="a4771-181">Monitoring cached file changes</span></span>

<span data-ttu-id="a4771-182">Dosya içeriği, önbelleğe alınan bellek içi kullanarak olabilir [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span><span class="sxs-lookup"><span data-stu-id="a4771-182">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="a4771-183">Bellek içi önbelleğe alma açıklanan [bellek içi önbellek](xref:performance/caching/memory) konu.</span><span class="sxs-lookup"><span data-stu-id="a4771-183">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="a4771-184">Uygulama, aşağıda açıklandığı gibi ek adımlar alma olmadan *eski* kaynak veriler değişirse (güncel olmayan) verileri bir önbellekten döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a4771-184">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="a4771-185">Hesaba bir önbelleğe alınan kaynak dosyası durumunu yenilenirken katarak olmayan bir [kayan bitiş tarihinin](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) önbellek verilerini eski dönemi müşteri adayları.</span><span class="sxs-lookup"><span data-stu-id="a4771-185">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="a4771-186">Kayan sona erme süresi her istek için verileri yeniler, ancak dosyası hiçbir zaman önbelleğe yeniden.</span><span class="sxs-lookup"><span data-stu-id="a4771-186">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="a4771-187">Dosyanın önbelleğe alınmış içeriği kullanan tüm uygulama büyük olasılıkla eski içerik alma tabi özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="a4771-187">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="a4771-188">Senaryo önbelleğe alma bir dosyada değişiklik belirteçleri kullanarak eski dosya içeriği önbelleğinde engeller.</span><span class="sxs-lookup"><span data-stu-id="a4771-188">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="a4771-189">Örnek uygulaması, bir uygulama yaklaşımı ortaya koyar.</span><span class="sxs-lookup"><span data-stu-id="a4771-189">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="a4771-190">Örnek kullanır `GetFileContent` için:</span><span class="sxs-lookup"><span data-stu-id="a4771-190">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="a4771-191">Dosya içeriğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="a4771-191">Return file content.</span></span>
* <span data-ttu-id="a4771-192">Üstel geri alma burada bir dosya kilidi geçici olarak bir dosya okunmaya karşı engelliyor kapak çalışmaları için bir yeniden deneme algoritmasıyla uygulayın.</span><span class="sxs-lookup"><span data-stu-id="a4771-192">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="a4771-193">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="a4771-193">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="a4771-194">A `FileService` önbelleğe alınmış dosya aramalarını işlemek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a4771-194">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="a4771-195">`GetFileContent` Yöntem çağrısı hizmetinin çalışır bellek içi önbellekten dosya içeriği almak ve çağırana dönmek (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="a4771-195">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="a4771-196">Önbelleğe alınmış içeriği önbellek anahtarını kullanarak bulunamazsa, şu işlemler uygulanır:</span><span class="sxs-lookup"><span data-stu-id="a4771-196">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="a4771-197">Dosya içeriği kullanılarak elde edilen `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="a4771-197">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="a4771-198">Bir değişiklik belirteci ile dosya sağlayıcısından alınan [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span><span class="sxs-lookup"><span data-stu-id="a4771-198">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="a4771-199">Dosya değiştirildiğinde belirtecin geri çağırma tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="a4771-199">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="a4771-200">Dosya içeriği ile önbelleğe alınmış bir [kayan bitiş tarihinin](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) süresi.</span><span class="sxs-lookup"><span data-stu-id="a4771-200">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="a4771-201">Değişiklik simgesi ile bağlı [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) önbelleğe sırada dosyası değişirse önbellek girişi çıkarmak için.</span><span class="sxs-lookup"><span data-stu-id="a4771-201">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="a4771-202">`FileService` Hizmet önbelleğe alma bellek birlikte hizmet kapsayıcısında kayıtlı (*haline*):</span><span class="sxs-lookup"><span data-stu-id="a4771-202">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="a4771-203">Hizmetini kullanarak dosyanın içeriğini sayfa modelini yükler (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a4771-203">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="a4771-204">CompositeChangeToken sınıfı</span><span class="sxs-lookup"><span data-stu-id="a4771-204">CompositeChangeToken class</span></span>

<span data-ttu-id="a4771-205">Bir veya daha fazla temsil etmek için `IChangeToken` tek bir nesne durumlarda kullanmak [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a4771-205">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

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

<span data-ttu-id="a4771-206">`HasChanged` Birleşik belirteci raporlarda `true` herhangi bir belirteç temsil varsa `HasChanged` olan `true`.</span><span class="sxs-lookup"><span data-stu-id="a4771-206">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="a4771-207">`ActiveChangeCallbacks` Birleşik belirteci raporlarda `true` herhangi bir belirteç temsil varsa `ActiveChangeCallbacks` olan `true`.</span><span class="sxs-lookup"><span data-stu-id="a4771-207">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="a4771-208">Birden çok eşzamanlı değişikliği olayları meydana gelirse, bileşik değişikliği geri çağırma tam olarak bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a4771-208">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4771-209">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a4771-209">Additional resources</span></span>

* [<span data-ttu-id="a4771-210">Belleğe yüklenmiş önbellek</span><span class="sxs-lookup"><span data-stu-id="a4771-210">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="a4771-211">Dağıtılmış önbellekle çalışma</span><span class="sxs-lookup"><span data-stu-id="a4771-211">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="a4771-212">Yanıtları önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="a4771-212">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="a4771-213">Yanıtları Önbelleğe Alma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="a4771-213">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="a4771-214">Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a4771-214">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="a4771-215">Dağıtılmış Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="a4771-215">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
