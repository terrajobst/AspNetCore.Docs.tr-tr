---
title: ASP.NET Core desende seçenekleri
author: guardrex
description: ASP.NET Core uygulamalarda ilgili ayar gruplarını göstermek için seçenekleri düzeni kullanmayı keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 96d7d2956fa9bf72706cde0532ee7f4ff753b72c
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126267"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="340fe-103">ASP.NET Core desende seçenekleri</span><span class="sxs-lookup"><span data-stu-id="340fe-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="340fe-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="340fe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="340fe-105">Seçenekler düzeni sınıfları ilgili ayar gruplarını göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="340fe-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="340fe-106">Yapılandırma ayarları ayrı sınıflara özelliğiyle yalıtılır, uygulama için iki önemli yazılım mühendislik ilkeden aynılarını:</span><span class="sxs-lookup"><span data-stu-id="340fe-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="340fe-107">[Arabirimi arasında ayrım yapma ilkesine (ISS)](http://deviq.com/interface-segregation-principle/): yapılandırma ayarlarına bağlıdır (sınıflar) özelliklerine bağlıdır, kullandıkları yapılandırma ayarları.</span><span class="sxs-lookup"><span data-stu-id="340fe-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="340fe-108">[Sorunları ayrılması](http://deviq.com/separation-of-concerns/): uygulamanın farklı bölümleri için ayarları bağımlı veya birbiriyle eşleşmiş değil.</span><span class="sxs-lookup"><span data-stu-id="340fe-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="340fe-109">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) Bu makalede örnek uygulama ile izleyin daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="340fe-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="340fe-110">Temel seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="340fe-110">Basic options configuration</span></span>

<span data-ttu-id="340fe-111">Temel Seçenekler yapılandırma, örnek olarak gösterilmiştir &num;1'de [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="340fe-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="340fe-112">Özet olmayan bir seçenek sınıfı olmalıdır genel bir parametresiz oluşturucuya sahip.</span><span class="sxs-lookup"><span data-stu-id="340fe-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="340fe-113">Aşağıdaki sınıf `MyOptions`, iki özelliğe sahip `Option1` ve `Option2`.</span><span class="sxs-lookup"><span data-stu-id="340fe-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="340fe-114">Varsayılan değerleri ayarlama isteğe bağlı olmakla birlikte, aşağıdaki örnekte sınıfı oluşturucusu varsayılan değerini ayarlar `Option1`.</span><span class="sxs-lookup"><span data-stu-id="340fe-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="340fe-115">`Option2` özellik doğrudan başlatarak ayarlamak varsayılan değeri (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="340fe-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="340fe-116">`MyOptions` Sınıf ile hizmet kapsayıcısı eklenen [yapılandırma&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) ve yapılandırmasına bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="340fe-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="340fe-117">Aşağıdaki modelinin kullandığı sayfa [Oluşturucusu bağımlılık ekleme](xref:fundamentals/dependency-injection#what-is-dependency-injection) ile [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) ayarlarına erişmek için (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="340fe-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="340fe-118">Örnek 's *appsettings.json* dosyayı belirtir değerlerini `option1` ve `option2`:</span><span class="sxs-lookup"><span data-stu-id="340fe-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="340fe-119">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="340fe-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="340fe-120">Özel bir kullanırken [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) seçenekleri yapılandırma ayarları dosyasından yüklemek için temel yolu doğru şekilde ayarlandığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="340fe-120">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="340fe-121">Taban yol açıkça ayarlama değil gerekli seçenekleri yapılandırma ayarları dosyasından yüklenirken [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="340fe-121">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="340fe-122">Bir temsilci ile basit seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="340fe-122">Configure simple options with a delegate</span></span>

<span data-ttu-id="340fe-123">Bir temsilci ile basit seçeneklerini yapılandırma örnek olarak gösterilmiştir &num;2'de [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="340fe-123">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="340fe-124">Bir temsilci seçenekleri değerleri ayarlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="340fe-124">Use a delegate to set options values.</span></span> <span data-ttu-id="340fe-125">Örnek uygulama kullandığı `MyOptionsWithDelegateConfig` sınıfı (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="340fe-125">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="340fe-126">Aşağıdaki kodda, ikinci bir `IConfigureOptions<TOptions>` hizmeti için hizmet kapsayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="340fe-126">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="340fe-127">Bağlama ile yapılandırmak için bir temsilci kullanır `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="340fe-127">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="340fe-128">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="340fe-128">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="340fe-129">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="340fe-129">You can add multiple configuration providers.</span></span> <span data-ttu-id="340fe-130">Yapılandırma sağlayıcıları NuGet paketleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="340fe-130">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="340fe-131">Kayıtlı edebilmesi uygulanmasıyla.</span><span class="sxs-lookup"><span data-stu-id="340fe-131">They're applied in order that they're registered.</span></span>

<span data-ttu-id="340fe-132">Her çağrı [yapılandırma&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ekler bir `IConfigureOptions<TOptions>` service hizmet kapsayıcısı.</span><span class="sxs-lookup"><span data-stu-id="340fe-132">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="340fe-133">Önceki örnekte, değerlerini `Option1` ve `Option2` her ikisi de belirtilen *appsettings.json*, ancak değerlerini `Option1` ve `Option2` yapılandırılmış temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="340fe-133">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="340fe-134">Son yapılandırma kaynağı birden fazla Yapılandırma hizmeti etkinleştirildiğinde, belirtilen *WINS* ve yapılandırma değeri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="340fe-134">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="340fe-135">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="340fe-135">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="340fe-136">Suboptions yapılandırma</span><span class="sxs-lookup"><span data-stu-id="340fe-136">Suboptions configuration</span></span>

<span data-ttu-id="340fe-137">Suboptions yapılandırma örnek olarak gösterilmiştir &num;3'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="340fe-137">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="340fe-138">Uygulamalar, uygulama (sınıflar) belirli özellik gruplarına ait seçenekleri sınıfları oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="340fe-138">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="340fe-139">Yapılandırma değerlerini gerektiren uygulama bölümleri yalnızca kullandıkları yapılandırma değerlerini erişiminiz olması.</span><span class="sxs-lookup"><span data-stu-id="340fe-139">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="340fe-140">İçin yapılandırma seçenekleri bağlama sırasında her bir özellik seçenekleri türü form için bir yapılandırma anahtarı bağlı `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="340fe-140">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="340fe-141">Örneğin, `MyOptions.Option1` özellik anahtarına bağlı `Option1`, den okunan `option1` özelliğinde *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="340fe-141">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="340fe-142">Aşağıdaki kodda, üçüncü `IConfigureOptions<TOptions>` hizmeti için hizmet kapsayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="340fe-142">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="340fe-143">Bunu bağlar `MySubOptions` bölümüne `subsection` , *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="340fe-143">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="340fe-144">`GetSection` Genişletme yöntemi gerektiren [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="340fe-144">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="340fe-145">Uygulama kullanıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya sonrası), paketi otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="340fe-145">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="340fe-146">Örnek 's *appsettings.json* dosya tanımlayan bir `subsection` tuşları üyesiyle `suboption1` ve `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="340fe-146">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="340fe-147">`MySubOptions` Sınıfı tanımlayan özellikleri, `SubOption1` ve `SubOption2`, alt seçenek değerleri tutmak için (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="340fe-147">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="340fe-148">Sayfa modelinin `OnGet` yöntemi alt seçeneği değerleri içeren bir dize döndürür (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="340fe-148">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="340fe-149">Uygulama çalıştırıldığında `OnGet` yöntemi alt seçeneği sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="340fe-149">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="340fe-150">Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri</span><span class="sxs-lookup"><span data-stu-id="340fe-150">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="340fe-151">Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri, örnek olarak gösterilmiştir &num;4'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="340fe-151">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="340fe-152">Seçenekler sağlanan bir görünüm modeli veya injecting `IOptions<TOptions>` bir görünüm doğrudan (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="340fe-152">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="340fe-153">Doğrudan ekleme işlemi için ekleme `IOptions<MyOptions>` ile bir `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="340fe-153">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="340fe-154">Uygulama çalıştırıldığında seçenek değerlerinin oluşturulan sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="340fe-154">When the app is run, the option values are shown in the rendered page:</span></span>

![Seçenek değerleri seçenek 1: value1_from_json ve Seçenek2: -1 modelinden ve görünümüne yerleştirme tarafından yüklenir.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="340fe-156">Yapılandırma verileri IOptionsSnapshot ile yeniden yükleyin</span><span class="sxs-lookup"><span data-stu-id="340fe-156">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="340fe-157">Yapılandırma verileri ile yeniden `IOptionsSnapshot` gösterildiği gibi &num;5'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="340fe-157">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="340fe-158">*ASP.NET Core 1.1 veya üstünü gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="340fe-158">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="340fe-159">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) en az işlem ek yüküne ile seçenekleri görüntülemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="340fe-159">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="340fe-160">ASP.NET Core 1. 1'da, `IOptionsSnapshot` anlık görüntüsüdür [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) ve güncelleştirmeleri otomatik olarak her izleyici tetikleyen verileri değiştirme kaynağı bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="340fe-160">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="340fe-161">ASP.NET Core 2.0 ve daha sonra seçenekleri erişilen ve istek ömrü boyunca önbelleğe alınan istek başına bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="340fe-161">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="340fe-162">Aşağıdaki örnek yeni bir nasıl gösterir `IOptionsSnapshot` sonra oluşturulan *appsettings.json* değişiklikleri (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="340fe-162">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="340fe-163">Sunucuya birden çok istek dönüş tarafından sağlanan sabit değerleri *appsettings.json* dosya değiştirildi ve yapılandırmayı yeniden yükler kadar dosya.</span><span class="sxs-lookup"><span data-stu-id="340fe-163">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="340fe-164">Aşağıdaki resimde ilk gösterilmiştir `option1` ve `option2` değerleri yüklenen *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="340fe-164">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="340fe-165">Değerlerinde değişiklik *appsettings.json* dosya `value1_from_json UPDATED` ve `200`.</span><span class="sxs-lookup"><span data-stu-id="340fe-165">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="340fe-166">Kaydet *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="340fe-166">Save the *appsettings.json* file.</span></span> <span data-ttu-id="340fe-167">Seçenekler değerler güncelleştirilir görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="340fe-167">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="340fe-168">IConfigureNamedOptions seçenekleri desteğiyle adlı</span><span class="sxs-lookup"><span data-stu-id="340fe-168">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="340fe-169">Seçenekler desteğiyle adlı [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) örnek olarak gösterilmiştir &num;6 ' [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="340fe-169">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="340fe-170">*ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="340fe-170">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="340fe-171">*Seçenekler adlı* adlandırılmış seçeneklerini yapılandırmaları arasında ayrım yapmak uygulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="340fe-171">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="340fe-172">Örnek uygulaması adlandırılmış seçenekleri ile bildirilen [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, dize, eylem&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure)genişletme yöntemi sırayla çağıran [ConfigureNamedOptions&lt;TOptions&gt;. Yapılandırma](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="340fe-172">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="340fe-173">Örnek uygulaması adlandırılmış seçenekleriyle eriştiği [IOptionsSnapshot&lt;TOptions&gt;. Alma](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="340fe-173">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="340fe-174">Örnek uygulama çalışırken, adlandırılmış seçenekleri döndürülür:</span><span class="sxs-lookup"><span data-stu-id="340fe-174">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="340fe-175">`named_options_1` değerleri, gelen yüklenen yapılandırmasından sağlanan *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="340fe-175">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="340fe-176">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="340fe-176">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="340fe-177">`named_options_2` İçinde temsilci `ConfigureServices` için `Option1`.</span><span class="sxs-lookup"><span data-stu-id="340fe-177">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="340fe-178">İçin varsayılan değer `Option2` tarafından sağlanan `MyOptions` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="340fe-178">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="340fe-179">Tüm adlandırılmış seçenekleri örnekleriyle yapılandırma [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="340fe-179">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="340fe-180">Aşağıdaki kod yapılandırır `Option1` tüm yapılandırma örnekleri ortak bir değerle adlı.</span><span class="sxs-lookup"><span data-stu-id="340fe-180">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="340fe-181">Aşağıdaki kodu el ile ekleyebilirsiniz `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="340fe-181">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="340fe-182">Kod ekledikten sonra örnek uygulaması çalıştıran aşağıdaki sonucu üretir:</span><span class="sxs-lookup"><span data-stu-id="340fe-182">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="340fe-183">ASP.NET Core 2.0 ve daha sonra tüm seçenekleri örnekleri adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="340fe-183">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="340fe-184">Varolan `IConfigureOption` örnekleri hedefleme olarak kabul edilir `Options.DefaultName` olan örneği `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="340fe-184">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="340fe-185">`IConfigureNamedOptions` Ayrıca `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="340fe-185">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="340fe-186">Varsayılan uygulaması [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([başvuru kaynağı](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) her uygun şekilde kullanmak için mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="340fe-186">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="340fe-187">`null` Adlandırılmış seçeneği tüm adlandırılmış örnek yerine adlandırılmış örneği belirli bir hedef için kullanılır ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) ve [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) bu yöntemi kullanın).</span><span class="sxs-lookup"><span data-stu-id="340fe-187">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="340fe-188">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="340fe-188">IPostConfigureOptions</span></span>

<span data-ttu-id="340fe-189">*ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="340fe-189">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="340fe-190">İle postconfiguration ayarlamak [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="340fe-190">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="340fe-191">Postconfiguration çalıştıran tüm [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) yapılandırma gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="340fe-191">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="340fe-192">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) sonrası adlandırılmış seçeneklerini yapılandırmak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="340fe-192">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="340fe-193">Kullanım [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) yapılandırma örnekleri adlı sonrası yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="340fe-193">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="340fe-194">Seçenekler Fabrika, izleme ve önbellek</span><span class="sxs-lookup"><span data-stu-id="340fe-194">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="340fe-195">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) bildirimler için kullanılan zaman `TOptions` örnekleri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="340fe-195">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="340fe-196">`IOptionsMonitor` reloadable seçeneklerini destekler, bildirimler, değiştirmek ve `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="340fe-196">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="340fe-197">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) yeni oluşturma örnekleri Seçenekler (ASP.NET Core 2.0 veya üstü) sorumlu.</span><span class="sxs-lookup"><span data-stu-id="340fe-197">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="340fe-198">Tek bir sahip [oluşturma](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="340fe-198">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="340fe-199">Varsayılan uygulamasını tüm kayıtlı alır `IConfigureOptions` ve `IPostConfigureOptions` ve tüm çalıştırır önce yapılandırır, arkasından sonrası yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="340fe-199">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="340fe-200">Arasında ayırt `IConfigureNamedOptions` ve `IConfigureOptions` ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="340fe-200">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="340fe-201">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 veya üstü) tarafından kullanılan `IOptionsMonitor` önbelleğe `TOptions` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="340fe-201">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="340fe-202">`IOptionsMonitorCache` Değeri yeniden böylece seçenekleri örnekleri İzleyicisi'nde geçersiz kılar ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="340fe-202">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="340fe-203">Değerleri el ile de ile sunulan [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="340fe-203">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="340fe-204">[Temizle](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) yöntemi, tüm adlandırılmış örnekleri isteğe bağlı olarak yeniden oluşturulması olduğunda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="340fe-204">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="340fe-205">Başlatma sırasında erişilebilirlik seçenekleri</span><span class="sxs-lookup"><span data-stu-id="340fe-205">Accessing options during startup</span></span>

<span data-ttu-id="340fe-206">`IOptions` kullanılabilir `Configure`, önce Hizmetleri yerleşiktir bu yana `Configure` yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="340fe-206">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="340fe-207">Bir hizmet sağlayıcısı oluşturulursa `ConfigureServices` seçeneklerine erişmek için onu içeremez olmayacaktır seçenekleri hizmet sağlayıcısı oluşturulduktan sonra sağlanan yapılandırmaları.</span><span class="sxs-lookup"><span data-stu-id="340fe-207">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="340fe-208">Bu nedenle, bir tutarsız seçenekleri durum hizmet kayıtları sıralama nedeniyle olabilir.</span><span class="sxs-lookup"><span data-stu-id="340fe-208">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="340fe-209">Seçenekleri genellikle yapılandırmasından yüklendiğinden bu yana yapılandırma hem de başlangıç kullanılabilir `Configure` ve `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="340fe-209">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="340fe-210">Başlatma sırasında yapılandırmayla örnekler için bkz: [uygulama başlangıç](xref:fundamentals/startup) konu.</span><span class="sxs-lookup"><span data-stu-id="340fe-210">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="340fe-211">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="340fe-211">See also</span></span>

* [<span data-ttu-id="340fe-212">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="340fe-212">Configuration</span></span>](xref:fundamentals/configuration/index)
