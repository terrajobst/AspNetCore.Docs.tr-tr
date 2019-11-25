---
title: Options pattern in ASP.NET Core
author: guardrex
description: Discover how to use the options pattern to represent groups of related settings in ASP.NET Core apps.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/18/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 4192bab8acef7c4f7bdf1ac481c468cd0a835420
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239792"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="770e7-103">Options pattern in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="770e7-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="770e7-104">By [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="770e7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="770e7-105">The options pattern uses classes to represent groups of related settings.</span><span class="sxs-lookup"><span data-stu-id="770e7-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="770e7-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span><span class="sxs-lookup"><span data-stu-id="770e7-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="770e7-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span><span class="sxs-lookup"><span data-stu-id="770e7-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="770e7-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span><span class="sxs-lookup"><span data-stu-id="770e7-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="770e7-109">Options also provide a mechanism to validate configuration data.</span><span class="sxs-lookup"><span data-stu-id="770e7-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="770e7-110">For more information, see the [Options validation](#options-validation) section.</span><span class="sxs-lookup"><span data-stu-id="770e7-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="770e7-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="770e7-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="770e7-112">Paket</span><span class="sxs-lookup"><span data-stu-id="770e7-112">Package</span></span>

<span data-ttu-id="770e7-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span><span class="sxs-lookup"><span data-stu-id="770e7-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="770e7-114">Options interfaces</span><span class="sxs-lookup"><span data-stu-id="770e7-114">Options interfaces</span></span>

<span data-ttu-id="770e7-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="770e7-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span><span class="sxs-lookup"><span data-stu-id="770e7-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="770e7-117">Change notifications</span><span class="sxs-lookup"><span data-stu-id="770e7-117">Change notifications</span></span>
* [<span data-ttu-id="770e7-118">Named options</span><span class="sxs-lookup"><span data-stu-id="770e7-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="770e7-119">Reloadable configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="770e7-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="770e7-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="770e7-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span><span class="sxs-lookup"><span data-stu-id="770e7-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="770e7-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="770e7-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span><span class="sxs-lookup"><span data-stu-id="770e7-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="770e7-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span><span class="sxs-lookup"><span data-stu-id="770e7-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="770e7-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span><span class="sxs-lookup"><span data-stu-id="770e7-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="770e7-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="770e7-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="770e7-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="770e7-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="770e7-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="770e7-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span><span class="sxs-lookup"><span data-stu-id="770e7-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="770e7-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span><span class="sxs-lookup"><span data-stu-id="770e7-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="770e7-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span><span class="sxs-lookup"><span data-stu-id="770e7-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="770e7-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span><span class="sxs-lookup"><span data-stu-id="770e7-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="770e7-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="770e7-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="770e7-135">General options configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-135">General options configuration</span></span>

<span data-ttu-id="770e7-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="770e7-137">An options class must be non-abstract with a public parameterless constructor.</span><span class="sxs-lookup"><span data-stu-id="770e7-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="770e7-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span><span class="sxs-lookup"><span data-stu-id="770e7-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="770e7-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span><span class="sxs-lookup"><span data-stu-id="770e7-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="770e7-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="770e7-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span><span class="sxs-lookup"><span data-stu-id="770e7-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="770e7-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="770e7-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span><span class="sxs-lookup"><span data-stu-id="770e7-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="770e7-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="770e7-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span><span class="sxs-lookup"><span data-stu-id="770e7-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="770e7-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="770e7-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="770e7-147">Configure simple options with a delegate</span><span class="sxs-lookup"><span data-stu-id="770e7-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="770e7-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="770e7-149">Use a delegate to set options values.</span><span class="sxs-lookup"><span data-stu-id="770e7-149">Use a delegate to set options values.</span></span> <span data-ttu-id="770e7-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="770e7-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="770e7-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="770e7-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="770e7-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="770e7-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="770e7-154">You can add multiple configuration providers.</span><span class="sxs-lookup"><span data-stu-id="770e7-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="770e7-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span><span class="sxs-lookup"><span data-stu-id="770e7-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="770e7-156">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="770e7-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="770e7-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="770e7-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span><span class="sxs-lookup"><span data-stu-id="770e7-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="770e7-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span><span class="sxs-lookup"><span data-stu-id="770e7-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="770e7-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="770e7-161">Suboptions configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-161">Suboptions configuration</span></span>

<span data-ttu-id="770e7-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="770e7-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span><span class="sxs-lookup"><span data-stu-id="770e7-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="770e7-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span><span class="sxs-lookup"><span data-stu-id="770e7-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="770e7-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="770e7-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="770e7-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="770e7-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="770e7-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="770e7-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="770e7-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="770e7-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span><span class="sxs-lookup"><span data-stu-id="770e7-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="770e7-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="770e7-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="770e7-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="770e7-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="770e7-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="770e7-174">Options injection</span><span class="sxs-lookup"><span data-stu-id="770e7-174">Options injection</span></span>

<span data-ttu-id="770e7-175">Options injection is demonstrated as Example &num;4 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-175">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="770e7-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span><span class="sxs-lookup"><span data-stu-id="770e7-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="770e7-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span><span class="sxs-lookup"><span data-stu-id="770e7-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="770e7-178">A page or view model.</span><span class="sxs-lookup"><span data-stu-id="770e7-178">A page or view model.</span></span>

<span data-ttu-id="770e7-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="770e7-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span><span class="sxs-lookup"><span data-stu-id="770e7-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="770e7-181">When the app is run, the options values are shown in the rendered page:</span><span class="sxs-lookup"><span data-stu-id="770e7-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Options values Option1: value1_from_json and Option2: -1 are loaded from the model and by injection into the view.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="770e7-183">Reload configuration data with IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="770e7-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="770e7-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="770e7-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span><span class="sxs-lookup"><span data-stu-id="770e7-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="770e7-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span><span class="sxs-lookup"><span data-stu-id="770e7-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="770e7-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span><span class="sxs-lookup"><span data-stu-id="770e7-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="770e7-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span><span class="sxs-lookup"><span data-stu-id="770e7-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="770e7-189">Options snapshots are designed for use with transient and scoped dependencies.</span><span class="sxs-lookup"><span data-stu-id="770e7-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="770e7-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="770e7-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="770e7-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span><span class="sxs-lookup"><span data-stu-id="770e7-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="770e7-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="770e7-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="770e7-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span><span class="sxs-lookup"><span data-stu-id="770e7-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="770e7-194">Save the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="770e7-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="770e7-195">Refresh the browser to see that the options values are updated:</span><span class="sxs-lookup"><span data-stu-id="770e7-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="770e7-196">Named options support with IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="770e7-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="770e7-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="770e7-198">*Named options* support allows the app to distinguish between named options configurations.</span><span class="sxs-lookup"><span data-stu-id="770e7-198">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="770e7-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span><span class="sxs-lookup"><span data-stu-id="770e7-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="770e7-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="770e7-201">Running the sample app, the named options are returned:</span><span class="sxs-lookup"><span data-stu-id="770e7-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="770e7-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="770e7-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="770e7-203">`named_options_2` values are provided by:</span><span class="sxs-lookup"><span data-stu-id="770e7-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="770e7-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span><span class="sxs-lookup"><span data-stu-id="770e7-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="770e7-205">The default value for `Option2` provided by the `MyOptions` class.</span><span class="sxs-lookup"><span data-stu-id="770e7-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="770e7-206">Configure all options with the ConfigureAll method</span><span class="sxs-lookup"><span data-stu-id="770e7-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="770e7-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span><span class="sxs-lookup"><span data-stu-id="770e7-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="770e7-208">The following code configures `Option1` for all configuration instances with a common value.</span><span class="sxs-lookup"><span data-stu-id="770e7-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="770e7-209">Add the following code manually to the `Startup.ConfigureServices` method:</span><span class="sxs-lookup"><span data-stu-id="770e7-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="770e7-210">Running the sample app after adding the code produces the following result:</span><span class="sxs-lookup"><span data-stu-id="770e7-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="770e7-211">All options are named instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-211">All options are named instances.</span></span> <span data-ttu-id="770e7-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="770e7-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="770e7-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="770e7-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span><span class="sxs-lookup"><span data-stu-id="770e7-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="770e7-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span><span class="sxs-lookup"><span data-stu-id="770e7-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="770e7-216">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="770e7-216">OptionsBuilder API</span></span>

<span data-ttu-id="770e7-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="770e7-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span><span class="sxs-lookup"><span data-stu-id="770e7-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="770e7-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="770e7-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="770e7-220">Use DI services to configure options</span><span class="sxs-lookup"><span data-stu-id="770e7-220">Use DI services to configure options</span></span>

<span data-ttu-id="770e7-221">You can access other services from dependency injection while configuring options in two ways:</span><span class="sxs-lookup"><span data-stu-id="770e7-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="770e7-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="770e7-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="770e7-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span><span class="sxs-lookup"><span data-stu-id="770e7-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="770e7-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span><span class="sxs-lookup"><span data-stu-id="770e7-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="770e7-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span><span class="sxs-lookup"><span data-stu-id="770e7-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="770e7-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="770e7-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="770e7-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span><span class="sxs-lookup"><span data-stu-id="770e7-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="770e7-228">Options validation</span><span class="sxs-lookup"><span data-stu-id="770e7-228">Options validation</span></span>

<span data-ttu-id="770e7-229">Options validation allows you to validate options when options are configured.</span><span class="sxs-lookup"><span data-stu-id="770e7-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="770e7-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span><span class="sxs-lookup"><span data-stu-id="770e7-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");

// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
}
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="770e7-231">The preceding example sets the named options instance to `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="770e7-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="770e7-232">The default options instance is `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="770e7-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="770e7-233">Validation runs when the options instance is created.</span><span class="sxs-lookup"><span data-stu-id="770e7-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="770e7-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span><span class="sxs-lookup"><span data-stu-id="770e7-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="770e7-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span><span class="sxs-lookup"><span data-stu-id="770e7-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="770e7-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="770e7-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="770e7-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span><span class="sxs-lookup"><span data-stu-id="770e7-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="770e7-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="770e7-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="770e7-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="770e7-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="770e7-240">`IValidateOptions` validates:</span><span class="sxs-lookup"><span data-stu-id="770e7-240">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="770e7-241">A specific named options instance.</span><span class="sxs-lookup"><span data-stu-id="770e7-241">A specific named options instance.</span></span>
* <span data-ttu-id="770e7-242">All options when `name` is `null`.</span><span class="sxs-lookup"><span data-stu-id="770e7-242">All options when `name` is `null`.</span></span>

<span data-ttu-id="770e7-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span><span class="sxs-lookup"><span data-stu-id="770e7-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="770e7-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="770e7-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="770e7-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span><span class="sxs-lookup"><span data-stu-id="770e7-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.Extensions.DependencyInjection;

private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="770e7-246">Eager validation (fail fast at startup) is under consideration for a future release.</span><span class="sxs-lookup"><span data-stu-id="770e7-246">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="770e7-247">Options post-configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-247">Options post-configuration</span></span>

<span data-ttu-id="770e7-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="770e7-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span><span class="sxs-lookup"><span data-stu-id="770e7-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="770e7-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span><span class="sxs-lookup"><span data-stu-id="770e7-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="770e7-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span><span class="sxs-lookup"><span data-stu-id="770e7-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="770e7-252">Accessing options during startup</span><span class="sxs-lookup"><span data-stu-id="770e7-252">Accessing options during startup</span></span>

<span data-ttu-id="770e7-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span><span class="sxs-lookup"><span data-stu-id="770e7-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="770e7-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="770e7-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="770e7-255">An inconsistent options state may exist due to the ordering of service registrations.</span><span class="sxs-lookup"><span data-stu-id="770e7-255">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="770e7-256">The options pattern uses classes to represent groups of related settings.</span><span class="sxs-lookup"><span data-stu-id="770e7-256">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="770e7-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span><span class="sxs-lookup"><span data-stu-id="770e7-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="770e7-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span><span class="sxs-lookup"><span data-stu-id="770e7-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="770e7-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span><span class="sxs-lookup"><span data-stu-id="770e7-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="770e7-260">Options also provide a mechanism to validate configuration data.</span><span class="sxs-lookup"><span data-stu-id="770e7-260">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="770e7-261">For more information, see the [Options validation](#options-validation) section.</span><span class="sxs-lookup"><span data-stu-id="770e7-261">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="770e7-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="770e7-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="770e7-263">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="770e7-263">Prerequisites</span></span>

<span data-ttu-id="770e7-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span><span class="sxs-lookup"><span data-stu-id="770e7-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="770e7-265">Options interfaces</span><span class="sxs-lookup"><span data-stu-id="770e7-265">Options interfaces</span></span>

<span data-ttu-id="770e7-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="770e7-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span><span class="sxs-lookup"><span data-stu-id="770e7-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="770e7-268">Change notifications</span><span class="sxs-lookup"><span data-stu-id="770e7-268">Change notifications</span></span>
* [<span data-ttu-id="770e7-269">Named options</span><span class="sxs-lookup"><span data-stu-id="770e7-269">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="770e7-270">Reloadable configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-270">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="770e7-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="770e7-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="770e7-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span><span class="sxs-lookup"><span data-stu-id="770e7-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="770e7-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="770e7-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span><span class="sxs-lookup"><span data-stu-id="770e7-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="770e7-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span><span class="sxs-lookup"><span data-stu-id="770e7-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="770e7-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span><span class="sxs-lookup"><span data-stu-id="770e7-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="770e7-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="770e7-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="770e7-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="770e7-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="770e7-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="770e7-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span><span class="sxs-lookup"><span data-stu-id="770e7-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="770e7-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span><span class="sxs-lookup"><span data-stu-id="770e7-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="770e7-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span><span class="sxs-lookup"><span data-stu-id="770e7-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="770e7-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span><span class="sxs-lookup"><span data-stu-id="770e7-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="770e7-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="770e7-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="770e7-286">General options configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-286">General options configuration</span></span>

<span data-ttu-id="770e7-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="770e7-288">An options class must be non-abstract with a public parameterless constructor.</span><span class="sxs-lookup"><span data-stu-id="770e7-288">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="770e7-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span><span class="sxs-lookup"><span data-stu-id="770e7-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="770e7-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span><span class="sxs-lookup"><span data-stu-id="770e7-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="770e7-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="770e7-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span><span class="sxs-lookup"><span data-stu-id="770e7-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="770e7-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="770e7-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span><span class="sxs-lookup"><span data-stu-id="770e7-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="770e7-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="770e7-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span><span class="sxs-lookup"><span data-stu-id="770e7-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="770e7-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="770e7-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="770e7-298">Configure simple options with a delegate</span><span class="sxs-lookup"><span data-stu-id="770e7-298">Configure simple options with a delegate</span></span>

<span data-ttu-id="770e7-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="770e7-300">Use a delegate to set options values.</span><span class="sxs-lookup"><span data-stu-id="770e7-300">Use a delegate to set options values.</span></span> <span data-ttu-id="770e7-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="770e7-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="770e7-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="770e7-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="770e7-304">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="770e7-304">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="770e7-305">You can add multiple configuration providers.</span><span class="sxs-lookup"><span data-stu-id="770e7-305">You can add multiple configuration providers.</span></span> <span data-ttu-id="770e7-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span><span class="sxs-lookup"><span data-stu-id="770e7-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="770e7-307">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="770e7-307">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="770e7-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="770e7-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span><span class="sxs-lookup"><span data-stu-id="770e7-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="770e7-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span><span class="sxs-lookup"><span data-stu-id="770e7-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="770e7-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="770e7-312">Suboptions configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-312">Suboptions configuration</span></span>

<span data-ttu-id="770e7-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="770e7-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span><span class="sxs-lookup"><span data-stu-id="770e7-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="770e7-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span><span class="sxs-lookup"><span data-stu-id="770e7-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="770e7-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="770e7-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="770e7-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="770e7-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="770e7-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="770e7-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="770e7-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="770e7-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span><span class="sxs-lookup"><span data-stu-id="770e7-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="770e7-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="770e7-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="770e7-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="770e7-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="770e7-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="770e7-325">Options injection</span><span class="sxs-lookup"><span data-stu-id="770e7-325">Options injection</span></span>

<span data-ttu-id="770e7-326">Options injection is demonstrated as Example &num;4 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-326">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="770e7-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span><span class="sxs-lookup"><span data-stu-id="770e7-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="770e7-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span><span class="sxs-lookup"><span data-stu-id="770e7-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="770e7-329">A page or view model.</span><span class="sxs-lookup"><span data-stu-id="770e7-329">A page or view model.</span></span>

<span data-ttu-id="770e7-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="770e7-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span><span class="sxs-lookup"><span data-stu-id="770e7-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="770e7-332">When the app is run, the options values are shown in the rendered page:</span><span class="sxs-lookup"><span data-stu-id="770e7-332">When the app is run, the options values are shown in the rendered page:</span></span>

![Options values Option1: value1_from_json and Option2: -1 are loaded from the model and by injection into the view.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="770e7-334">Reload configuration data with IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="770e7-334">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="770e7-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="770e7-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span><span class="sxs-lookup"><span data-stu-id="770e7-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="770e7-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span><span class="sxs-lookup"><span data-stu-id="770e7-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="770e7-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span><span class="sxs-lookup"><span data-stu-id="770e7-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="770e7-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span><span class="sxs-lookup"><span data-stu-id="770e7-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="770e7-340">Options snapshots are designed for use with transient and scoped dependencies.</span><span class="sxs-lookup"><span data-stu-id="770e7-340">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="770e7-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="770e7-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="770e7-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span><span class="sxs-lookup"><span data-stu-id="770e7-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="770e7-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="770e7-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="770e7-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span><span class="sxs-lookup"><span data-stu-id="770e7-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="770e7-345">Save the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="770e7-345">Save the *appsettings.json* file.</span></span> <span data-ttu-id="770e7-346">Refresh the browser to see that the options values are updated:</span><span class="sxs-lookup"><span data-stu-id="770e7-346">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="770e7-347">Named options support with IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="770e7-347">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="770e7-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="770e7-349">*Named options* support allows the app to distinguish between named options configurations.</span><span class="sxs-lookup"><span data-stu-id="770e7-349">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="770e7-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span><span class="sxs-lookup"><span data-stu-id="770e7-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="770e7-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="770e7-352">Running the sample app, the named options are returned:</span><span class="sxs-lookup"><span data-stu-id="770e7-352">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="770e7-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="770e7-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="770e7-354">`named_options_2` values are provided by:</span><span class="sxs-lookup"><span data-stu-id="770e7-354">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="770e7-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span><span class="sxs-lookup"><span data-stu-id="770e7-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="770e7-356">The default value for `Option2` provided by the `MyOptions` class.</span><span class="sxs-lookup"><span data-stu-id="770e7-356">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="770e7-357">Configure all options with the ConfigureAll method</span><span class="sxs-lookup"><span data-stu-id="770e7-357">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="770e7-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span><span class="sxs-lookup"><span data-stu-id="770e7-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="770e7-359">The following code configures `Option1` for all configuration instances with a common value.</span><span class="sxs-lookup"><span data-stu-id="770e7-359">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="770e7-360">Add the following code manually to the `Startup.ConfigureServices` method:</span><span class="sxs-lookup"><span data-stu-id="770e7-360">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="770e7-361">Running the sample app after adding the code produces the following result:</span><span class="sxs-lookup"><span data-stu-id="770e7-361">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="770e7-362">All options are named instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-362">All options are named instances.</span></span> <span data-ttu-id="770e7-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="770e7-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="770e7-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="770e7-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span><span class="sxs-lookup"><span data-stu-id="770e7-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="770e7-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span><span class="sxs-lookup"><span data-stu-id="770e7-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="770e7-367">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="770e7-367">OptionsBuilder API</span></span>

<span data-ttu-id="770e7-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="770e7-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span><span class="sxs-lookup"><span data-stu-id="770e7-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="770e7-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="770e7-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="770e7-371">Use DI services to configure options</span><span class="sxs-lookup"><span data-stu-id="770e7-371">Use DI services to configure options</span></span>

<span data-ttu-id="770e7-372">You can access other services from dependency injection while configuring options in two ways:</span><span class="sxs-lookup"><span data-stu-id="770e7-372">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="770e7-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="770e7-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="770e7-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span><span class="sxs-lookup"><span data-stu-id="770e7-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="770e7-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span><span class="sxs-lookup"><span data-stu-id="770e7-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="770e7-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span><span class="sxs-lookup"><span data-stu-id="770e7-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="770e7-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="770e7-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="770e7-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span><span class="sxs-lookup"><span data-stu-id="770e7-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="770e7-379">Options validation</span><span class="sxs-lookup"><span data-stu-id="770e7-379">Options validation</span></span>

<span data-ttu-id="770e7-380">Options validation allows you to validate options when options are configured.</span><span class="sxs-lookup"><span data-stu-id="770e7-380">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="770e7-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span><span class="sxs-lookup"><span data-stu-id="770e7-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");

// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
}
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="770e7-382">The preceding example sets the named options instance to `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="770e7-382">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="770e7-383">The default options instance is `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="770e7-383">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="770e7-384">Validation runs when the options instance is created.</span><span class="sxs-lookup"><span data-stu-id="770e7-384">Validation runs when the options instance is created.</span></span> <span data-ttu-id="770e7-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span><span class="sxs-lookup"><span data-stu-id="770e7-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="770e7-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span><span class="sxs-lookup"><span data-stu-id="770e7-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="770e7-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="770e7-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="770e7-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span><span class="sxs-lookup"><span data-stu-id="770e7-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="770e7-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="770e7-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="770e7-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="770e7-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="770e7-391">`IValidateOptions` validates:</span><span class="sxs-lookup"><span data-stu-id="770e7-391">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="770e7-392">A specific named options instance.</span><span class="sxs-lookup"><span data-stu-id="770e7-392">A specific named options instance.</span></span>
* <span data-ttu-id="770e7-393">All options when `name` is `null`.</span><span class="sxs-lookup"><span data-stu-id="770e7-393">All options when `name` is `null`.</span></span>

<span data-ttu-id="770e7-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span><span class="sxs-lookup"><span data-stu-id="770e7-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="770e7-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="770e7-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="770e7-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="770e7-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="770e7-397">Eager validation (fail fast at startup) is under consideration for a future release.</span><span class="sxs-lookup"><span data-stu-id="770e7-397">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="770e7-398">Options post-configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-398">Options post-configuration</span></span>

<span data-ttu-id="770e7-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="770e7-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span><span class="sxs-lookup"><span data-stu-id="770e7-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="770e7-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span><span class="sxs-lookup"><span data-stu-id="770e7-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="770e7-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span><span class="sxs-lookup"><span data-stu-id="770e7-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="770e7-403">Accessing options during startup</span><span class="sxs-lookup"><span data-stu-id="770e7-403">Accessing options during startup</span></span>

<span data-ttu-id="770e7-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span><span class="sxs-lookup"><span data-stu-id="770e7-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="770e7-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="770e7-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="770e7-406">An inconsistent options state may exist due to the ordering of service registrations.</span><span class="sxs-lookup"><span data-stu-id="770e7-406">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="770e7-407">The options pattern uses classes to represent groups of related settings.</span><span class="sxs-lookup"><span data-stu-id="770e7-407">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="770e7-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span><span class="sxs-lookup"><span data-stu-id="770e7-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="770e7-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span><span class="sxs-lookup"><span data-stu-id="770e7-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="770e7-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span><span class="sxs-lookup"><span data-stu-id="770e7-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="770e7-411">Options also provide a mechanism to validate configuration data.</span><span class="sxs-lookup"><span data-stu-id="770e7-411">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="770e7-412">For more information, see the [Options validation](#options-validation) section.</span><span class="sxs-lookup"><span data-stu-id="770e7-412">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="770e7-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="770e7-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="770e7-414">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="770e7-414">Prerequisites</span></span>

<span data-ttu-id="770e7-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span><span class="sxs-lookup"><span data-stu-id="770e7-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="770e7-416">Options interfaces</span><span class="sxs-lookup"><span data-stu-id="770e7-416">Options interfaces</span></span>

<span data-ttu-id="770e7-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="770e7-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span><span class="sxs-lookup"><span data-stu-id="770e7-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="770e7-419">Change notifications</span><span class="sxs-lookup"><span data-stu-id="770e7-419">Change notifications</span></span>
* [<span data-ttu-id="770e7-420">Named options</span><span class="sxs-lookup"><span data-stu-id="770e7-420">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="770e7-421">Reloadable configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-421">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="770e7-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="770e7-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="770e7-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span><span class="sxs-lookup"><span data-stu-id="770e7-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="770e7-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="770e7-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span><span class="sxs-lookup"><span data-stu-id="770e7-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="770e7-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span><span class="sxs-lookup"><span data-stu-id="770e7-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="770e7-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span><span class="sxs-lookup"><span data-stu-id="770e7-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="770e7-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="770e7-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="770e7-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="770e7-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="770e7-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="770e7-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span><span class="sxs-lookup"><span data-stu-id="770e7-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="770e7-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span><span class="sxs-lookup"><span data-stu-id="770e7-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="770e7-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span><span class="sxs-lookup"><span data-stu-id="770e7-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="770e7-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span><span class="sxs-lookup"><span data-stu-id="770e7-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="770e7-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="770e7-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="770e7-437">General options configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-437">General options configuration</span></span>

<span data-ttu-id="770e7-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="770e7-439">An options class must be non-abstract with a public parameterless constructor.</span><span class="sxs-lookup"><span data-stu-id="770e7-439">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="770e7-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span><span class="sxs-lookup"><span data-stu-id="770e7-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="770e7-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span><span class="sxs-lookup"><span data-stu-id="770e7-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="770e7-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="770e7-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span><span class="sxs-lookup"><span data-stu-id="770e7-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="770e7-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="770e7-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span><span class="sxs-lookup"><span data-stu-id="770e7-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="770e7-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="770e7-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span><span class="sxs-lookup"><span data-stu-id="770e7-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="770e7-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="770e7-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="770e7-449">Configure simple options with a delegate</span><span class="sxs-lookup"><span data-stu-id="770e7-449">Configure simple options with a delegate</span></span>

<span data-ttu-id="770e7-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="770e7-451">Use a delegate to set options values.</span><span class="sxs-lookup"><span data-stu-id="770e7-451">Use a delegate to set options values.</span></span> <span data-ttu-id="770e7-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="770e7-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="770e7-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="770e7-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="770e7-455">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="770e7-455">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="770e7-456">You can add multiple configuration providers.</span><span class="sxs-lookup"><span data-stu-id="770e7-456">You can add multiple configuration providers.</span></span> <span data-ttu-id="770e7-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span><span class="sxs-lookup"><span data-stu-id="770e7-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="770e7-458">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="770e7-458">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="770e7-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="770e7-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span><span class="sxs-lookup"><span data-stu-id="770e7-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="770e7-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span><span class="sxs-lookup"><span data-stu-id="770e7-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="770e7-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="770e7-463">Suboptions configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-463">Suboptions configuration</span></span>

<span data-ttu-id="770e7-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="770e7-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span><span class="sxs-lookup"><span data-stu-id="770e7-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="770e7-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span><span class="sxs-lookup"><span data-stu-id="770e7-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="770e7-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="770e7-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="770e7-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="770e7-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="770e7-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="770e7-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="770e7-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="770e7-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="770e7-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span><span class="sxs-lookup"><span data-stu-id="770e7-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="770e7-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="770e7-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="770e7-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="770e7-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="770e7-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span><span class="sxs-lookup"><span data-stu-id="770e7-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="770e7-476">Options provided by a view model or with direct view injection</span><span class="sxs-lookup"><span data-stu-id="770e7-476">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="770e7-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="770e7-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="770e7-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span><span class="sxs-lookup"><span data-stu-id="770e7-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="770e7-480">When the app is run, the options values are shown in the rendered page:</span><span class="sxs-lookup"><span data-stu-id="770e7-480">When the app is run, the options values are shown in the rendered page:</span></span>

![Options values Option1: value1_from_json and Option2: -1 are loaded from the model and by injection into the view.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="770e7-482">Reload configuration data with IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="770e7-482">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="770e7-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="770e7-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span><span class="sxs-lookup"><span data-stu-id="770e7-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="770e7-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span><span class="sxs-lookup"><span data-stu-id="770e7-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="770e7-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="770e7-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="770e7-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span><span class="sxs-lookup"><span data-stu-id="770e7-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="770e7-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="770e7-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="770e7-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span><span class="sxs-lookup"><span data-stu-id="770e7-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="770e7-490">Save the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="770e7-490">Save the *appsettings.json* file.</span></span> <span data-ttu-id="770e7-491">Refresh the browser to see that the options values are updated:</span><span class="sxs-lookup"><span data-stu-id="770e7-491">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="770e7-492">Named options support with IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="770e7-492">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="770e7-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="770e7-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="770e7-494">*Named options* support allows the app to distinguish between named options configurations.</span><span class="sxs-lookup"><span data-stu-id="770e7-494">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="770e7-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span><span class="sxs-lookup"><span data-stu-id="770e7-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="770e7-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="770e7-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="770e7-497">Running the sample app, the named options are returned:</span><span class="sxs-lookup"><span data-stu-id="770e7-497">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="770e7-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="770e7-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="770e7-499">`named_options_2` values are provided by:</span><span class="sxs-lookup"><span data-stu-id="770e7-499">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="770e7-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span><span class="sxs-lookup"><span data-stu-id="770e7-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="770e7-501">The default value for `Option2` provided by the `MyOptions` class.</span><span class="sxs-lookup"><span data-stu-id="770e7-501">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="770e7-502">Configure all options with the ConfigureAll method</span><span class="sxs-lookup"><span data-stu-id="770e7-502">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="770e7-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span><span class="sxs-lookup"><span data-stu-id="770e7-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="770e7-504">The following code configures `Option1` for all configuration instances with a common value.</span><span class="sxs-lookup"><span data-stu-id="770e7-504">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="770e7-505">Add the following code manually to the `Startup.ConfigureServices` method:</span><span class="sxs-lookup"><span data-stu-id="770e7-505">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="770e7-506">Running the sample app after adding the code produces the following result:</span><span class="sxs-lookup"><span data-stu-id="770e7-506">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="770e7-507">All options are named instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-507">All options are named instances.</span></span> <span data-ttu-id="770e7-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="770e7-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="770e7-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="770e7-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span><span class="sxs-lookup"><span data-stu-id="770e7-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="770e7-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span><span class="sxs-lookup"><span data-stu-id="770e7-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="770e7-512">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="770e7-512">OptionsBuilder API</span></span>

<span data-ttu-id="770e7-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="770e7-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="770e7-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span><span class="sxs-lookup"><span data-stu-id="770e7-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="770e7-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="770e7-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="770e7-516">Use DI services to configure options</span><span class="sxs-lookup"><span data-stu-id="770e7-516">Use DI services to configure options</span></span>

<span data-ttu-id="770e7-517">You can access other services from dependency injection while configuring options in two ways:</span><span class="sxs-lookup"><span data-stu-id="770e7-517">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="770e7-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="770e7-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="770e7-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span><span class="sxs-lookup"><span data-stu-id="770e7-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="770e7-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span><span class="sxs-lookup"><span data-stu-id="770e7-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="770e7-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span><span class="sxs-lookup"><span data-stu-id="770e7-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="770e7-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="770e7-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="770e7-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span><span class="sxs-lookup"><span data-stu-id="770e7-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="770e7-524">Options post-configuration</span><span class="sxs-lookup"><span data-stu-id="770e7-524">Options post-configuration</span></span>

<span data-ttu-id="770e7-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="770e7-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="770e7-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span><span class="sxs-lookup"><span data-stu-id="770e7-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="770e7-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span><span class="sxs-lookup"><span data-stu-id="770e7-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="770e7-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span><span class="sxs-lookup"><span data-stu-id="770e7-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="770e7-529">Accessing options during startup</span><span class="sxs-lookup"><span data-stu-id="770e7-529">Accessing options during startup</span></span>

<span data-ttu-id="770e7-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span><span class="sxs-lookup"><span data-stu-id="770e7-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="770e7-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="770e7-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="770e7-532">An inconsistent options state may exist due to the ordering of service registrations.</span><span class="sxs-lookup"><span data-stu-id="770e7-532">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="770e7-533">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="770e7-533">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
