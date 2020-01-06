---
title: ASP.NET Core için seçenek kalıbı
author: guardrex
description: ASP.NET Core uygulamalarında ilgili ayarların gruplarını temsil etmek için seçenekler deseninin nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/18/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 1e0bbe041223d376a778c6a326fcee4d254a9127
ms.sourcegitcommit: c815a9465e7b1bab44ce1643ec345b33e6cf1598
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75606785"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="bf9d5-103">ASP.NET Core için seçenek kalıbı</span><span class="sxs-lookup"><span data-stu-id="bf9d5-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="bf9d5-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bf9d5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bf9d5-105">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="bf9d5-106">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="bf9d5-107">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="bf9d5-108">Uygulamanın farklı parçaları için &ndash; ayarlarının [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı değildir veya birbirlerine aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="bf9d5-109">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="bf9d5-110">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="bf9d5-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bf9d5-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="bf9d5-112">Paket</span><span class="sxs-lookup"><span data-stu-id="bf9d5-112">Package</span></span>

<span data-ttu-id="bf9d5-113">[Microsoft. Extensions. Options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine ASP.NET Core uygulamalarında örtük olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="bf9d5-114">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="bf9d5-114">Options interfaces</span></span>

<span data-ttu-id="bf9d5-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="bf9d5-117">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="bf9d5-117">Change notifications</span></span>
* [<span data-ttu-id="bf9d5-118">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="bf9d5-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="bf9d5-119">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="bf9d5-120">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="bf9d5-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="bf9d5-121">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="bf9d5-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="bf9d5-123">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="bf9d5-124">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> alır ve önce tüm yapılandırmaların, ardından yapılandırma sonrası sonrasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="bf9d5-125"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="bf9d5-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="bf9d5-128">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="bf9d5-129"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="bf9d5-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="bf9d5-131">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="bf9d5-132"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="bf9d5-133">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> önceki <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="bf9d5-134">Zaten <xref:Microsoft.Extensions.Options.IOptions%601> arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>tarafından sağlanmış senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="bf9d5-135">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-135">General options configuration</span></span>

<span data-ttu-id="bf9d5-136">Genel Seçenekler yapılandırması örnek uygulamada &num;1 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="bf9d5-137">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="bf9d5-138">Aşağıdaki `MyOptions`sınıfında, `Option1` ve `Option2`iki özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="bf9d5-139">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1`varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="bf9d5-140">`Option2`, özelliği doğrudan başlatarak varsayılan değer kümesine sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="bf9d5-141">`MyOptions` sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmayla bağlantılı olarak hizmet kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="bf9d5-142">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="bf9d5-143">Örneğin *appSettings. JSON* dosyası `option1` ve `option2`değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="bf9d5-144">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="bf9d5-145">Bir ayarlar dosyasından seçenek yapılandırması 'nı yüklemek için özel bir <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="bf9d5-146">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="bf9d5-147">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="bf9d5-148">Basit seçeneklerin bir temsilciyle yapılandırılması, örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="bf9d5-149">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-149">Use a delegate to set options values.</span></span> <span data-ttu-id="bf9d5-150">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını kullanır (*modeller/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="bf9d5-151">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bf9d5-152">`MyOptionsWithDelegateConfig`ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="bf9d5-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="bf9d5-154">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="bf9d5-155">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="bf9d5-156">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="bf9d5-157">Her <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> çağrısı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="bf9d5-158">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="bf9d5-159">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="bf9d5-160">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="bf9d5-161">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-161">Suboptions configuration</span></span>

<span data-ttu-id="bf9d5-162">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="bf9d5-163">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="bf9d5-164">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="bf9d5-165">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her bir özellik `property[:sub-property:]`formun bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="bf9d5-166">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan anahtar `Option1`bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="bf9d5-167">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bf9d5-168">`MySubOptions` *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="bf9d5-169">`GetSection` yöntemi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> ad alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="bf9d5-170">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2`için anahtarlar içeren bir `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="bf9d5-171">`MySubOptions` sınıfı, Seçenekler değerlerini tutmak için özellikleri, `SubOption1` ve `SubOption2`tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="bf9d5-172">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="bf9d5-173">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="bf9d5-174">Seçeneklere ekleme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-174">Options injection</span></span>

<span data-ttu-id="bf9d5-175">Seçenekler ekleme, örnek uygulamada &num;4 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-175">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="bf9d5-176"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> içine ekle:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="bf9d5-177">[`@inject`](xref:mvc/views/razor#inject) Razor yönergesi ile Razor sayfası veya MVC görünümü.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-177">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="bf9d5-178">Bir sayfa veya görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-178">A page or view model.</span></span>

<span data-ttu-id="bf9d5-179">Örnek uygulamadan alınan aşağıdaki örnek, bir sayfa modeline <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="bf9d5-180">Örnek uygulama, `@inject` yönergeyle `IOptionsMonitor<MyOptions>` nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="bf9d5-181">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yapılır.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="bf9d5-183">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="bf9d5-184">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme örnek uygulamada örnek &num;5 gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="bf9d5-185"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>kullanarak, erişilen ve isteğin ömrü boyunca önbelleğe alınan istekler her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="bf9d5-186">`IOptionsMonitor` ve `IOptionsSnapshot` arasındaki fark şudur:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="bf9d5-187">`IOptionsMonitor`, geçerli seçenek değerlerini her zaman alan, özellikle de tek bağımlılıklarda yararlı olan bir [tek hizmettir](xref:fundamentals/dependency-injection#singleton) .</span><span class="sxs-lookup"><span data-stu-id="bf9d5-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="bf9d5-188">`IOptionsSnapshot` kapsamlı bir [hizmettir](xref:fundamentals/dependency-injection#scoped) ve `IOptionsSnapshot<T>` nesnesinin oluşturulduğu sırada seçeneklerin anlık görüntüsünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="bf9d5-189">Seçenekler anlık görüntüleri geçici ve kapsamlı bağımlılıklarla kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="bf9d5-190">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra yeni bir <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="bf9d5-191">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="bf9d5-192">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk `option1` ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="bf9d5-193">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200`olacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="bf9d5-194">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="bf9d5-195">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="bf9d5-196">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="bf9d5-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="bf9d5-197"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ile adlandırılmış seçenekler, örnek uygulamada 6 &num;örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="bf9d5-198">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-198">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="bf9d5-199">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [\<TOptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="bf9d5-200">Örnek uygulama, <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) adlı adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="bf9d5-201">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="bf9d5-202">`named_options_1` değerler, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="bf9d5-203">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="bf9d5-204">`Option1`için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="bf9d5-205">`MyOptions` sınıfı tarafından sağlanmış `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="bf9d5-206">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="bf9d5-207">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="bf9d5-208">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="bf9d5-209">`Startup.ConfigureServices` yöntemine el ile aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="bf9d5-210">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="bf9d5-211">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-211">All options are named instances.</span></span> <span data-ttu-id="bf9d5-212">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty``Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="bf9d5-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> Ayrıca <xref:Microsoft.Extensions.Options.IConfigureOptions%601>uygular.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="bf9d5-214"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="bf9d5-215">`null` adlandırılmış seçenek, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="bf9d5-216">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="bf9d5-216">OptionsBuilder API</span></span>

<span data-ttu-id="bf9d5-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>, `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-218">`OptionsBuilder`, sonraki çağrıların tümünde olmak yerine ilk `AddOptions<TOptions>(string optionsName)` çağrısına yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="bf9d5-219">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeler yalnızca `OptionsBuilder`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="bf9d5-220">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-220">Use DI services to configure options</span></span>

<span data-ttu-id="bf9d5-221">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="bf9d5-222">[\<TOptions > ' nin Options Builder](xref:Microsoft.Extensions.Options.OptionsBuilder`1)'da [yapılandırılması](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="bf9d5-223">`OptionsBuilder<TOptions>`, seçenekleri yapılandırmak için en fazla beş hizmet kullanılmasına izin veren [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-223">`OptionsBuilder<TOptions>` provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="bf9d5-224"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan ve türü bir hizmet olarak kaydeden kendi türünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="bf9d5-225">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="bf9d5-226">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="bf9d5-227">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>kaydeder.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="bf9d5-228">Seçenekler doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-228">Options validation</span></span>

<span data-ttu-id="bf9d5-229">Seçenekler doğrulaması seçenekler yapılandırıldığında seçenekleri doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="bf9d5-230">Seçenekler geçerliyse `true` döndüren ve geçerli değillerse `false` bir doğrulama yöntemiyle `Validate` çağırın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="bf9d5-231">Önceki örnekte, adlandırılmış seçenekler örneği `optionalOptionsName`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="bf9d5-232">Varsayılan Seçenekler örneği `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="bf9d5-233">Seçenekler örneği oluşturulduğunda doğrulama çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="bf9d5-234">Seçenek Örneğinizde, ilk kez erişildiğinde doğrulamanın başarılı olması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf9d5-235">Seçenekler, Seçenekler başlangıçta yapılandırıldıktan ve doğrulandıktan sonra seçeneklerindeki değişikliklere karşı koruma yapmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="bf9d5-236">`Validate` yöntemi bir `Func<TOptions, bool>`kabul eder.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="bf9d5-237">Doğrulamayı tamamen özelleştirmek için, şunları sağlayan `IValidateOptions<TOptions>`uygulayın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="bf9d5-238">Birden çok seçenek türünün doğrulanması: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="bf9d5-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="bf9d5-239">Başka bir seçenek türüne bağlı olan doğrulama: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="bf9d5-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="bf9d5-240">`IValidateOptions` şunları doğrular:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-240">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="bf9d5-241">Belirli bir adlandırılmış seçenekler örneği.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-241">A specific named options instance.</span></span>
* <span data-ttu-id="bf9d5-242">`name` `null`tüm seçenekler.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-242">All options when `name` is `null`.</span></span>

<span data-ttu-id="bf9d5-243">Arabirimin uygulanınızdan bir `ValidateOptionsResult` döndürün:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="bf9d5-244">Veri ek açıklaması tabanlı doğrulama, `OptionsBuilder<TOptions>`<xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> yöntemi çağırarak [Microsoft. Extensions. Options. DataAnnotation](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) paketinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="bf9d5-245">`Microsoft.Extensions.Options.DataAnnotations` ASP.NET Core uygulamalarda örtülü olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="bf9d5-246">Eager doğrulaması (başlangıçta hızlı başarısız), gelecek bir sürüm için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-246">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="bf9d5-247">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="bf9d5-247">Options post-configuration</span></span>

<span data-ttu-id="bf9d5-248">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="bf9d5-249">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra yapılandırma sonrası çalıştırmalar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bf9d5-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bf9d5-251">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="bf9d5-252">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-252">Accessing options during startup</span></span>

<span data-ttu-id="bf9d5-253"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure`kullanılabilir, çünkü Hizmetleri `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="bf9d5-254">`Startup.ConfigureServices`<xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bf9d5-255">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-255">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="bf9d5-256">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-256">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="bf9d5-257">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="bf9d5-258">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="bf9d5-259">Uygulamanın farklı parçaları için &ndash; ayarlarının [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı değildir veya birbirlerine aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="bf9d5-260">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-260">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="bf9d5-261">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-261">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="bf9d5-262">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bf9d5-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf9d5-263">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="bf9d5-263">Prerequisites</span></span>

<span data-ttu-id="bf9d5-264">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Options. configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="bf9d5-265">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="bf9d5-265">Options interfaces</span></span>

<span data-ttu-id="bf9d5-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="bf9d5-268">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="bf9d5-268">Change notifications</span></span>
* [<span data-ttu-id="bf9d5-269">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="bf9d5-269">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="bf9d5-270">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-270">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="bf9d5-271">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="bf9d5-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="bf9d5-272">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="bf9d5-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="bf9d5-274">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="bf9d5-275">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> alır ve önce tüm yapılandırmaların, ardından yapılandırma sonrası sonrasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="bf9d5-276"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="bf9d5-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-278"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="bf9d5-279">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="bf9d5-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="bf9d5-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="bf9d5-282">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="bf9d5-283"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="bf9d5-284">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> önceki <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="bf9d5-285">Zaten <xref:Microsoft.Extensions.Options.IOptions%601> arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>tarafından sağlanmış senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="bf9d5-286">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-286">General options configuration</span></span>

<span data-ttu-id="bf9d5-287">Genel Seçenekler yapılandırması örnek uygulamada &num;1 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="bf9d5-288">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-288">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="bf9d5-289">Aşağıdaki `MyOptions`sınıfında, `Option1` ve `Option2`iki özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="bf9d5-290">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1`varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="bf9d5-291">`Option2`, özelliği doğrudan başlatarak varsayılan değer kümesine sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="bf9d5-292">`MyOptions` sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmayla bağlantılı olarak hizmet kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="bf9d5-293">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="bf9d5-294">Örneğin *appSettings. JSON* dosyası `option1` ve `option2`değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="bf9d5-295">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="bf9d5-296">Bir ayarlar dosyasından seçenek yapılandırması 'nı yüklemek için özel bir <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="bf9d5-297">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="bf9d5-298">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-298">Configure simple options with a delegate</span></span>

<span data-ttu-id="bf9d5-299">Basit seçeneklerin bir temsilciyle yapılandırılması, örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="bf9d5-300">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-300">Use a delegate to set options values.</span></span> <span data-ttu-id="bf9d5-301">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını kullanır (*modeller/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="bf9d5-302">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bf9d5-303">`MyOptionsWithDelegateConfig`ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="bf9d5-304">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-304">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="bf9d5-305">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-305">You can add multiple configuration providers.</span></span> <span data-ttu-id="bf9d5-306">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="bf9d5-307">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-307">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="bf9d5-308">Her <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> çağrısı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="bf9d5-309">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="bf9d5-310">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="bf9d5-311">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="bf9d5-312">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-312">Suboptions configuration</span></span>

<span data-ttu-id="bf9d5-313">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="bf9d5-314">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="bf9d5-315">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="bf9d5-316">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her bir özellik `property[:sub-property:]`formun bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="bf9d5-317">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan anahtar `Option1`bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="bf9d5-318">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bf9d5-319">`MySubOptions` *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="bf9d5-320">`GetSection` yöntemi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> ad alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="bf9d5-321">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2`için anahtarlar içeren bir `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="bf9d5-322">`MySubOptions` sınıfı, Seçenekler değerlerini tutmak için özellikleri, `SubOption1` ve `SubOption2`tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="bf9d5-323">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="bf9d5-324">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="bf9d5-325">Seçeneklere ekleme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-325">Options injection</span></span>

<span data-ttu-id="bf9d5-326">Seçenekler ekleme, örnek uygulamada &num;4 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-326">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="bf9d5-327"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> içine ekle:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="bf9d5-328">[`@inject`](xref:mvc/views/razor#inject) Razor yönergesi ile Razor sayfası veya MVC görünümü.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-328">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="bf9d5-329">Bir sayfa veya görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-329">A page or view model.</span></span>

<span data-ttu-id="bf9d5-330">Örnek uygulamadan alınan aşağıdaki örnek, bir sayfa modeline <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="bf9d5-331">Örnek uygulama, `@inject` yönergeyle `IOptionsMonitor<MyOptions>` nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="bf9d5-332">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-332">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yapılır.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="bf9d5-334">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-334">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="bf9d5-335">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme örnek uygulamada örnek &num;5 gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="bf9d5-336"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>kullanarak, erişilen ve isteğin ömrü boyunca önbelleğe alınan istekler her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="bf9d5-337">`IOptionsMonitor` ve `IOptionsSnapshot` arasındaki fark şudur:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="bf9d5-338">`IOptionsMonitor`, geçerli seçenek değerlerini her zaman alan, özellikle de tek bağımlılıklarda yararlı olan bir [tek hizmettir](xref:fundamentals/dependency-injection#singleton) .</span><span class="sxs-lookup"><span data-stu-id="bf9d5-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="bf9d5-339">`IOptionsSnapshot` kapsamlı bir [hizmettir](xref:fundamentals/dependency-injection#scoped) ve `IOptionsSnapshot<T>` nesnesinin oluşturulduğu sırada seçeneklerin anlık görüntüsünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="bf9d5-340">Seçenekler anlık görüntüleri geçici ve kapsamlı bağımlılıklarla kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-340">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="bf9d5-341">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra yeni bir <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="bf9d5-342">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="bf9d5-343">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk `option1` ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="bf9d5-344">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200`olacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="bf9d5-345">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-345">Save the *appsettings.json* file.</span></span> <span data-ttu-id="bf9d5-346">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-346">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="bf9d5-347">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="bf9d5-347">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="bf9d5-348"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ile adlandırılmış seçenekler, örnek uygulamada 6 &num;örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="bf9d5-349">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-349">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="bf9d5-350">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [\<TOptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="bf9d5-351">Örnek uygulama, <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) adlı adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="bf9d5-352">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-352">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="bf9d5-353">`named_options_1` değerler, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="bf9d5-354">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-354">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="bf9d5-355">`Option1`için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="bf9d5-356">`MyOptions` sınıfı tarafından sağlanmış `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-356">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="bf9d5-357">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-357">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="bf9d5-358">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="bf9d5-359">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-359">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="bf9d5-360">`Startup.ConfigureServices` yöntemine el ile aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-360">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="bf9d5-361">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-361">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="bf9d5-362">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-362">All options are named instances.</span></span> <span data-ttu-id="bf9d5-363">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty``Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="bf9d5-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> Ayrıca <xref:Microsoft.Extensions.Options.IConfigureOptions%601>uygular.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="bf9d5-365"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="bf9d5-366">`null` adlandırılmış seçenek, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="bf9d5-367">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="bf9d5-367">OptionsBuilder API</span></span>

<span data-ttu-id="bf9d5-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>, `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-369">`OptionsBuilder`, sonraki çağrıların tümünde olmak yerine ilk `AddOptions<TOptions>(string optionsName)` çağrısına yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="bf9d5-370">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeler yalnızca `OptionsBuilder`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="bf9d5-371">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-371">Use DI services to configure options</span></span>

<span data-ttu-id="bf9d5-372">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-372">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="bf9d5-373">[\<TOptions > ' nin Options Builder](xref:Microsoft.Extensions.Options.OptionsBuilder`1)'da [yapılandırılması](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="bf9d5-374">[\<bir TOptions > seçenek Oluşturucusu](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="bf9d5-375"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan ve türü bir hizmet olarak kaydeden kendi türünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="bf9d5-376">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="bf9d5-377">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="bf9d5-378">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>kaydeder.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="bf9d5-379">Seçenekler doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-379">Options validation</span></span>

<span data-ttu-id="bf9d5-380">Seçenekler doğrulaması seçenekler yapılandırıldığında seçenekleri doğrulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-380">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="bf9d5-381">Seçenekler geçerliyse `true` döndüren ve geçerli değillerse `false` bir doğrulama yöntemiyle `Validate` çağırın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="bf9d5-382">Önceki örnekte, adlandırılmış seçenekler örneği `optionalOptionsName`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-382">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="bf9d5-383">Varsayılan Seçenekler örneği `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-383">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="bf9d5-384">Seçenekler örneği oluşturulduğunda doğrulama çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-384">Validation runs when the options instance is created.</span></span> <span data-ttu-id="bf9d5-385">Seçenek Örneğinizde, ilk kez erişildiğinde doğrulamanın başarılı olması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf9d5-386">Seçenekler, Seçenekler başlangıçta yapılandırıldıktan ve doğrulandıktan sonra seçeneklerindeki değişikliklere karşı koruma yapmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="bf9d5-387">`Validate` yöntemi bir `Func<TOptions, bool>`kabul eder.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="bf9d5-388">Doğrulamayı tamamen özelleştirmek için, şunları sağlayan `IValidateOptions<TOptions>`uygulayın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="bf9d5-389">Birden çok seçenek türünün doğrulanması: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="bf9d5-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="bf9d5-390">Başka bir seçenek türüne bağlı olan doğrulama: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="bf9d5-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="bf9d5-391">`IValidateOptions` şunları doğrular:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-391">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="bf9d5-392">Belirli bir adlandırılmış seçenekler örneği.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-392">A specific named options instance.</span></span>
* <span data-ttu-id="bf9d5-393">`name` `null`tüm seçenekler.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-393">All options when `name` is `null`.</span></span>

<span data-ttu-id="bf9d5-394">Arabirimin uygulanınızdan bir `ValidateOptionsResult` döndürün:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="bf9d5-395">Veri ek açıklaması tabanlı doğrulama, `OptionsBuilder<TOptions>`<xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> yöntemi çağırarak [Microsoft. Extensions. Options. DataAnnotation](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) paketinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="bf9d5-396">`Microsoft.Extensions.Options.DataAnnotations`, [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="bf9d5-397">Eager doğrulaması (başlangıçta hızlı başarısız), gelecek bir sürüm için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-397">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="bf9d5-398">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="bf9d5-398">Options post-configuration</span></span>

<span data-ttu-id="bf9d5-399">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="bf9d5-400">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra yapılandırma sonrası çalıştırmalar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bf9d5-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bf9d5-402">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="bf9d5-403">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-403">Accessing options during startup</span></span>

<span data-ttu-id="bf9d5-404"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure`kullanılabilir, çünkü Hizmetleri `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="bf9d5-405">`Startup.ConfigureServices`<xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bf9d5-406">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-406">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="bf9d5-407">Seçenekler stili, ilişkili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-407">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="bf9d5-408">[Yapılandırma ayarları](xref:fundamentals/configuration/index) senaryo tarafından ayrı sınıflara ayrılmışsa, uygulama iki önemli yazılım mühendisliği ilkelerine uyar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="bf9d5-409">Yapılandırma ayarlarına bağlı olan [arabirim ayırma ilkesi (ISS) veya kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; senaryoları (sınıflar) yalnızca kullandıkları yapılandırma ayarlarına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="bf9d5-410">Uygulamanın farklı parçaları için &ndash; ayarlarının [ayrılması,](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) birbirine bağımlı değildir veya birbirlerine aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="bf9d5-411">Seçenekler Ayrıca yapılandırma verilerini doğrulamaya yönelik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-411">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="bf9d5-412">Daha fazla bilgi için [Seçenekler doğrulama](#options-validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-412">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="bf9d5-413">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bf9d5-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf9d5-414">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="bf9d5-414">Prerequisites</span></span>

<span data-ttu-id="bf9d5-415">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Options. configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="bf9d5-416">Seçenekler arabirimleri</span><span class="sxs-lookup"><span data-stu-id="bf9d5-416">Options interfaces</span></span>

<span data-ttu-id="bf9d5-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, `TOptions` örnekleri için seçenekleri almak ve seçenek bildirimlerini yönetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="bf9d5-419">Değişiklik bildirimleri</span><span class="sxs-lookup"><span data-stu-id="bf9d5-419">Change notifications</span></span>
* [<span data-ttu-id="bf9d5-420">Adlandırılmış seçenekler</span><span class="sxs-lookup"><span data-stu-id="bf9d5-420">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="bf9d5-421">Yeniden yüklenebilir yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-421">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="bf9d5-422">Seçmeli seçenekler geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="bf9d5-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="bf9d5-423">[Yapılandırma sonrası](#options-post-configuration) senaryolar, tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra seçenekleri ayarlamanıza veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="bf9d5-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> yeni seçenek örnekleri oluşturmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="bf9d5-425">Tek bir <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="bf9d5-426">Varsayılan uygulama tüm kayıtlı <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> alır ve önce tüm yapılandırmaların, ardından yapılandırma sonrası sonrasında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="bf9d5-427"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> arasında ayrım yapar ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="bf9d5-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> tarafından `TOptions` örnekleri önbelleğe almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-429"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>, değer yeniden hesaplanabilmesi için izleyici içindeki seçenek örneklerini geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="bf9d5-430">Değerler, <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>ile el ile tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="bf9d5-431"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> yöntemi, tüm adlandırılmış örneklerin isteğe bağlı olarak yeniden oluşturulması gerektiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="bf9d5-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, her istekte seçeneklerin yeniden hesaplanması gereken senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="bf9d5-433">Daha fazla bilgi için bkz. [ıoptionssnapshot ile yapılandırma verilerini yeniden yükleme](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="bf9d5-434"><xref:Microsoft.Extensions.Options.IOptions%601>, seçenekleri desteklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="bf9d5-435">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> önceki <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>senaryolarını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="bf9d5-436">Zaten <xref:Microsoft.Extensions.Options.IOptions%601> arabirimini kullanan mevcut çerçeveler ve kitaplıklarda <xref:Microsoft.Extensions.Options.IOptions%601> kullanmaya devam edebilir ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>tarafından sağlanmış senaryolara gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="bf9d5-437">Genel Seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-437">General options configuration</span></span>

<span data-ttu-id="bf9d5-438">Genel Seçenekler yapılandırması örnek uygulamada &num;1 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="bf9d5-439">Bir seçenek sınıfı ortak parametresiz bir Oluşturucu ile soyut olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-439">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="bf9d5-440">Aşağıdaki `MyOptions`sınıfında, `Option1` ve `Option2`iki özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="bf9d5-441">Varsayılan değerleri ayarlama isteğe bağlıdır, ancak aşağıdaki örnekteki sınıf oluşturucusu `Option1`varsayılan değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="bf9d5-442">`Option2`, özelliği doğrudan başlatarak varsayılan değer kümesine sahiptir (*modeller/MyOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="bf9d5-443">`MyOptions` sınıfı, <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmayla bağlantılı olarak hizmet kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="bf9d5-444">Aşağıdaki sayfa modeli, ayarlara erişmek için <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ile [Oluşturucu bağımlılığı ekleme](xref:mvc/controllers/dependency-injection) işlemini kullanır (*Sayfalar/Index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="bf9d5-445">Örneğin *appSettings. JSON* dosyası `option1` ve `option2`değerlerini belirtir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="bf9d5-446">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="bf9d5-447">Bir ayarlar dosyasından seçenek yapılandırması 'nı yüklemek için özel bir <xref:System.Configuration.ConfigurationBuilder> kullanırken, temel yolun doğru şekilde ayarlandığını onaylayın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="bf9d5-448">Ayarlar dosyasından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>aracılığıyla seçenek yapılandırması yüklenirken taban yolunu açıkça ayarlama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="bf9d5-449">Bir temsilciyle basit seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-449">Configure simple options with a delegate</span></span>

<span data-ttu-id="bf9d5-450">Basit seçeneklerin bir temsilciyle yapılandırılması, örnek uygulamada &num;2 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="bf9d5-451">Seçenek değerlerini ayarlamak için bir temsilci kullanın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-451">Use a delegate to set options values.</span></span> <span data-ttu-id="bf9d5-452">Örnek uygulama `MyOptionsWithDelegateConfig` sınıfını kullanır (*modeller/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="bf9d5-453">Aşağıdaki kodda, hizmet kapsayıcısına ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bf9d5-454">`MyOptionsWithDelegateConfig`ile bağlamayı yapılandırmak için bir temsilci kullanır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="bf9d5-455">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-455">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="bf9d5-456">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-456">You can add multiple configuration providers.</span></span> <span data-ttu-id="bf9d5-457">Yapılandırma sağlayıcıları NuGet paketlerinde kullanılabilir ve kayıtlı oldukları sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="bf9d5-458">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-458">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="bf9d5-459">Her <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> çağrısı, hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti ekler.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="bf9d5-460">Yukarıdaki örnekte, `Option1` ve `Option2` değerlerinin ikisi de *appSettings. JSON*içinde belirtilmiştir, ancak `Option1` ve `Option2` değerleri yapılandırılan temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="bf9d5-461">Birden fazla yapılandırma hizmeti etkinleştirildiğinde, son yapılandırma kaynağı *WINS* ' i ve yapılandırma değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="bf9d5-462">Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi, seçenek sınıfı değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="bf9d5-463">Alt seçenekler yapılandırması</span><span class="sxs-lookup"><span data-stu-id="bf9d5-463">Suboptions configuration</span></span>

<span data-ttu-id="bf9d5-464">Alt seçenekler yapılandırması örnek uygulamada &num;3 örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="bf9d5-465">Uygulamalar, uygulamadaki belirli senaryo gruplarına (sınıflar) ait seçenek sınıfları oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="bf9d5-466">Uygulamanın yapılandırma değerleri gerektiren bölümlerinin yalnızca kullandıkları yapılandırma değerlerine erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="bf9d5-467">Seçenekleri yapılandırmaya bağlama sırasında, seçenek türündeki her bir özellik `property[:sub-property:]`formun bir yapılandırma anahtarına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="bf9d5-468">Örneğin, `MyOptions.Option1` özelliği *appSettings. JSON*içindeki `option1` özelliğinden okunan anahtar `Option1`bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="bf9d5-469">Aşağıdaki kodda, hizmet kapsayıcısına üçüncü bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmeti eklenir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bf9d5-470">`MySubOptions` *appSettings. JSON* dosyasının `subsection` bölümüne bağlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="bf9d5-471">`GetSection` yöntemi <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> ad alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="bf9d5-472">Örneğin *appSettings. JSON* dosyası, `suboption1` ve `suboption2`için anahtarlar içeren bir `subsection` üyesini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="bf9d5-473">`MySubOptions` sınıfı, Seçenekler değerlerini tutmak için özellikleri, `SubOption1` ve `SubOption2`tanımlar (*modeller/Myalt seçenekler. cs*):</span><span class="sxs-lookup"><span data-stu-id="bf9d5-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="bf9d5-474">Sayfa modelinin `OnGet` yöntemi, Seçenekler değerleriyle (*Pages/Index. cshtml. cs*) bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="bf9d5-475">Uygulama çalıştırıldığında `OnGet` yöntemi, alt sınıf değerlerini gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="bf9d5-476">Bir görünüm modeli veya doğrudan görünüm ekleme ile belirtilen seçenekler</span><span class="sxs-lookup"><span data-stu-id="bf9d5-476">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="bf9d5-477">Bir görünüm modeli veya doğrudan görünüm ekleme ile sunulan seçenekler örnek uygulamada &num;4 olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="bf9d5-478">Seçenekler, bir görünüm modelinde veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ekleme tarafından doğrudan bir görünüme (*Pages/Index. cshtml. cs*) sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="bf9d5-479">Örnek uygulama, `@inject` yönergeyle `IOptionsMonitor<MyOptions>` nasıl ekleneceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="bf9d5-480">Uygulama çalıştırıldığında, seçenek değerleri işlenen sayfada gösterilir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-480">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek1: value1_from_json ve Seçenek2:-1 seçenek değerleri modelden yüklenir ve görünüme ekleme yapılır.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="bf9d5-482">Ioptionssnapshot ile yapılandırma verilerini yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-482">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="bf9d5-483">Yapılandırma verilerini <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> ile yeniden yükleme örnek uygulamada örnek &num;5 gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="bf9d5-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, minimum işleme yüküyle yeniden yükleme seçeneklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="bf9d5-485">Seçenekler erişildiğinde ve isteğin ömrü boyunca önbelleğe alındığında her istek için bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="bf9d5-486">Aşağıdaki örnek, *appSettings. JSON* değişikliklerinden sonra yeni bir <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> nasıl oluşturulduğunu gösterir (*Pages/Index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="bf9d5-487">Sunucu için birden çok istek, dosya değiştirilene ve yapılandırma yeniden yükleninceye kadar *appSettings. JSON* dosyası tarafından belirtilen sabit değerler döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="bf9d5-488">Aşağıdaki görüntüde, *appSettings. JSON* dosyasından yüklenen ilk `option1` ve `option2` değerleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="bf9d5-489">*AppSettings. JSON* dosyasındaki değerleri `value1_from_json UPDATED` ve `200`olacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="bf9d5-490">*AppSettings. JSON* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-490">Save the *appsettings.json* file.</span></span> <span data-ttu-id="bf9d5-491">Seçenekler değerlerinin güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-491">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="bf9d5-492">IController Enamedooptıons ile adlandırılmış seçenekler desteği</span><span class="sxs-lookup"><span data-stu-id="bf9d5-492">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="bf9d5-493"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ile adlandırılmış seçenekler, örnek uygulamada 6 &num;örnek olarak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="bf9d5-494">*Adlandırılmış seçenekler* desteği, uygulamanın adlandırılmış seçenek yapılandırmalarının ayırt etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-494">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="bf9d5-495">Örnek uygulamada, adlandırılmış Seçenekler [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)ile bildirilmiştir ve bu, [\<TOptions > ' i çağırır. ](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)Uzantı yöntemini Yapılandır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="bf9d5-496">Örnek uygulama, <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index. cshtml. cs*) adlı adlandırılmış seçeneklere erişir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="bf9d5-497">Örnek uygulama çalıştırıldığında, adlandırılmış seçenekler döndürülür:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-497">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="bf9d5-498">`named_options_1` değerler, *appSettings. JSON* dosyasından yüklenen yapılandırmadan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="bf9d5-499">`named_options_2` değerleri tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-499">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="bf9d5-500">`Option1`için `ConfigureServices` `named_options_2` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="bf9d5-501">`MyOptions` sınıfı tarafından sağlanmış `Option2` için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-501">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="bf9d5-502">Tüm seçenekleri ConfigureAll yöntemiyle yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-502">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="bf9d5-503">Tüm seçenek örneklerini <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemiyle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="bf9d5-504">Aşağıdaki kod, ortak bir değere sahip tüm yapılandırma örnekleri için `Option1` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-504">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="bf9d5-505">`Startup.ConfigureServices` yöntemine el ile aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-505">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="bf9d5-506">Kodu ekledikten sonra örnek uygulamayı çalıştırmak aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-506">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="bf9d5-507">Tüm seçenekler adlandırılmış örneklerdir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-507">All options are named instances.</span></span> <span data-ttu-id="bf9d5-508">Mevcut <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri, `string.Empty``Options.DefaultName` örneğini hedefleyecek şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="bf9d5-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> Ayrıca <xref:Microsoft.Extensions.Options.IConfigureOptions%601>uygular.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="bf9d5-510"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> varsayılan uygulamasının her birini uygun şekilde kullanma mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="bf9d5-511">`null` adlandırılmış seçenek, belirli bir adlandırılmış örnek yerine tüm adlandırılmış örnekleri hedeflemek için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="bf9d5-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="bf9d5-512">Seçenekno Oluşturucu API 'SI</span><span class="sxs-lookup"><span data-stu-id="bf9d5-512">OptionsBuilder API</span></span>

<span data-ttu-id="bf9d5-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>, `TOptions` örnekleri yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="bf9d5-514">`OptionsBuilder`, sonraki çağrıların tümünde olmak yerine ilk `AddOptions<TOptions>(string optionsName)` çağrısına yalnızca tek bir parametre olan adlandırılmış seçenekleri oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="bf9d5-515">Seçenekler doğrulaması ve hizmet bağımlılıklarını kabul eden `ConfigureOptions` aşırı yüklemeler yalnızca `OptionsBuilder`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="bf9d5-516">Ayarları yapılandırmak için dı hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="bf9d5-516">Use DI services to configure options</span></span>

<span data-ttu-id="bf9d5-517">Seçenekleri iki şekilde yapılandırırken, bağımlılık ekleme işleminden diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-517">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="bf9d5-518">[\<TOptions > ' nin Options Builder](xref:Microsoft.Extensions.Options.OptionsBuilder`1)'da [yapılandırılması](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) için bir yapılandırma temsilcisi geçirin.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="bf9d5-519">[\<bir TOptions > seçenek Oluşturucusu](xref:Microsoft.Extensions.Options.OptionsBuilder`1) , seçenekleri yapılandırmak için en fazla beş hizmeti kullanmanıza olanak sağlayan, [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) yüklerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="bf9d5-520"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> uygulayan ve türü bir hizmet olarak kaydeden kendi türünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="bf9d5-521">Bir hizmetin oluşturulması daha karmaşık olduğundan, [yapılandırmak](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)için bir yapılandırma temsilcisinin geçirilmesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="bf9d5-522">Kendi türünü oluşturmak, [Yapılandır](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)'ı kullandığınızda çerçevenin sizin için yaptığı işe eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="bf9d5-523">[Yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) çağrısı, belirtilen genel hizmet türlerini kabul eden bir oluşturucuya sahip olan geçici genel <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>kaydeder.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="bf9d5-524">Yapılandırma sonrası seçenekler</span><span class="sxs-lookup"><span data-stu-id="bf9d5-524">Options post-configuration</span></span>

<span data-ttu-id="bf9d5-525">Yapılandırma sonrası <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="bf9d5-526">Tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma oluştuktan sonra yapılandırma sonrası çalıştırmalar:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bf9d5-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>, adlandırılmış seçenekleri yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bf9d5-528">Tüm yapılandırma örneklerini yapılandırmak için <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> kullanın:</span><span class="sxs-lookup"><span data-stu-id="bf9d5-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="bf9d5-529">Başlangıç sırasında seçeneklere erişme</span><span class="sxs-lookup"><span data-stu-id="bf9d5-529">Accessing options during startup</span></span>

<span data-ttu-id="bf9d5-530"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> `Startup.Configure`kullanılabilir, çünkü Hizmetleri `Configure` Yöntemi yürütülmeden önce oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="bf9d5-531">`Startup.ConfigureServices`<xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bf9d5-532">Hizmet kayıtlarının sıralaması nedeniyle tutarsız bir seçenek durumu var olabilir.</span><span class="sxs-lookup"><span data-stu-id="bf9d5-532">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="bf9d5-533">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bf9d5-533">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
