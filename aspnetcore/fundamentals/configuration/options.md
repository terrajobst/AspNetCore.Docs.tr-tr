---
title: ASP.NET Core için seçenek kalıbı
author: guardrex
description: ASP.NET Core uygulamalarında ilgili ayarların gruplarını temsil etmek için seçenekler deseninin nasıl kullanılacağını öğrenin.
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
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="2c58a-103">ASP.NET Core için seçenek kalıbı</span><span class="sxs-lookup"><span data-stu-id="2c58a-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="2c58a-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="2c58a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2c58a-105">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="2c58a-106">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="2c58a-107">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="2c58a-108">Uygulamanın farklı parçaları için &ndash; ayarlarının [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı değildir veya birbirlerine aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="2c58a-109">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="2c58a-110">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="2c58a-111">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2c58a-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="2c58a-112">Paket</span><span class="sxs-lookup"><span data-stu-id="2c58a-112">Package</span></span>

<span data-ttu-id="2c58a-113">[Microsoft. Extensions. Options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine ASP.NET Core uygulamalarında örtük olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="2c58a-114">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="2c58a-114">Options interfaces</span></span>

<span data-ttu-id="2c58a-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="2c58a-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="2c58a-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="2c58a-117">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="2c58a-117">Change notifications</span></span>
* [<span data-ttu-id="2c58a-118">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="2c58a-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="2c58a-119">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="2c58a-120">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="2c58a-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="2c58a-121">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="2c58a-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="2c58a-123">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="2c58a-124">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> alır ve önce tüm yapılandırmaların, ardından yapılandırma sonrası sonrasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="2c58a-125"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="2c58a-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="2c58a-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="2c58a-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="2c58a-128">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="2c58a-129"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="2c58a-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="2c58a-131">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="2c58a-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="2c58a-132"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="2c58a-133">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> önceki <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="2c58a-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="2c58a-134">Zaten <xref:Microsoft.Extensions.Options.IOptions%601> arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>tarafından sağlanmış senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="2c58a-135">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="2c58a-135">General options configuration</span></span>

<span data-ttu-id="2c58a-136">Genel Seçenekler yapılandırması örnek uygulamada &num;1 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="2c58a-137">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="2c58a-138">Aşağıdaki `MyOptions`sınıfında, `Option1` ve `Option2`iki özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="2c58a-139">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1`varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="2c58a-140">`Option2`, özelliği doğrudan başlatarak varsayılan değer kümesine sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="2c58a-141">`MyOptions` sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmayla bağlantılı olarak hizmet kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="2c58a-142">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="2c58a-143">Örneğin *appSettings. JSON* dosyası `option1` ve `option2`değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="2c58a-144">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="2c58a-145">Bir ayarlar dosyasından seçenek yapılandırması 'nı yüklemek için özel bir <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="2c58a-146">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="2c58a-147">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="2c58a-148">Basit seçeneklerin bir temsilciyle yapılandırılması, örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="2c58a-149">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-149">Use a delegate to set options values.</span></span> <span data-ttu-id="2c58a-150">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını kullanır (*modeller/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="2c58a-151">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="2c58a-152">`MyOptionsWithDelegateConfig`ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="2c58a-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2c58a-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="2c58a-154">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="2c58a-155">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="2c58a-156">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2c58a-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="2c58a-157">Her <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> çağrısı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="2c58a-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="2c58a-158">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="2c58a-159">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="2c58a-160">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="2c58a-161">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="2c58a-161">Suboptions configuration</span></span>

<span data-ttu-id="2c58a-162">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="2c58a-163">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="2c58a-164">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="2c58a-165">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her bir özellik `property[:sub-property:]`formun bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="2c58a-166">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan anahtar `Option1`bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="2c58a-167">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="2c58a-168">`MySubOptions` *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="2c58a-169">`GetSection` yöntemi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> ad alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="2c58a-170">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2`için anahtarlar içeren bir `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="2c58a-171">`MySubOptions` sınıfı, Seçenekler değerlerini tutmak için özellikleri, `SubOption1` ve `SubOption2`tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="2c58a-172">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="2c58a-173">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="2c58a-174">Seçeneklere ekleme</span><span class="sxs-lookup"><span data-stu-id="2c58a-174">Options injection</span></span>

<span data-ttu-id="2c58a-175">Seçenekler ekleme, örnek uygulamada &num;4 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-175">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="2c58a-176"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> içine ekle:</span><span class="sxs-lookup"><span data-stu-id="2c58a-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="2c58a-177">[@inject](xref:mvc/views/razor#inject) Razor yönergesi ile Razor sayfası veya MVC görünümü.</span><span class="sxs-lookup"><span data-stu-id="2c58a-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="2c58a-178">Bir sayfa veya görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="2c58a-178">A page or view model.</span></span>

<span data-ttu-id="2c58a-179">Örnek uygulamadan alınan aşağıdaki örnek, bir sayfa modeline <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="2c58a-180">Örnek uygulama, `@inject` yönergeyle `IOptionsMonitor<MyOptions>` nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="2c58a-181">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yapılır.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="2c58a-183">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="2c58a-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="2c58a-184">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme örnek uygulamada örnek &num;5 gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="2c58a-185"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>kullanarak, erişilen ve isteğin ömrü boyunca önbelleğe alınan istekler her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="2c58a-186">`IOptionsMonitor` ve `IOptionsSnapshot` arasındaki fark şudur:</span><span class="sxs-lookup"><span data-stu-id="2c58a-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="2c58a-187">`IOptionsMonitor`, geçerli seçenek değerlerini her zaman alan, özellikle de tek bağımlılıklarda yararlı olan bir [tek hizmettir](xref:fundamentals/dependency-injection#singleton) .</span><span class="sxs-lookup"><span data-stu-id="2c58a-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="2c58a-188">`IOptionsSnapshot` kapsamlı bir [hizmettir](xref:fundamentals/dependency-injection#scoped) ve `IOptionsSnapshot<T>` nesnesinin oluşturulduğu sırada seçeneklerin anlık görüntüsünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="2c58a-189">Seçenekler anlık görüntüleri geçici ve kapsamlı bağımlılıklarla kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="2c58a-190">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra yeni bir <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="2c58a-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="2c58a-191">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="2c58a-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="2c58a-192">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk `option1` ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="2c58a-193">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200`olacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="2c58a-194">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="2c58a-195">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="2c58a-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="2c58a-196">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="2c58a-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="2c58a-197"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ile adlandırılmış seçenekler, örnek uygulamada 6 &num;örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="2c58a-198">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-198">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="2c58a-199">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [\<TOptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="2c58a-200">Örnek uygulama, <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) adlı adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="2c58a-201">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="2c58a-202">`named_options_1` değerler, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="2c58a-203">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="2c58a-204">`Option1`için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="2c58a-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="2c58a-205">`MyOptions` sınıfı tarafından sağlanmış `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="2c58a-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="2c58a-206">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="2c58a-207">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="2c58a-208">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="2c58a-209">`Startup.ConfigureServices` yöntemine el ile aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c58a-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="2c58a-210">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="2c58a-211">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-211">All options are named instances.</span></span> <span data-ttu-id="2c58a-212">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty``Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="2c58a-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> Ayrıca <xref:Microsoft.Extensions.Options.IConfigureOptions%601>uygular.</span><span class="sxs-lookup"><span data-stu-id="2c58a-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="2c58a-214"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="2c58a-215">`null` adlandırılmış seçenek, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="2c58a-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="2c58a-216">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="2c58a-216">OptionsBuilder API</span></span>

<span data-ttu-id="2c58a-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>, `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="2c58a-218">`OptionsBuilder`, sonraki çağrıların tümünde olmak yerine ilk `AddOptions<TOptions>(string optionsName)` çağrısına yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="2c58a-219">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeler yalnızca `OptionsBuilder`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="2c58a-220">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c58a-220">Use DI services to configure options</span></span>

<span data-ttu-id="2c58a-221">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2c58a-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="2c58a-222">[\<TOptions > ' nin Options Builder](xref:Microsoft.Extensions.Options.OptionsBuilder`1)'da [yapılandırılması](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="2c58a-223">[\<bir TOptions > seçenek Oluşturucusu](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="2c58a-224"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan ve türü bir hizmet olarak kaydeden kendi türünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c58a-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="2c58a-225">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="2c58a-226">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="2c58a-227">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2c58a-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="2c58a-228">Seçenekler doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2c58a-228">Options validation</span></span>

<span data-ttu-id="2c58a-229">Seçenekler doğrulaması seçenekler yapılandırıldığında seçenekleri doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="2c58a-230">Seçenekler geçerliyse `true` döndüren ve geçerli değillerse `false` bir doğrulama yöntemiyle `Validate` çağırın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="2c58a-231">Önceki örnekte, adlandırılmış seçenekler örneği `optionalOptionsName`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="2c58a-232">Varsayılan Seçenekler örneği `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="2c58a-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="2c58a-233">Seçenekler örneği oluşturulduğunda doğrulama çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="2c58a-234">Seçenek Örneğinizde, ilk kez erişildiğinde doğrulamanın başarılı olması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c58a-235">Seçenekler, Seçenekler başlangıçta yapılandırıldıktan ve doğrulandıktan sonra seçeneklerindeki değişikliklere karşı koruma yapmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="2c58a-236">`Validate` yöntemi bir `Func<TOptions, bool>`kabul eder.</span><span class="sxs-lookup"><span data-stu-id="2c58a-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="2c58a-237">Doğrulamayı tamamen özelleştirmek için, şunları sağlayan `IValidateOptions<TOptions>`uygulayın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="2c58a-238">Birden çok seçenek türünün doğrulanması: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="2c58a-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="2c58a-239">Başka bir seçenek türüne bağlı olan doğrulama: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="2c58a-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="2c58a-240">`IValidateOptions` şunları doğrular:</span><span class="sxs-lookup"><span data-stu-id="2c58a-240">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="2c58a-241">Belirli bir adlandırılmış seçenekler örneği.</span><span class="sxs-lookup"><span data-stu-id="2c58a-241">A specific named options instance.</span></span>
* <span data-ttu-id="2c58a-242">`name` `null`tüm seçenekler.</span><span class="sxs-lookup"><span data-stu-id="2c58a-242">All options when `name` is `null`.</span></span>

<span data-ttu-id="2c58a-243">Arabirimin uygulanınızdan bir `ValidateOptionsResult` döndürün:</span><span class="sxs-lookup"><span data-stu-id="2c58a-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="2c58a-244">Veri ek açıklaması tabanlı doğrulama, `OptionsBuilder<TOptions>`<xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> yöntemi çağırarak [Microsoft. Extensions. Options. DataAnnotation](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) paketinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="2c58a-245">`Microsoft.Extensions.Options.DataAnnotations` ASP.NET Core uygulamalarda örtülü olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="2c58a-246">Eager doğrulaması (başlangıçta hızlı başarısız), gelecek bir sürüm için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-246">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="2c58a-247">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="2c58a-247">Options post-configuration</span></span>

<span data-ttu-id="2c58a-248">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="2c58a-249">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra yapılandırma sonrası çalıştırmalar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2c58a-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2c58a-251">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="2c58a-252">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="2c58a-252">Accessing options during startup</span></span>

<span data-ttu-id="2c58a-253"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure`kullanılabilir, çünkü Hizmetleri `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="2c58a-254">`Startup.ConfigureServices`<xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2c58a-255">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-255">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="2c58a-256">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-256">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="2c58a-257">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="2c58a-258">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="2c58a-259">Uygulamanın farklı parçaları için &ndash; ayarlarının [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı değildir veya birbirlerine aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="2c58a-260">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-260">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="2c58a-261">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-261">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="2c58a-262">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2c58a-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c58a-263">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2c58a-263">Prerequisites</span></span>

<span data-ttu-id="2c58a-264">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Options. configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="2c58a-265">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="2c58a-265">Options interfaces</span></span>

<span data-ttu-id="2c58a-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="2c58a-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="2c58a-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="2c58a-268">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="2c58a-268">Change notifications</span></span>
* [<span data-ttu-id="2c58a-269">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="2c58a-269">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="2c58a-270">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-270">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="2c58a-271">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="2c58a-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="2c58a-272">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="2c58a-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="2c58a-274">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="2c58a-275">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> alır ve önce tüm yapılandırmaların, ardından yapılandırma sonrası sonrasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="2c58a-276"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="2c58a-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="2c58a-278"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="2c58a-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="2c58a-279">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="2c58a-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="2c58a-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="2c58a-282">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="2c58a-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="2c58a-283"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="2c58a-284">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> önceki <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="2c58a-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="2c58a-285">Zaten <xref:Microsoft.Extensions.Options.IOptions%601> arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>tarafından sağlanmış senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="2c58a-286">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="2c58a-286">General options configuration</span></span>

<span data-ttu-id="2c58a-287">Genel Seçenekler yapılandırması örnek uygulamada &num;1 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="2c58a-288">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-288">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="2c58a-289">Aşağıdaki `MyOptions`sınıfında, `Option1` ve `Option2`iki özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="2c58a-290">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1`varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="2c58a-291">`Option2`, özelliği doğrudan başlatarak varsayılan değer kümesine sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="2c58a-292">`MyOptions` sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmayla bağlantılı olarak hizmet kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="2c58a-293">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="2c58a-294">Örneğin *appSettings. JSON* dosyası `option1` ve `option2`değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="2c58a-295">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="2c58a-296">Bir ayarlar dosyasından seçenek yapılandırması 'nı yüklemek için özel bir <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="2c58a-297">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="2c58a-298">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-298">Configure simple options with a delegate</span></span>

<span data-ttu-id="2c58a-299">Basit seçeneklerin bir temsilciyle yapılandırılması, örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="2c58a-300">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-300">Use a delegate to set options values.</span></span> <span data-ttu-id="2c58a-301">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını kullanır (*modeller/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="2c58a-302">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="2c58a-303">`MyOptionsWithDelegateConfig`ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="2c58a-304">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2c58a-304">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="2c58a-305">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-305">You can add multiple configuration providers.</span></span> <span data-ttu-id="2c58a-306">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="2c58a-307">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2c58a-307">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="2c58a-308">Her <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> çağrısı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="2c58a-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="2c58a-309">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="2c58a-310">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="2c58a-311">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="2c58a-312">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="2c58a-312">Suboptions configuration</span></span>

<span data-ttu-id="2c58a-313">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="2c58a-314">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="2c58a-315">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="2c58a-316">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her bir özellik `property[:sub-property:]`formun bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="2c58a-317">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan anahtar `Option1`bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="2c58a-318">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="2c58a-319">`MySubOptions` *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="2c58a-320">`GetSection` yöntemi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> ad alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="2c58a-321">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2`için anahtarlar içeren bir `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="2c58a-322">`MySubOptions` sınıfı, Seçenekler değerlerini tutmak için özellikleri, `SubOption1` ve `SubOption2`tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="2c58a-323">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="2c58a-324">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="2c58a-325">Seçeneklere ekleme</span><span class="sxs-lookup"><span data-stu-id="2c58a-325">Options injection</span></span>

<span data-ttu-id="2c58a-326">Seçenekler ekleme, örnek uygulamada &num;4 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-326">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="2c58a-327"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> içine ekle:</span><span class="sxs-lookup"><span data-stu-id="2c58a-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="2c58a-328">[@inject](xref:mvc/views/razor#inject) Razor yönergesi ile Razor sayfası veya MVC görünümü.</span><span class="sxs-lookup"><span data-stu-id="2c58a-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="2c58a-329">Bir sayfa veya görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="2c58a-329">A page or view model.</span></span>

<span data-ttu-id="2c58a-330">Örnek uygulamadan alınan aşağıdaki örnek, bir sayfa modeline <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="2c58a-331">Örnek uygulama, `@inject` yönergeyle `IOptionsMonitor<MyOptions>` nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="2c58a-332">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-332">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yapılır.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="2c58a-334">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="2c58a-334">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="2c58a-335">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme örnek uygulamada örnek &num;5 gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="2c58a-336"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>kullanarak, erişilen ve isteğin ömrü boyunca önbelleğe alınan istekler her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="2c58a-337">`IOptionsMonitor` ve `IOptionsSnapshot` arasındaki fark şudur:</span><span class="sxs-lookup"><span data-stu-id="2c58a-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="2c58a-338">`IOptionsMonitor`, geçerli seçenek değerlerini her zaman alan, özellikle de tek bağımlılıklarda yararlı olan bir [tek hizmettir](xref:fundamentals/dependency-injection#singleton) .</span><span class="sxs-lookup"><span data-stu-id="2c58a-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="2c58a-339">`IOptionsSnapshot` kapsamlı bir [hizmettir](xref:fundamentals/dependency-injection#scoped) ve `IOptionsSnapshot<T>` nesnesinin oluşturulduğu sırada seçeneklerin anlık görüntüsünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="2c58a-340">Seçenekler anlık görüntüleri geçici ve kapsamlı bağımlılıklarla kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-340">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="2c58a-341">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra yeni bir <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="2c58a-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="2c58a-342">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="2c58a-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="2c58a-343">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk `option1` ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="2c58a-344">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200`olacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="2c58a-345">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-345">Save the *appsettings.json* file.</span></span> <span data-ttu-id="2c58a-346">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="2c58a-346">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="2c58a-347">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="2c58a-347">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="2c58a-348"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ile adlandırılmış seçenekler, örnek uygulamada 6 &num;örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="2c58a-349">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-349">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="2c58a-350">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [\<TOptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="2c58a-351">Örnek uygulama, <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) adlı adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="2c58a-352">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-352">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="2c58a-353">`named_options_1` değerler, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="2c58a-354">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-354">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="2c58a-355">`Option1`için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="2c58a-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="2c58a-356">`MyOptions` sınıfı tarafından sağlanmış `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="2c58a-356">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="2c58a-357">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-357">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="2c58a-358">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="2c58a-359">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-359">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="2c58a-360">`Startup.ConfigureServices` yöntemine el ile aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c58a-360">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="2c58a-361">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-361">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="2c58a-362">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-362">All options are named instances.</span></span> <span data-ttu-id="2c58a-363">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty``Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="2c58a-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> Ayrıca <xref:Microsoft.Extensions.Options.IConfigureOptions%601>uygular.</span><span class="sxs-lookup"><span data-stu-id="2c58a-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="2c58a-365"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="2c58a-366">`null` adlandırılmış seçenek, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="2c58a-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="2c58a-367">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="2c58a-367">OptionsBuilder API</span></span>

<span data-ttu-id="2c58a-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>, `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="2c58a-369">`OptionsBuilder`, sonraki çağrıların tümünde olmak yerine ilk `AddOptions<TOptions>(string optionsName)` çağrısına yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="2c58a-370">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeler yalnızca `OptionsBuilder`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="2c58a-371">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c58a-371">Use DI services to configure options</span></span>

<span data-ttu-id="2c58a-372">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2c58a-372">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="2c58a-373">[\<TOptions > ' nin Options Builder](xref:Microsoft.Extensions.Options.OptionsBuilder`1)'da [yapılandırılması](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="2c58a-374">[\<bir TOptions > seçenek Oluşturucusu](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="2c58a-375"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan ve türü bir hizmet olarak kaydeden kendi türünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c58a-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="2c58a-376">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="2c58a-377">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="2c58a-378">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2c58a-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="2c58a-379">Seçenekler doğrulaması</span><span class="sxs-lookup"><span data-stu-id="2c58a-379">Options validation</span></span>

<span data-ttu-id="2c58a-380">Seçenekler doğrulaması seçenekler yapılandırıldığında seçenekleri doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-380">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="2c58a-381">Seçenekler geçerliyse `true` döndüren ve geçerli değillerse `false` bir doğrulama yöntemiyle `Validate` çağırın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="2c58a-382">Önceki örnekte, adlandırılmış seçenekler örneği `optionalOptionsName`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-382">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="2c58a-383">Varsayılan Seçenekler örneği `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="2c58a-383">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="2c58a-384">Seçenekler örneği oluşturulduğunda doğrulama çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-384">Validation runs when the options instance is created.</span></span> <span data-ttu-id="2c58a-385">Seçenek Örneğinizde, ilk kez erişildiğinde doğrulamanın başarılı olması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c58a-386">Seçenekler, Seçenekler başlangıçta yapılandırıldıktan ve doğrulandıktan sonra seçeneklerindeki değişikliklere karşı koruma yapmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="2c58a-387">`Validate` yöntemi bir `Func<TOptions, bool>`kabul eder.</span><span class="sxs-lookup"><span data-stu-id="2c58a-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="2c58a-388">Doğrulamayı tamamen özelleştirmek için, şunları sağlayan `IValidateOptions<TOptions>`uygulayın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="2c58a-389">Birden çok seçenek türünün doğrulanması: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="2c58a-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="2c58a-390">Başka bir seçenek türüne bağlı olan doğrulama: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="2c58a-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="2c58a-391">`IValidateOptions` şunları doğrular:</span><span class="sxs-lookup"><span data-stu-id="2c58a-391">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="2c58a-392">Belirli bir adlandırılmış seçenekler örneği.</span><span class="sxs-lookup"><span data-stu-id="2c58a-392">A specific named options instance.</span></span>
* <span data-ttu-id="2c58a-393">`name` `null`tüm seçenekler.</span><span class="sxs-lookup"><span data-stu-id="2c58a-393">All options when `name` is `null`.</span></span>

<span data-ttu-id="2c58a-394">Arabirimin uygulanınızdan bir `ValidateOptionsResult` döndürün:</span><span class="sxs-lookup"><span data-stu-id="2c58a-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="2c58a-395">Veri ek açıklaması tabanlı doğrulama, `OptionsBuilder<TOptions>`<xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> yöntemi çağırarak [Microsoft. Extensions. Options. DataAnnotation](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) paketinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="2c58a-396">`Microsoft.Extensions.Options.DataAnnotations`, [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="2c58a-397">Eager doğrulaması (başlangıçta hızlı başarısız), gelecek bir sürüm için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-397">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="2c58a-398">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="2c58a-398">Options post-configuration</span></span>

<span data-ttu-id="2c58a-399">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="2c58a-400">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra yapılandırma sonrası çalıştırmalar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2c58a-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2c58a-402">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="2c58a-403">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="2c58a-403">Accessing options during startup</span></span>

<span data-ttu-id="2c58a-404"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure`kullanılabilir, çünkü Hizmetleri `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="2c58a-405">`Startup.ConfigureServices`<xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2c58a-406">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-406">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="2c58a-407">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-407">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="2c58a-408">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="2c58a-409">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="2c58a-410">Uygulamanın farklı parçaları için &ndash; ayarlarının [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı değildir veya birbirlerine aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="2c58a-411">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-411">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="2c58a-412">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-412">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="2c58a-413">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2c58a-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c58a-414">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2c58a-414">Prerequisites</span></span>

<span data-ttu-id="2c58a-415">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Options. configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="2c58a-416">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="2c58a-416">Options interfaces</span></span>

<span data-ttu-id="2c58a-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="2c58a-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="2c58a-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="2c58a-419">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="2c58a-419">Change notifications</span></span>
* [<span data-ttu-id="2c58a-420">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="2c58a-420">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="2c58a-421">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-421">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="2c58a-422">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="2c58a-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="2c58a-423">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="2c58a-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="2c58a-425">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="2c58a-426">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> alır ve önce tüm yapılandırmaların, ardından yapılandırma sonrası sonrasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="2c58a-427"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="2c58a-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="2c58a-429"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="2c58a-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="2c58a-430">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="2c58a-431"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="2c58a-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="2c58a-433">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="2c58a-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="2c58a-434"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="2c58a-435">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> önceki <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="2c58a-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="2c58a-436">Zaten <xref:Microsoft.Extensions.Options.IOptions%601> arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>tarafından sağlanmış senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="2c58a-437">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="2c58a-437">General options configuration</span></span>

<span data-ttu-id="2c58a-438">Genel Seçenekler yapılandırması örnek uygulamada &num;1 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="2c58a-439">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-439">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="2c58a-440">Aşağıdaki `MyOptions`sınıfında, `Option1` ve `Option2`iki özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="2c58a-441">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1`varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="2c58a-442">`Option2`, özelliği doğrudan başlatarak varsayılan değer kümesine sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="2c58a-443">`MyOptions` sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmayla bağlantılı olarak hizmet kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="2c58a-444">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="2c58a-445">Örneğin *appSettings. JSON* dosyası `option1` ve `option2`değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="2c58a-446">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="2c58a-447">Bir ayarlar dosyasından seçenek yapılandırması 'nı yüklemek için özel bir <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="2c58a-448">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="2c58a-449">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-449">Configure simple options with a delegate</span></span>

<span data-ttu-id="2c58a-450">Basit seçeneklerin bir temsilciyle yapılandırılması, örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="2c58a-451">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-451">Use a delegate to set options values.</span></span> <span data-ttu-id="2c58a-452">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını kullanır (*modeller/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="2c58a-453">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="2c58a-454">`MyOptionsWithDelegateConfig`ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="2c58a-455">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2c58a-455">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="2c58a-456">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-456">You can add multiple configuration providers.</span></span> <span data-ttu-id="2c58a-457">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="2c58a-458">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2c58a-458">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="2c58a-459">Her <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> çağrısı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="2c58a-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="2c58a-460">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="2c58a-461">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c58a-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="2c58a-462">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="2c58a-463">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="2c58a-463">Suboptions configuration</span></span>

<span data-ttu-id="2c58a-464">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="2c58a-465">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="2c58a-466">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="2c58a-467">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her bir özellik `property[:sub-property:]`formun bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="2c58a-468">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan anahtar `Option1`bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="2c58a-469">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="2c58a-470">`MySubOptions` *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="2c58a-471">`GetSection` yöntemi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> ad alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="2c58a-472">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2`için anahtarlar içeren bir `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="2c58a-473">`MySubOptions` sınıfı, Seçenekler değerlerini tutmak için özellikleri, `SubOption1` ve `SubOption2`tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="2c58a-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="2c58a-474">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="2c58a-475">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="2c58a-476">Bir görünüm modeli veya doğrudan görünüm ekleme ile belirtilen seçenekler</span><span class="sxs-lookup"><span data-stu-id="2c58a-476">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="2c58a-477">Bir görünüm modeli veya doğrudan görünüm ekleme ile sunulan seçenekler örnek uygulamada &num;4 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="2c58a-478">Seçenekler, bir görünüm modelinde veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ekleme tarafından doğrudan bir görünüme (*Pages/Index. cshtml. cs*) sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="2c58a-479">Örnek uygulama, `@inject` yönergeyle `IOptionsMonitor<MyOptions>` nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="2c58a-480">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-480">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yapılır.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="2c58a-482">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="2c58a-482">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="2c58a-483">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme örnek uygulamada örnek &num;5 gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="2c58a-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, minimum işleme yüküyle yeniden yükleme seçeneklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="2c58a-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="2c58a-485">Seçenekler erişildiğinde ve isteğin ömrü boyunca önbelleğe alındığında her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="2c58a-486">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra yeni bir <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="2c58a-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="2c58a-487">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="2c58a-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="2c58a-488">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk `option1` ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="2c58a-489">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200`olacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="2c58a-490">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-490">Save the *appsettings.json* file.</span></span> <span data-ttu-id="2c58a-491">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="2c58a-491">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="2c58a-492">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="2c58a-492">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="2c58a-493"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ile adlandırılmış seçenekler, örnek uygulamada 6 &num;örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="2c58a-494">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-494">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="2c58a-495">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [\<TOptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="2c58a-496">Örnek uygulama, <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) adlı adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="2c58a-497">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="2c58a-497">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="2c58a-498">`named_options_1` değerler, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="2c58a-499">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="2c58a-499">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="2c58a-500">`Option1`için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="2c58a-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="2c58a-501">`MyOptions` sınıfı tarafından sağlanmış `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="2c58a-501">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="2c58a-502">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c58a-502">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="2c58a-503">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="2c58a-504">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-504">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="2c58a-505">`Startup.ConfigureServices` yöntemine el ile aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c58a-505">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="2c58a-506">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-506">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="2c58a-507">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-507">All options are named instances.</span></span> <span data-ttu-id="2c58a-508">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty``Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="2c58a-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> Ayrıca <xref:Microsoft.Extensions.Options.IConfigureOptions%601>uygular.</span><span class="sxs-lookup"><span data-stu-id="2c58a-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="2c58a-510"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="2c58a-511">`null` adlandırılmış seçenek, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="2c58a-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="2c58a-512">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="2c58a-512">OptionsBuilder API</span></span>

<span data-ttu-id="2c58a-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>, `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="2c58a-514">`OptionsBuilder`, sonraki çağrıların tümünde olmak yerine ilk `AddOptions<TOptions>(string optionsName)` çağrısına yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="2c58a-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="2c58a-515">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeler yalnızca `OptionsBuilder`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="2c58a-516">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c58a-516">Use DI services to configure options</span></span>

<span data-ttu-id="2c58a-517">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2c58a-517">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="2c58a-518">[\<TOptions > ' nin Options Builder](xref:Microsoft.Extensions.Options.OptionsBuilder`1)'da [yapılandırılması](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="2c58a-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="2c58a-519">[\<bir TOptions > seçenek Oluşturucusu](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="2c58a-520"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan ve türü bir hizmet olarak kaydeden kendi türünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c58a-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="2c58a-521">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="2c58a-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="2c58a-522">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="2c58a-523">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2c58a-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="2c58a-524">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="2c58a-524">Options post-configuration</span></span>

<span data-ttu-id="2c58a-525">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="2c58a-526">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra yapılandırma sonrası çalıştırmalar:</span><span class="sxs-lookup"><span data-stu-id="2c58a-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2c58a-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2c58a-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2c58a-528">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="2c58a-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="2c58a-529">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="2c58a-529">Accessing options during startup</span></span>

<span data-ttu-id="2c58a-530"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure`kullanılabilir, çünkü Hizmetleri `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2c58a-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="2c58a-531">`Startup.ConfigureServices`<xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="2c58a-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2c58a-532">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c58a-532">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2c58a-533">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2c58a-533">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
