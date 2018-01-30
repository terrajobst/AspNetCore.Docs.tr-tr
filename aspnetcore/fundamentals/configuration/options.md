---
title: "ASP.NET Core desende seçenekleri"
author: guardrex
description: "ASP.NET Core uygulamalarda ilgili ayar gruplarını göstermek için seçenekleri düzeni kullanmayı keşfedin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: abb3b92af07a7b3b199712fcfdc459ca283d0017
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="ce4a8-103">ASP.NET Core desende seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ce4a8-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="ce4a8-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ce4a8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ce4a8-105">Seçenekler düzeni seçenekleri sınıfları ilgili ayar gruplarını göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="ce4a8-106">Yapılandırma ayarlarını ayrı seçenekler sınıflara özelliğiyle yalıtılır, uygulama için iki önemli yazılım mühendislik ilkeden uyar:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ce4a8-107">[Arabirimi arasında ayrım yapma ilkesine (ISS)](http://deviq.com/interface-segregation-principle/): yapılandırma ayarlarına bağlıdır (sınıflar) özelliklerine bağlıdır, kullandıkları yapılandırma ayarları.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ce4a8-108">[Sorunları ayrılması](http://deviq.com/separation-of-concerns/): uygulamanın farklı bölümleri için ayarları bağımlı veya birbiriyle eşleşmiş değil.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ce4a8-109">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) Bu makalede örnek uygulama ile izleyin daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="ce4a8-110">Temel seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ce4a8-110">Basic options configuration</span></span>

<span data-ttu-id="ce4a8-111">Temel Seçenekler yapılandırma, örnek olarak gösterilmiştir &num;1'de [örnek uygulaması](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ce4a8-112">Özet olmayan bir seçenek sınıfı olmalıdır genel bir parametresiz oluşturucuya sahip.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ce4a8-113">Aşağıdaki sınıf `MyOptions`, iki özelliğe sahip `Option1` ve `Option2`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ce4a8-114">Varsayılan değerleri ayarlama isteğe bağlı olmakla birlikte, aşağıdaki örnekte sınıfı oluşturucusu varsayılan değerini ayarlar `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ce4a8-115">`Option2`özellik doğrudan başlatarak ayarlamak varsayılan değeri (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce4a8-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ce4a8-116">`MyOptions` Sınıf ile hizmet kapsayıcısı eklenen [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) ve yapılandırmasına bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ce4a8-117">Aşağıdaki modelinin kullandığı sayfa [Oluşturucusu bağımlılık ekleme](xref:fundamentals/dependency-injection#what-is-dependency-injection) ile [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) ayarlarına erişmek için (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce4a8-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ce4a8-118">Örnek 's *appsettings.json* dosyayı belirtir değerlerini `option1` ve `option2`:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="ce4a8-119">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ce4a8-120">Bir temsilci ile basit seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ce4a8-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="ce4a8-121">Bir temsilci ile basit seçeneklerini yapılandırma örnek olarak gösterilmiştir &num;2'de [örnek uygulaması](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ce4a8-122">Bir temsilci seçenekleri değerleri ayarlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-122">Use a delegate to set options values.</span></span> <span data-ttu-id="ce4a8-123">Örnek uygulama kullandığı `MyOptionsWithDelegateConfig` sınıfı (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce4a8-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ce4a8-124">Aşağıdaki kodda, ikinci bir `IConfigureOptions<TOptions>` hizmeti için hizmet kapsayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ce4a8-125">Bağlama ile yapılandırmak için bir temsilci kullanır `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ce4a8-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ce4a8-127">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="ce4a8-128">Yapılandırma sağlayıcıları NuGet paketleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="ce4a8-129">Kayıtlı edebilmesi uygulanmasıyla.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="ce4a8-130">Her çağrı [yapılandırma&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ekler bir `IConfigureOptions<TOptions>` service hizmet kapsayıcısı.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="ce4a8-131">Önceki örnekte, değerlerini `Option1` ve `Option2` her ikisi de belirtilen *appsettings.json*, ancak değerlerini `Option1` ve `Option2` yapılandırılmış temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ce4a8-132">Son yapılandırma kaynağı birden fazla Yapılandırma hizmeti etkinleştirildiğinde, belirtilen *WINS* ve yapılandırma değeri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ce4a8-133">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ce4a8-134">Suboptions yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ce4a8-134">Suboptions configuration</span></span>

<span data-ttu-id="ce4a8-135">Suboptions yapılandırma örnek olarak gösterilmiştir &num;3'te [örnek uygulaması](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ce4a8-136">Uygulamalar, uygulama (sınıflar) belirli özellik gruplarına ait seçenekleri sınıfları oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="ce4a8-137">Yapılandırma değerlerini gerektiren uygulama bölümleri yalnızca kullandıkları yapılandırma değerlerini erişiminiz olması.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ce4a8-138">İçin yapılandırma seçenekleri bağlama sırasında her bir özellik seçenekleri türü form için bir yapılandırma anahtarı bağlı `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ce4a8-139">Örneğin, `MyOptions.Option1` özellik anahtarına bağlı `Option1`, den okunan `option1` özelliğinde *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ce4a8-140">Aşağıdaki kodda, üçüncü `IConfigureOptions<TOptions>` hizmeti için hizmet kapsayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ce4a8-141">Bunu bağlar `MySubOptions` bölümüne `subsection` , *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ce4a8-142">`GetSection` Genişletme yöntemi gerektiren [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="ce4a8-143">Uygulama kullanıyorsa [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, paketi otomatik olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="ce4a8-144">Örnek 's *appsettings.json* dosya tanımlayan bir `subsection` tuşları üyesiyle `suboption1` ve `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ce4a8-145">`MySubOptions` Sınıfı tanımlayan özellikleri, `SubOption1` ve `SubOption2`, alt seçenek değerleri tutmak için (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce4a8-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ce4a8-146">Sayfa modelinin `OnGet` yöntemi alt seçeneği değerleri içeren bir dize döndürür (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce4a8-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ce4a8-147">Uygulama çalıştırıldığında `OnGet` yöntemi alt seçeneği sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ce4a8-148">Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ce4a8-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ce4a8-149">Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri, örnek olarak gösterilmiştir &num;4'te [örnek uygulaması](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ce4a8-150">Seçenekler sağlanan bir görünüm modeli veya injecting `IOptions<TOptions>` bir görünüm doğrudan (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce4a8-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ce4a8-151">Doğrudan ekleme işlemi için ekleme `IOptions<MyOptions>` ile bir `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="ce4a8-152">Uygulama çalıştırıldığında seçenek değerlerinin oluşturulan sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Seçenek değerleri seçenek 1: value1_from_json ve Seçenek2: -1 modelinden ve görünümüne yerleştirme tarafından yüklenir.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ce4a8-154">Yapılandırma verileri IOptionsSnapshot ile yeniden yükleyin</span><span class="sxs-lookup"><span data-stu-id="ce4a8-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ce4a8-155">Yapılandırma verileri ile yeniden `IOptionsSnapshot` gösterildiği gibi &num;5'te [örnek uygulaması](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ce4a8-156">*ASP.NET Core 1.1 veya üstünü gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="ce4a8-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="ce4a8-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) en az işlem ek yüküne ile seçenekleri görüntülemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="ce4a8-158">ASP.NET Core 1. 1'da, `IOptionsSnapshot` anlık görüntüsüdür [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) ve güncelleştirmeleri otomatik olarak her izleyici tetikleyen verileri değiştirme kaynağı bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="ce4a8-159">ASP.NET Core 2.0 ve daha sonra seçenekleri erişilen ve istek ömrü boyunca önbelleğe alınan istek başına bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ce4a8-160">Aşağıdaki örnek yeni bir nasıl gösterir `IOptionsSnapshot` sonra oluşturulan *appsettings.json* değişiklikleri (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ce4a8-161">Sunucuya birden çok istek dönüş tarafından sağlanan sabit değerleri *appsettings.json* dosya değiştirildi ve yapılandırmayı yeniden yükler kadar dosya.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ce4a8-162">Aşağıdaki resimde ilk gösterilmiştir `option1` ve `option2` değerleri yüklenen *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ce4a8-163">Değerlerinde değişiklik *appsettings.json* dosya `value1_from_json UPDATED` ve `200`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ce4a8-164">Kaydet *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ce4a8-165">Seçenekler değerler güncelleştirilir görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ce4a8-166">IConfigureNamedOptions seçenekleri desteğiyle adlı</span><span class="sxs-lookup"><span data-stu-id="ce4a8-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ce4a8-167">Seçenekler desteğiyle adlı [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) örnek olarak gösterilmiştir &num;6 ' [örnek uygulaması](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ce4a8-168">*ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="ce4a8-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="ce4a8-169">*Seçenekler adlı* adlandırılmış seçeneklerini yapılandırmaları arasında ayrım yapmak uygulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ce4a8-170">Örnek uygulaması adlandırılmış seçenekleri ile bildirilen [ConfigureNamedOptions&lt;TOptions&gt;. Yapılandırma](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ce4a8-171">Örnek uygulaması adlandırılmış seçenekleriyle eriştiği [IOptionsSnapshot&lt;TOptions&gt;. Alma](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce4a8-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ce4a8-172">Örnek uygulama çalışırken, adlandırılmış seçenekleri döndürülür:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ce4a8-173">`named_options_1`değerleri, gelen yüklenen yapılandırmasından sağlanan *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ce4a8-174">`named_options_2`değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ce4a8-175">`named_options_2` İçinde temsilci `ConfigureServices` için `Option1`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ce4a8-176">İçin varsayılan değer `Option2` tarafından sağlanan `MyOptions` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="ce4a8-177">Tüm adlandırılmış seçenekleri örnekleriyle yapılandırma [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="ce4a8-178">Aşağıdaki kod yapılandırır `Option1` tüm yapılandırma örnekleri ortak bir değerle adlı.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="ce4a8-179">Aşağıdaki kodu el ile ekleyebilirsiniz `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ce4a8-180">Kod ekledikten sonra örnek uygulaması çalıştıran aşağıdaki sonucu üretir:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ce4a8-181">ASP.NET Core 2.0 ve daha sonra tüm seçenekleri örnekleri adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="ce4a8-182">Varolan `IConfigureOption` örnekleri hedefleme olarak kabul edilir `Options.DefaultName` olan örneği `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ce4a8-183">`IConfigureNamedOptions`Ayrıca `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="ce4a8-184">Varsayılan uygulaması [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([başvuru kaynağı](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) her uygun şekilde kullanmak için mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="ce4a8-185">`null` Adlandırılmış seçeneği tüm adlandırılmış örnek yerine adlandırılmış örneği belirli bir hedef için kullanılır ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) ve [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) bu yöntemi kullanın).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="ce4a8-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="ce4a8-186">IPostConfigureOptions</span></span>

<span data-ttu-id="ce4a8-187">*ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="ce4a8-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="ce4a8-188">İle postconfiguration ayarlamak [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="ce4a8-189">Postconfiguration çalıştıran tüm [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) yapılandırma gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ce4a8-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) sonrası adlandırılmış seçeneklerini yapılandırmak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ce4a8-191">Kullanım [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) yapılandırma örnekleri adlı sonrası yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="ce4a8-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="ce4a8-192">Seçenekler Fabrika, izleme ve önbellek</span><span class="sxs-lookup"><span data-stu-id="ce4a8-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="ce4a8-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) bildirimler için kullanılan zaman `TOptions` örnekleri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="ce4a8-194">`IOptionsMonitor`reloadable seçeneklerini destekler, bildirimler, değiştirmek ve `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="ce4a8-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="ce4a8-196">Tek bir sahip [oluşturma](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="ce4a8-197">Varsayılan uygulamasını tüm kayıtlı alır `IConfigureOptions` ve `IPostConfigureOptions` ve tüm çalıştırır önce yapılandırır, arkasından sonrası yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="ce4a8-198">Arasında ayırt `IConfigureNamedOptions` ve `IConfigureOptions` ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="ce4a8-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="ce4a8-200">`IOptionsMonitorCache` Değeri yeniden böylece seçenekleri örnekleri İzleyicisi'nde geçersiz kılar ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="ce4a8-201">Değerleri el ile de ile sunulan [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="ce4a8-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="ce4a8-202">[Temizle](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) yöntemi, tüm adlandırılmış örnekleri isteğe bağlı olarak yeniden oluşturulması olduğunda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="ce4a8-203">Başlatma sırasında erişilebilirlik seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ce4a8-203">Accessing options during startup</span></span>

<span data-ttu-id="ce4a8-204">`IOptions`kullanılabilir `Configure`, önce Hizmetleri yerleşiktir bu yana `Configure` yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-204">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="ce4a8-205">Bir hizmet sağlayıcısı oluşturulursa `ConfigureServices` seçeneklerine erişmek için onu içeremez olmayacaktır seçenekleri hizmet sağlayıcısı oluşturulduktan sonra sağlanan yapılandırmaları.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-205">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="ce4a8-206">Bu nedenle, bir tutarsız seçenekleri durum hizmet kayıtları sıralama nedeniyle olabilir.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-206">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="ce4a8-207">Seçenekleri genellikle yapılandırmasından yüklendiğinden bu yana yapılandırma hem de başlangıç kullanılabilir `Configure` ve `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-207">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="ce4a8-208">Başlatma sırasında yapılandırmayla örnekler için bkz: [uygulama başlangıç](xref:fundamentals/startup) konu.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-208">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="ce4a8-209">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="ce4a8-209">See also</span></span>

* [<span data-ttu-id="ce4a8-210">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ce4a8-210">Configuration</span></span>](xref:fundamentals/configuration/index)
