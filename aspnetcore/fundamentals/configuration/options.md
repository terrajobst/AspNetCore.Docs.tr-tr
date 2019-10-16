---
title: ASP.NET Core için seçenek kalıbı
author: guardrex
description: ASP.NET Core uygulamalarında ilgili ayarların gruplarını temsil etmek için seçenekler deseninin nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: eb0b7f3f4596b63cf3142017c5c5fe4923aac3a4
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378736"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="ccd28-103">ASP.NET Core için seçenek kalıbı</span><span class="sxs-lookup"><span data-stu-id="ccd28-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="ccd28-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="ccd28-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ccd28-105">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="ccd28-106">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ccd28-107">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ccd28-108">@No__t, uygulamanın farklı bölümleri için 1 ayarların [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı veya bir diğeriyle bağlantılı değildir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ccd28-109">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="ccd28-110">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="ccd28-111">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ccd28-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="ccd28-112">Paket</span><span class="sxs-lookup"><span data-stu-id="ccd28-112">Package</span></span>

<span data-ttu-id="ccd28-113">[Microsoft. Extensions. Options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine ASP.NET Core uygulamalarında örtük olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="ccd28-114">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="ccd28-114">Options interfaces</span></span>

<span data-ttu-id="ccd28-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="ccd28-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="ccd28-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="ccd28-117">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="ccd28-117">Change notifications</span></span>
* [<span data-ttu-id="ccd28-118">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="ccd28-119">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="ccd28-120">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="ccd28-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="ccd28-121">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırması oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="ccd28-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601>, yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="ccd28-123">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="ccd28-124">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ' ı ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> ' ı alır ve önce tüm yapılandırmaları çalıştırır, sonrasında yapılandırma sonrası.</span><span class="sxs-lookup"><span data-stu-id="ccd28-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="ccd28-125">@No__t-0 ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="ccd28-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="ccd28-127">@No__t-0, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="ccd28-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="ccd28-128">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="ccd28-129">@No__t-0 yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="ccd28-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="ccd28-131">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ccd28-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="ccd28-132"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="ccd28-133">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ' in önceki senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ccd28-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="ccd28-134">@No__t-1 arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından sunulan senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="ccd28-135">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ccd28-135">General options configuration</span></span>

<span data-ttu-id="ccd28-136">Genel Seçenekler yapılandırması örnek uygulamada &num;1 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="ccd28-137">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ccd28-138">Aşağıdaki `MyOptions` sınıfının iki özelliği vardır, `Option1` ve `Option2`.</span><span class="sxs-lookup"><span data-stu-id="ccd28-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ccd28-139">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1` ' ın varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ccd28-140">`Option2`, doğrudan Özellik başlatılarak ayarlanmış bir varsayılan değere sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ccd28-141">@No__t-0 sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ile hizmet kapsayıcısına eklenir ve yapılandırmaya bağlanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ccd28-142">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ccd28-143">Örneğin *appSettings. JSON* dosyası `option1` ve `option2` değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="ccd28-144">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="ccd28-145">Bir ayar dosyasından seçenek yapılandırmasını yüklemek için özel <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="ccd28-146">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ccd28-147">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="ccd28-148">Basit seçeneklerin bir temsilciyle yapılandırılması örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="ccd28-149">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-149">Use a delegate to set options values.</span></span> <span data-ttu-id="ccd28-150">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını (*modeller/MyOptionsWithDelegateConfig. cs*) kullanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ccd28-151">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ccd28-152">@No__t-0 ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ccd28-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ccd28-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ccd28-154">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="ccd28-155">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="ccd28-156">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ccd28-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="ccd28-157">@No__t-0 ' a yapılan her çağrı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="ccd28-158">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ccd28-159">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ccd28-160">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ccd28-161">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ccd28-161">Suboptions configuration</span></span>

<span data-ttu-id="ccd28-162">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="ccd28-163">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="ccd28-164">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ccd28-165">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her özellik `property[:sub-property:]` biçiminde bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ccd28-166">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan `Option1` anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ccd28-167">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ccd28-168">@No__t-0 ' i *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ccd28-169">@No__t-0 genişletme yöntemi, [Microsoft. Extensions. Options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-169">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="ccd28-170">`Microsoft.Extensions.Options.ConfigurationExtensions` ASP.NET Core uygulamalarda örtülü olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-170">`Microsoft.Extensions.Options.ConfigurationExtensions` is implicitly referenced in ASP.NET Core apps.</span></span>

<span data-ttu-id="ccd28-171">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2` anahtarlarına sahip `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-171">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ccd28-172">@No__t-0 sınıfı, Seçenekler değerlerini tutmak için, `SubOption1` ve `SubOption2` özelliklerini tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-172">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ccd28-173">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-173">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ccd28-174">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-174">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ccd28-175">Bir görünüm modeli veya doğrudan görünüm ekleme ile belirtilen seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-175">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ccd28-176">Bir görünüm modeli veya doğrudan görünüm ekleme ile sunulan seçenekler örnek uygulamada &num;4 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-176">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="ccd28-177">Seçenekler bir görünüm modelinde veya ekleme <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından doğrudan bir görünüme (*Sayfalar/Index. cshtml. cs*) sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-177">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ccd28-178">Örnek uygulama, `@inject` yönergesiyle `IOptionsMonitor<MyOptions>` ' nın nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-178">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="ccd28-179">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-179">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yaparak.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ccd28-181">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="ccd28-181">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ccd28-182">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme, örnek uygulamada &num;5 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-182">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="ccd28-183"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, minimum işleme yüküyle yeniden yükleme seçeneklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-183"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="ccd28-184">Seçenekler erişildiğinde ve isteğin ömrü boyunca önbelleğe alındığında her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-184">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ccd28-185">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra New <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ' ın nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="ccd28-185">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ccd28-186">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="ccd28-186">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ccd28-187">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk @no__t 0 ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-187">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ccd28-188">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200` ' e değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-188">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ccd28-189">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-189">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ccd28-190">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="ccd28-190">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ccd28-191">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="ccd28-191">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ccd28-192">@No__t-0 ile adlandırılmış seçenekler desteği örnek uygulamada &num;6 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-192">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="ccd28-193">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-193">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ccd28-194">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [configurenamedooptıons @ No__t-2toptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-194">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ccd28-195">Örnek uygulama <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) ile adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-195">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ccd28-196">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-196">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ccd28-197">`named_options_1` değerleri, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-197">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ccd28-198">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-198">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ccd28-199">@No__t-2 için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="ccd28-199">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ccd28-200">@No__t-1 sınıfı tarafından sunulan `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="ccd28-200">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="ccd28-201">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-201">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="ccd28-202">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-202">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="ccd28-203">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-203">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="ccd28-204">Aşağıdaki kodu `Startup.ConfigureServices` yöntemine el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ccd28-204">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ccd28-205">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-205">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ccd28-206">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-206">All options are named instances.</span></span> <span data-ttu-id="ccd28-207">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty` olan `Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-207">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ccd28-208"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ' i de uygular.</span><span class="sxs-lookup"><span data-stu-id="ccd28-208"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="ccd28-209">@No__t-0 ' ın varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-209">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="ccd28-210">@No__t-0 adlandırılmış seçeneği, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> Bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="ccd28-210">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="ccd28-211">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="ccd28-211">OptionsBuilder API</span></span>

<span data-ttu-id="ccd28-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="ccd28-213">`OptionsBuilder` @no__t, sonraki çağrıların tümünde görünmesini yerine yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-213">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="ccd28-214">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeleri yalnızca `OptionsBuilder` aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-214">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="ccd28-215">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="ccd28-215">Use DI services to configure options</span></span>

<span data-ttu-id="ccd28-216">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ccd28-216">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="ccd28-217">[Options Builder @ no__t-2TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1)üzerinde [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-217">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="ccd28-218">Options [Builder @ no__t-1TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-218">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="ccd28-219">@No__t-0 veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan kendi türünü oluşturun ve türü bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-219">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="ccd28-220">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-220">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="ccd28-221">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-221">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="ccd28-222">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ' i kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-222">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="ccd28-223">Seçenekler doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ccd28-223">Options validation</span></span>

<span data-ttu-id="ccd28-224">Seçenekler doğrulaması seçenekler yapılandırıldığında seçenekleri doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-224">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="ccd28-225">Seçenekler geçerliyse `true` döndüren bir doğrulama yöntemiyle `Validate` ' i çağırın ve geçerli değillerse-2 ' ye @no__t:</span><span class="sxs-lookup"><span data-stu-id="ccd28-225">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="ccd28-226">Yukarıdaki örnek, adlandırılmış seçenekler örneğini `optionalOptionsName` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-226">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="ccd28-227">Varsayılan Seçenekler örneği `Options.DefaultName` ' dır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-227">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="ccd28-228">Seçenekler örneği oluşturulduğunda doğrulama çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-228">Validation runs when the options instance is created.</span></span> <span data-ttu-id="ccd28-229">Seçenek Örneğinizde, ilk kez erişildiğinde doğrulamanın başarılı olması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-229">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ccd28-230">Seçenekler, Seçenekler başlangıçta yapılandırıldıktan ve doğrulandıktan sonra seçeneklerindeki değişikliklere karşı koruma yapmaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-230">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="ccd28-231">@No__t-0 yöntemi `Func<TOptions, bool>` kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ccd28-231">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="ccd28-232">Doğrulamayı tamamen özelleştirmek için, şunları sağlayan `IValidateOptions<TOptions>` ' ı uygulayın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-232">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="ccd28-233">Birden çok seçenek türü doğrulaması: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="ccd28-233">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="ccd28-234">Başka bir seçenek türüne bağlı olan doğrulama: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="ccd28-234">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="ccd28-235">`IValidateOptions` şunları doğrular:</span><span class="sxs-lookup"><span data-stu-id="ccd28-235">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="ccd28-236">Belirli bir adlandırılmış seçenekler örneği.</span><span class="sxs-lookup"><span data-stu-id="ccd28-236">A specific named options instance.</span></span>
* <span data-ttu-id="ccd28-237">@No__t-0 `null` olduğunda tüm seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-237">All options when `name` is `null`.</span></span>

<span data-ttu-id="ccd28-238">Arabirimin uygulanınızdan bir `ValidateOptionsResult` döndürün:</span><span class="sxs-lookup"><span data-stu-id="ccd28-238">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="ccd28-239">Veri ek açıklaması tabanlı doğrulama, `OptionsBuilder<TOptions>` ' de <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> yöntemi çağırarak [Microsoft. Extensions. Options. DataAnnotation](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) paketinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-239">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="ccd28-240">`Microsoft.Extensions.Options.DataAnnotations` ASP.NET Core uygulamalarda örtülü olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-240">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="ccd28-241">Eager doğrulaması (başlangıçta hızlı başarısız), gelecek bir sürüm için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-241">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="ccd28-242">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-242">Options post-configuration</span></span>

<span data-ttu-id="ccd28-243">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-243">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="ccd28-244">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırması oluştuktan sonra yapılandırma sonrası çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-244">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ccd28-245"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-245"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ccd28-246">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-246">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="ccd28-247">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="ccd28-247">Accessing options during startup</span></span>

<span data-ttu-id="ccd28-248"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure` ' de kullanılabilir. çünkü hizmetler `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-248"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="ccd28-249">@No__t-2 ' de <xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-249">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ccd28-250">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-250">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ccd28-251">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-251">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="ccd28-252">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-252">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ccd28-253">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-253">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ccd28-254">@No__t, uygulamanın farklı bölümleri için 1 ayarların [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı veya bir diğeriyle bağlantılı değildir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-254">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ccd28-255">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-255">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="ccd28-256">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-256">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="ccd28-257">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ccd28-257">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccd28-258">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="ccd28-258">Prerequisites</span></span>

<span data-ttu-id="ccd28-259">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Options. configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-259">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="ccd28-260">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="ccd28-260">Options interfaces</span></span>

<span data-ttu-id="ccd28-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="ccd28-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="ccd28-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="ccd28-263">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="ccd28-263">Change notifications</span></span>
* [<span data-ttu-id="ccd28-264">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-264">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="ccd28-265">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-265">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="ccd28-266">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="ccd28-266">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="ccd28-267">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırması oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-267">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="ccd28-268"><xref:Microsoft.Extensions.Options.IOptionsFactory%601>, yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-268"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="ccd28-269">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-269">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="ccd28-270">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ' ı ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> ' ı alır ve önce tüm yapılandırmaları çalıştırır, sonrasında yapılandırma sonrası.</span><span class="sxs-lookup"><span data-stu-id="ccd28-270">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="ccd28-271">@No__t-0 ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-271">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="ccd28-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="ccd28-273">@No__t-0, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="ccd28-273">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="ccd28-274">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-274">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="ccd28-275">@No__t-0 yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-275">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="ccd28-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="ccd28-277">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ccd28-277">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="ccd28-278"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-278"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="ccd28-279">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ' in önceki senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ccd28-279">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="ccd28-280">@No__t-1 arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından sunulan senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-280">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="ccd28-281">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ccd28-281">General options configuration</span></span>

<span data-ttu-id="ccd28-282">Genel Seçenekler yapılandırması örnek uygulamada &num;1 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-282">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="ccd28-283">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-283">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ccd28-284">Aşağıdaki `MyOptions` sınıfının iki özelliği vardır, `Option1` ve `Option2`.</span><span class="sxs-lookup"><span data-stu-id="ccd28-284">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ccd28-285">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1` ' ın varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-285">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ccd28-286">`Option2`, doğrudan Özellik başlatılarak ayarlanmış bir varsayılan değere sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-286">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ccd28-287">@No__t-0 sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ile hizmet kapsayıcısına eklenir ve yapılandırmaya bağlanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-287">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ccd28-288">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-288">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ccd28-289">Örneğin *appSettings. JSON* dosyası `option1` ve `option2` değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-289">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="ccd28-290">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-290">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="ccd28-291">Bir ayar dosyasından seçenek yapılandırmasını yüklemek için özel <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-291">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="ccd28-292">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-292">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ccd28-293">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-293">Configure simple options with a delegate</span></span>

<span data-ttu-id="ccd28-294">Basit seçeneklerin bir temsilciyle yapılandırılması örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-294">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="ccd28-295">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-295">Use a delegate to set options values.</span></span> <span data-ttu-id="ccd28-296">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını (*modeller/MyOptionsWithDelegateConfig. cs*) kullanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-296">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ccd28-297">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-297">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ccd28-298">@No__t-0 ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-298">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ccd28-299">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ccd28-299">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ccd28-300">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-300">You can add multiple configuration providers.</span></span> <span data-ttu-id="ccd28-301">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-301">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="ccd28-302">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ccd28-302">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="ccd28-303">@No__t-0 ' a yapılan her çağrı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-303">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="ccd28-304">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-304">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ccd28-305">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-305">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ccd28-306">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-306">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ccd28-307">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ccd28-307">Suboptions configuration</span></span>

<span data-ttu-id="ccd28-308">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-308">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="ccd28-309">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-309">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="ccd28-310">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-310">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ccd28-311">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her özellik `property[:sub-property:]` biçiminde bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-311">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ccd28-312">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan `Option1` anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-312">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ccd28-313">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-313">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ccd28-314">@No__t-0 ' i *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-314">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ccd28-315">@No__t-0 genişletme yöntemi, [Microsoft. Extensions. Options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-315">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="ccd28-316">Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya üzeri) kullanıyorsa, paket otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-316">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="ccd28-317">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2` anahtarlarına sahip `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-317">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ccd28-318">@No__t-0 sınıfı, Seçenekler değerlerini tutmak için, `SubOption1` ve `SubOption2` özelliklerini tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-318">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ccd28-319">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-319">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ccd28-320">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-320">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ccd28-321">Bir görünüm modeli veya doğrudan görünüm ekleme ile belirtilen seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-321">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ccd28-322">Bir görünüm modeli veya doğrudan görünüm ekleme ile sunulan seçenekler örnek uygulamada &num;4 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-322">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="ccd28-323">Seçenekler bir görünüm modelinde veya ekleme <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından doğrudan bir görünüme (*Sayfalar/Index. cshtml. cs*) sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-323">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ccd28-324">Örnek uygulama, `@inject` yönergesiyle `IOptionsMonitor<MyOptions>` ' nın nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-324">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="ccd28-325">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-325">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yaparak.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ccd28-327">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="ccd28-327">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ccd28-328">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme, örnek uygulamada &num;5 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-328">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="ccd28-329"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, minimum işleme yüküyle yeniden yükleme seçeneklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-329"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="ccd28-330">Seçenekler erişildiğinde ve isteğin ömrü boyunca önbelleğe alındığında her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-330">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ccd28-331">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra New <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ' ın nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="ccd28-331">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ccd28-332">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="ccd28-332">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ccd28-333">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk @no__t 0 ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-333">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ccd28-334">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200` ' e değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-334">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ccd28-335">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-335">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ccd28-336">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="ccd28-336">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ccd28-337">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="ccd28-337">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ccd28-338">@No__t-0 ile adlandırılmış seçenekler desteği örnek uygulamada &num;6 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-338">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="ccd28-339">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-339">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ccd28-340">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [configurenamedooptıons @ No__t-2toptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-340">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ccd28-341">Örnek uygulama <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) ile adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-341">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ccd28-342">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-342">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ccd28-343">`named_options_1` değerleri, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-343">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ccd28-344">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-344">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ccd28-345">@No__t-2 için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="ccd28-345">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ccd28-346">@No__t-1 sınıfı tarafından sunulan `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="ccd28-346">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="ccd28-347">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-347">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="ccd28-348">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-348">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="ccd28-349">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-349">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="ccd28-350">Aşağıdaki kodu `Startup.ConfigureServices` yöntemine el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ccd28-350">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ccd28-351">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-351">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ccd28-352">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-352">All options are named instances.</span></span> <span data-ttu-id="ccd28-353">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty` olan `Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-353">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ccd28-354"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ' i de uygular.</span><span class="sxs-lookup"><span data-stu-id="ccd28-354"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="ccd28-355">@No__t-0 ' ın varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-355">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="ccd28-356">@No__t-0 adlandırılmış seçeneği, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> Bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="ccd28-356">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="ccd28-357">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="ccd28-357">OptionsBuilder API</span></span>

<span data-ttu-id="ccd28-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="ccd28-359">`OptionsBuilder` @no__t, sonraki çağrıların tümünde görünmesini yerine yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-359">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="ccd28-360">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeleri yalnızca `OptionsBuilder` aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-360">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="ccd28-361">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="ccd28-361">Use DI services to configure options</span></span>

<span data-ttu-id="ccd28-362">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ccd28-362">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="ccd28-363">[Options Builder @ no__t-2TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1)üzerinde [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-363">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="ccd28-364">Options [Builder @ no__t-1TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-364">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="ccd28-365">@No__t-0 veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan kendi türünü oluşturun ve türü bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-365">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="ccd28-366">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-366">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="ccd28-367">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-367">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="ccd28-368">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ' i kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-368">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="ccd28-369">Seçenekler doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ccd28-369">Options validation</span></span>

<span data-ttu-id="ccd28-370">Seçenekler doğrulaması seçenekler yapılandırıldığında seçenekleri doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-370">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="ccd28-371">Seçenekler geçerliyse `true` döndüren bir doğrulama yöntemiyle `Validate` ' i çağırın ve geçerli değillerse-2 ' ye @no__t:</span><span class="sxs-lookup"><span data-stu-id="ccd28-371">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="ccd28-372">Yukarıdaki örnek, adlandırılmış seçenekler örneğini `optionalOptionsName` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-372">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="ccd28-373">Varsayılan Seçenekler örneği `Options.DefaultName` ' dır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-373">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="ccd28-374">Seçenekler örneği oluşturulduğunda doğrulama çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-374">Validation runs when the options instance is created.</span></span> <span data-ttu-id="ccd28-375">Seçenek Örneğinizde, ilk kez erişildiğinde doğrulamanın başarılı olması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-375">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ccd28-376">Seçenekler, Seçenekler başlangıçta yapılandırıldıktan ve doğrulandıktan sonra seçeneklerindeki değişikliklere karşı koruma yapmaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-376">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="ccd28-377">@No__t-0 yöntemi `Func<TOptions, bool>` kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ccd28-377">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="ccd28-378">Doğrulamayı tamamen özelleştirmek için, şunları sağlayan `IValidateOptions<TOptions>` ' ı uygulayın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-378">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="ccd28-379">Birden çok seçenek türü doğrulaması: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="ccd28-379">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="ccd28-380">Başka bir seçenek türüne bağlı olan doğrulama: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="ccd28-380">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="ccd28-381">`IValidateOptions` şunları doğrular:</span><span class="sxs-lookup"><span data-stu-id="ccd28-381">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="ccd28-382">Belirli bir adlandırılmış seçenekler örneği.</span><span class="sxs-lookup"><span data-stu-id="ccd28-382">A specific named options instance.</span></span>
* <span data-ttu-id="ccd28-383">@No__t-0 `null` olduğunda tüm seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-383">All options when `name` is `null`.</span></span>

<span data-ttu-id="ccd28-384">Arabirimin uygulanınızdan bir `ValidateOptionsResult` döndürün:</span><span class="sxs-lookup"><span data-stu-id="ccd28-384">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="ccd28-385">Veri ek açıklaması tabanlı doğrulama, `OptionsBuilder<TOptions>` ' de <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> yöntemi çağırarak [Microsoft. Extensions. Options. DataAnnotation](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) paketinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-385">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="ccd28-386">`Microsoft.Extensions.Options.DataAnnotations`, [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e dahildir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-386">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="ccd28-387">Eager doğrulaması (başlangıçta hızlı başarısız), gelecek bir sürüm için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-387">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="ccd28-388">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-388">Options post-configuration</span></span>

<span data-ttu-id="ccd28-389">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-389">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="ccd28-390">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırması oluştuktan sonra yapılandırma sonrası çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-390">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ccd28-391"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-391"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ccd28-392">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-392">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="ccd28-393">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="ccd28-393">Accessing options during startup</span></span>

<span data-ttu-id="ccd28-394"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure` ' de kullanılabilir. çünkü hizmetler `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-394"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="ccd28-395">@No__t-2 ' de <xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-395">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ccd28-396">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-396">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ccd28-397">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-397">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="ccd28-398">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-398">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ccd28-399">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-399">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ccd28-400">@No__t, uygulamanın farklı bölümleri için 1 ayarların [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı veya bir diğeriyle bağlantılı değildir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-400">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ccd28-401">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-401">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="ccd28-402">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-402">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="ccd28-403">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ccd28-403">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccd28-404">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="ccd28-404">Prerequisites</span></span>

<span data-ttu-id="ccd28-405">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Options. configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-405">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="ccd28-406">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="ccd28-406">Options interfaces</span></span>

<span data-ttu-id="ccd28-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="ccd28-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="ccd28-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="ccd28-409">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="ccd28-409">Change notifications</span></span>
* [<span data-ttu-id="ccd28-410">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-410">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="ccd28-411">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-411">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="ccd28-412">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="ccd28-412">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="ccd28-413">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırması oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-413">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="ccd28-414"><xref:Microsoft.Extensions.Options.IOptionsFactory%601>, yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-414"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="ccd28-415">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-415">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="ccd28-416">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ' ı ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> ' ı alır ve önce tüm yapılandırmaları çalıştırır, sonrasında yapılandırma sonrası.</span><span class="sxs-lookup"><span data-stu-id="ccd28-416">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="ccd28-417">@No__t-0 ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-417">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="ccd28-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="ccd28-419">@No__t-0, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="ccd28-419">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="ccd28-420">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-420">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="ccd28-421">@No__t-0 yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-421">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="ccd28-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="ccd28-423">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="ccd28-423">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="ccd28-424"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-424"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="ccd28-425">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ' in önceki senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ccd28-425">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="ccd28-426">@No__t-1 arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından sunulan senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-426">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="ccd28-427">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ccd28-427">General options configuration</span></span>

<span data-ttu-id="ccd28-428">Genel Seçenekler yapılandırması örnek uygulamada &num;1 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-428">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="ccd28-429">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-429">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ccd28-430">Aşağıdaki `MyOptions` sınıfının iki özelliği vardır, `Option1` ve `Option2`.</span><span class="sxs-lookup"><span data-stu-id="ccd28-430">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ccd28-431">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1` ' ın varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-431">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ccd28-432">`Option2`, doğrudan Özellik başlatılarak ayarlanmış bir varsayılan değere sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-432">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ccd28-433">@No__t-0 sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ile hizmet kapsayıcısına eklenir ve yapılandırmaya bağlanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-433">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ccd28-434">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-434">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ccd28-435">Örneğin *appSettings. JSON* dosyası `option1` ve `option2` değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-435">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="ccd28-436">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-436">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="ccd28-437">Bir ayar dosyasından seçenek yapılandırmasını yüklemek için özel <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-437">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="ccd28-438">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-438">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ccd28-439">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-439">Configure simple options with a delegate</span></span>

<span data-ttu-id="ccd28-440">Basit seçeneklerin bir temsilciyle yapılandırılması örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-440">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="ccd28-441">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-441">Use a delegate to set options values.</span></span> <span data-ttu-id="ccd28-442">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını (*modeller/MyOptionsWithDelegateConfig. cs*) kullanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-442">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ccd28-443">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-443">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ccd28-444">@No__t-0 ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-444">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ccd28-445">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ccd28-445">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ccd28-446">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-446">You can add multiple configuration providers.</span></span> <span data-ttu-id="ccd28-447">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-447">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="ccd28-448">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ccd28-448">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="ccd28-449">@No__t-0 ' a yapılan her çağrı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-449">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="ccd28-450">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-450">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ccd28-451">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ccd28-451">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ccd28-452">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-452">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ccd28-453">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ccd28-453">Suboptions configuration</span></span>

<span data-ttu-id="ccd28-454">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-454">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="ccd28-455">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-455">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="ccd28-456">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-456">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ccd28-457">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her özellik `property[:sub-property:]` biçiminde bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-457">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ccd28-458">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan `Option1` anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-458">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ccd28-459">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-459">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="ccd28-460">@No__t-0 ' i *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-460">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ccd28-461">@No__t-0 genişletme yöntemi, [Microsoft. Extensions. Options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-461">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="ccd28-462">Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya üzeri) kullanıyorsa, paket otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-462">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="ccd28-463">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2` anahtarlarına sahip `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-463">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ccd28-464">@No__t-0 sınıfı, Seçenekler değerlerini tutmak için, `SubOption1` ve `SubOption2` özelliklerini tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="ccd28-464">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ccd28-465">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-465">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ccd28-466">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-466">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ccd28-467">Bir görünüm modeli veya doğrudan görünüm ekleme ile belirtilen seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-467">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ccd28-468">Bir görünüm modeli veya doğrudan görünüm ekleme ile sunulan seçenekler örnek uygulamada &num;4 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-468">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="ccd28-469">Seçenekler bir görünüm modelinde veya ekleme <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından doğrudan bir görünüme (*Sayfalar/Index. cshtml. cs*) sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-469">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ccd28-470">Örnek uygulama, `@inject` yönergesiyle `IOptionsMonitor<MyOptions>` ' nın nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-470">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="ccd28-471">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-471">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yaparak.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ccd28-473">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="ccd28-473">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ccd28-474">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme, örnek uygulamada &num;5 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-474">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="ccd28-475"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, minimum işleme yüküyle yeniden yükleme seçeneklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="ccd28-475"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="ccd28-476">Seçenekler erişildiğinde ve isteğin ömrü boyunca önbelleğe alındığında her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-476">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ccd28-477">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra New <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ' ın nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="ccd28-477">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ccd28-478">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="ccd28-478">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ccd28-479">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk @no__t 0 ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-479">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ccd28-480">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200` ' e değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-480">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ccd28-481">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-481">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ccd28-482">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="ccd28-482">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ccd28-483">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="ccd28-483">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ccd28-484">@No__t-0 ile adlandırılmış seçenekler desteği örnek uygulamada &num;6 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-484">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="ccd28-485">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-485">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ccd28-486">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [configurenamedooptıons @ No__t-2toptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-486">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ccd28-487">Örnek uygulama <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) ile adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-487">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ccd28-488">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="ccd28-488">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ccd28-489">`named_options_1` değerleri, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-489">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ccd28-490">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-490">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ccd28-491">@No__t-2 için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="ccd28-491">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ccd28-492">@No__t-1 sınıfı tarafından sunulan `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="ccd28-492">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="ccd28-493">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ccd28-493">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="ccd28-494">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-494">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="ccd28-495">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-495">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="ccd28-496">Aşağıdaki kodu `Startup.ConfigureServices` yöntemine el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ccd28-496">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ccd28-497">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-497">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ccd28-498">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-498">All options are named instances.</span></span> <span data-ttu-id="ccd28-499">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty` olan `Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-499">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ccd28-500"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ' i de uygular.</span><span class="sxs-lookup"><span data-stu-id="ccd28-500"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="ccd28-501">@No__t-0 ' ın varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-501">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="ccd28-502">@No__t-0 adlandırılmış seçeneği, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> Bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="ccd28-502">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="ccd28-503">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="ccd28-503">OptionsBuilder API</span></span>

<span data-ttu-id="ccd28-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="ccd28-505">`OptionsBuilder` @no__t, sonraki çağrıların tümünde görünmesini yerine yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ccd28-505">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="ccd28-506">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeleri yalnızca `OptionsBuilder` aracılığıyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-506">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="ccd28-507">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="ccd28-507">Use DI services to configure options</span></span>

<span data-ttu-id="ccd28-508">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ccd28-508">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="ccd28-509">[Options Builder @ no__t-2TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1)üzerinde [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-509">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="ccd28-510">Options [Builder @ no__t-1TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="ccd28-510">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="ccd28-511">@No__t-0 veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan kendi türünü oluşturun ve türü bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ccd28-511">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="ccd28-512">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="ccd28-512">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="ccd28-513">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-513">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="ccd28-514">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ' i kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-514">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="ccd28-515">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="ccd28-515">Options post-configuration</span></span>

<span data-ttu-id="ccd28-516">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-516">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="ccd28-517">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırması oluştuktan sonra yapılandırma sonrası çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="ccd28-517">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ccd28-518"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="ccd28-518"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ccd28-519">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="ccd28-519">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="ccd28-520">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="ccd28-520">Accessing options during startup</span></span>

<span data-ttu-id="ccd28-521"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure` ' de kullanılabilir. çünkü hizmetler `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="ccd28-521"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="ccd28-522">@No__t-2 ' de <xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="ccd28-522">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ccd28-523">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="ccd28-523">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ccd28-524">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ccd28-524">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
