---
title: ASP.NET Core desende seçenekleri
author: guardrex
description: ASP.NET Core uygulamalarında ilgili ayar gruplarını temsil etmek için seçenekleri deseni kullanmayı keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 62da2504941be0c29ddb15aaab9c091f387ccdb2
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327293"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="bdeef-103">ASP.NET Core desende seçenekleri</span><span class="sxs-lookup"><span data-stu-id="bdeef-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="bdeef-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bdeef-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="bdeef-105">Bu konuda 1.1 sürümü için indirme [ASP.NET Core (sürüm 1.1, PDF) seçenekleri desende](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="bdeef-105">For the 1.1 version of this topic, download [Options pattern in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="bdeef-106">Seçenekleri deseni sınıfları, ilgili ayar gruplarını temsil etmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-106">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="bdeef-107">Zaman [yapılandırma ayarlarını](xref:fundamentals/configuration/index) yalıtılmış ayrı sınıf senaryoya göre uygulama için iki önemli yazılım Mühendisliği ilkeden uyar:</span><span class="sxs-lookup"><span data-stu-id="bdeef-107">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="bdeef-108">[Arabirimi ayırma ilkesi (ISS) veya Kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; kullandıkları yapılandırma ayarlarını yapılandırma ayarlarına bağlı olan senaryoları (sınıflar) bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-108">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="bdeef-109">[Görev ayrımı nettir](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; uygulamanın farklı kısımlarını ayarları bağımlı veya birbirine bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="bdeef-109">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="bdeef-110">Seçenekler, ayrıca yapılandırma verilerini doğrulamak için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="bdeef-110">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="bdeef-111">Daha fazla bilgi için [seçeneklerini doğrulama](#options-validation) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bdeef-111">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="bdeef-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bdeef-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdeef-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="bdeef-113">Prerequisites</span></span>

<span data-ttu-id="bdeef-114">Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paket.</span><span class="sxs-lookup"><span data-stu-id="bdeef-114">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="bdeef-115">Seçenekleri arabirimleri</span><span class="sxs-lookup"><span data-stu-id="bdeef-115">Options interfaces</span></span>

<span data-ttu-id="bdeef-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> seçenekleri almak ve yönetmek için seçenekleri bildirimleri için kullanılan `TOptions` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="bdeef-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="bdeef-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> Aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="bdeef-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="bdeef-118">Değişiklik bildirimleri verme</span><span class="sxs-lookup"><span data-stu-id="bdeef-118">Change notifications</span></span>
* [<span data-ttu-id="bdeef-119">Adlandırılmış seçenekleri</span><span class="sxs-lookup"><span data-stu-id="bdeef-119">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="bdeef-120">Reloadable yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bdeef-120">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="bdeef-121">Seçici seçenekleri geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="bdeef-121">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="bdeef-122">[Yapılandırma sonrası](#options-post-configuration) senaryolar ayarlamak veya tüm seçenekleri değiştirmek izin <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-122">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="bdeef-123"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> yeni seçenekler örnekleri oluşturmak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="bdeef-123"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="bdeef-124">Tek bir sahip <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bdeef-124">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="bdeef-125">Varsayılan uygulama, tüm kayıtlı alan <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> ve sonrası yapılandırma tarafından izlenen tüm yapılandırmaları ilk olarak çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-125">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="bdeef-126">Bunu ayırt <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve <xref:Microsoft.Extensions.Options.IConfigureOptions%601> ve yalnızca uygun arabirimi çağırır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-126">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="bdeef-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> tarafından kullanılan <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> önbelleğine `TOptions` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="bdeef-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="bdeef-128"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> Değeri yeniden böylece seçenekleri durumlarda, izleyici geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="bdeef-128">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="bdeef-129">Değerleri el ile sunulan <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="bdeef-129">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="bdeef-130"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> Yöntemi tüm adlandırılmış örnekler isteğe bağlı olarak yeniden oluşturulması sırasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-130">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="bdeef-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> Burada seçenekleri her istekte yeniden senaryolarda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="bdeef-132">Daha fazla bilgi için [yeniden yapılandırma verileriyle IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bdeef-132">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="bdeef-133"><xref:Microsoft.Extensions.Options.IOptions%601> destek seçenekleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-133"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="bdeef-134">Ancak, <xref:Microsoft.Extensions.Options.IOptions%601> önceki senaryoları desteklemez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="bdeef-134">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="bdeef-135">Uygulamasını kullanmaya devam edebilir <xref:Microsoft.Extensions.Options.IOptions%601> mevcut altyapılarınız ve zaten kullandığınız kitaplıklar <xref:Microsoft.Extensions.Options.IOptions%601> arabirim ve tarafından sağlanan senaryoları gerektirmeyen <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="bdeef-135">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="bdeef-136">Genel Seçenekler yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bdeef-136">General options configuration</span></span>

<span data-ttu-id="bdeef-137">Genel seçenekleri yapılandırma, örnek olarak gösterilmiştir &num;örnek uygulamada 1.</span><span class="sxs-lookup"><span data-stu-id="bdeef-137">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="bdeef-138">Bir seçenek sınıfı soyut olmayan olmalıdır genel parametresiz oluşturucusu ile.</span><span class="sxs-lookup"><span data-stu-id="bdeef-138">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="bdeef-139">Aşağıdaki sınıf `MyOptions`, iki özelliğe sahiptir `Option1` ve `Option2`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-139">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="bdeef-140">Varsayılan değerleri ayarlama, isteğe bağlıdır, ancak aşağıdaki örnekte sınıf oluşturucu varsayılan değerini ayarlar `Option1`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-140">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="bdeef-141">`Option2` özelliği doğrudan başlatarak ayarlanmış varsayılan değerine sahip (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdeef-141">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="bdeef-142">`MyOptions` Sınıfı ile hizmet kapsayıcıya eklenir <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmasına bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="bdeef-142">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="bdeef-143">Sayfa modeli kullanan aşağıdaki [Oluşturucusu bağımlılık ekleme](xref:mvc/controllers/dependency-injection) ile <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> ayarlara erişmek için (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdeef-143">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="bdeef-144">Örnek kullanıcının *appsettings.json* dosya için değerler belirten `option1` ve `option2`:</span><span class="sxs-lookup"><span data-stu-id="bdeef-144">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="bdeef-145">Ne zaman uygulamayı çalıştırın, sayfa modelin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bdeef-145">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="bdeef-146">Özel bir kullanırken <xref:System.Configuration.ConfigurationBuilder> seçenekleri yapılandırma ayarları dosyadan yüklemek için temel yolu doğru şekilde ayarlandığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="bdeef-146">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="bdeef-147">Temel yol açık olarak ayarlama gerekli değildir seçenekleri yapılandırma ile ayarları dosyasından yüklenirken <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="bdeef-147">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="bdeef-148">Bir temsilci ile basit seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="bdeef-148">Configure simple options with a delegate</span></span>

<span data-ttu-id="bdeef-149">Bir temsilci ile basit seçeneklerini yapılandırma örnek olarak gösterilmiştir &num;örnek uygulamada 2.</span><span class="sxs-lookup"><span data-stu-id="bdeef-149">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="bdeef-150">Bir temsilci seçenekleri değerleri ayarlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="bdeef-150">Use a delegate to set options values.</span></span> <span data-ttu-id="bdeef-151">Örnek uygulama kullandığı `MyOptionsWithDelegateConfig` sınıfı (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdeef-151">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="bdeef-152">Aşağıdaki kodda, ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmet, hizmet kapsayıcıya eklenir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-152">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bdeef-153">Bir temsilci ile yapılandırmak için kullandığı `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="bdeef-153">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="bdeef-154">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="bdeef-154">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="bdeef-155">Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bdeef-155">You can add multiple configuration providers.</span></span> <span data-ttu-id="bdeef-156">Yapılandırma sağlayıcıları NuGet paketleri kullanılabilir ve kayıtlı sırayla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-156">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="bdeef-157">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bdeef-157">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="bdeef-158">Her çağrı <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> ekler bir <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmet kapsayıcıya hizmet.</span><span class="sxs-lookup"><span data-stu-id="bdeef-158">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="bdeef-159">Yukarıdaki örnekte, değerlerini `Option1` ve `Option2` her ikisi de belirtilmiş *appsettings.json*, ancak değerlerini `Option1` ve `Option2` yapılandırılmış temsilci tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-159">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="bdeef-160">Son yapılandırma kaynağı birden fazla Yapılandırma hizmeti etkinleştirildiğinde, belirtilen *WINS* ve yapılandırma değeri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bdeef-160">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="bdeef-161">Ne zaman uygulamayı çalıştırın, sayfa modelin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bdeef-161">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="bdeef-162">Suboptions yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bdeef-162">Suboptions configuration</span></span>

<span data-ttu-id="bdeef-163">Suboptions yapılandırma, örnek olarak gösterilmiştir &num;örnek uygulamada 3.</span><span class="sxs-lookup"><span data-stu-id="bdeef-163">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="bdeef-164">Uygulamalar, uygulamadaki belirli bir senaryoyu grupları (sınıflar) ilgili seçenekleri sınıflar oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-164">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="bdeef-165">Yapılandırma değerleri gerektiren uygulama bölümleri, yalnızca kullandıkları yapılandırma değerleri için erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-165">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="bdeef-166">Seçenekleri yapılandırmayı bağlanırken, seçenek türünün her bir özellik form için bir yapılandırma anahtarı bağlı `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-166">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="bdeef-167">Örneğin, `MyOptions.Option1` özelliğe anahtarına `Option1`, den okunan `option1` özelliğinde *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bdeef-167">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="bdeef-168">Aşağıdaki kodda, üçüncü <xref:Microsoft.Extensions.Options.IConfigureOptions%601> hizmet, hizmet kapsayıcıya eklenir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-168">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="bdeef-169">Bunu bağlar `MySubOptions` bölümüne `subsection` , *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="bdeef-169">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="bdeef-170">`GetSection` Genişletme yöntemi gerektiren [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="bdeef-170">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="bdeef-171">Uygulama kullanıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri), paket otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-171">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="bdeef-172">Örnek'ın *appsettings.json* dosyasını tanımlayan bir `subsection` tuşları üyesiyle `suboption1` ve `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="bdeef-172">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="bdeef-173">`MySubOptions` Sınıf özelliklerini tanımlayan `SubOption1` ve `SubOption2`seçenekleri değerleri tutmak için (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdeef-173">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="bdeef-174">Sayfa modeli `OnGet` yöntemi seçenekleri değerlere sahip bir dize döndürür (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdeef-174">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="bdeef-175">Uygulamayı çalıştırdığınızda `OnGet` yöntemi suboption sınıfı değerleri gösteren bir dize döndürür:</span><span class="sxs-lookup"><span data-stu-id="bdeef-175">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="bdeef-176">Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri</span><span class="sxs-lookup"><span data-stu-id="bdeef-176">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="bdeef-177">Seçenekler ile doğrudan görünümü ekleme veya bir görünüm modeli tarafından sağlanan örnek olarak gösterilen &num;örnek uygulamada 4.</span><span class="sxs-lookup"><span data-stu-id="bdeef-177">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="bdeef-178">Bir görünüm modeli veya ekleme seçenekleri sağlanabilir <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> doğrudan bir görünüm (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdeef-178">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="bdeef-179">Örnek uygulama, ekleme işlemi gösterilmektedir `IOptionsMonitor<MyOptions>` ile bir `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="bdeef-179">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="bdeef-180">Uygulama çalıştırıldığında, oluşturulan sayfada seçenekleri değerler gösterilir:</span><span class="sxs-lookup"><span data-stu-id="bdeef-180">When the app is run, the options values are shown in the rendered page:</span></span>

![Seçenek değerleri Seçenek1: value1_from_json ve Seçenek2: -1 modelinden ve ekleme görünümüne tarafından yüklenir.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="bdeef-182">Yapılandırma verileri IOptionsSnapshot ile yeniden yükleyin</span><span class="sxs-lookup"><span data-stu-id="bdeef-182">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="bdeef-183">Yapılandırma verileri ile yeniden yüklemeyi <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> gösterildiği gibi &num;örnek uygulamada 5.</span><span class="sxs-lookup"><span data-stu-id="bdeef-183">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="bdeef-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> en az işlem ek yükü ile seçenekleri yeniden yüklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="bdeef-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="bdeef-185">Seçenekler, erişilebilir ve istek ömrü boyunca önbelleğe istek başına bir kez hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-185">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="bdeef-186">Aşağıdaki örnek, yeni bir nasıl gösterir <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> sonra oluşturulan *appsettings.json* değişiklikleri (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="bdeef-186">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="bdeef-187">Sunucuya birden çok istek tarafından sağlanan sabit değerler döndürür *appsettings.json* yapılandırma yeniden yükler ve dosya değiştirildiğinde kadar dosya.</span><span class="sxs-lookup"><span data-stu-id="bdeef-187">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="bdeef-188">Aşağıdaki resimde gösterilmektedir ilk `option1` ve `option2` yüklenen değerler *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="bdeef-188">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="bdeef-189">Değerlerinde değişiklik *appsettings.json* dosyasını `value1_from_json UPDATED` ve `200`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-189">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="bdeef-190">Kaydet *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="bdeef-190">Save the *appsettings.json* file.</span></span> <span data-ttu-id="bdeef-191">Seçenekleri değerleri güncelleştirildiğini görmek için tarayıcıyı yenileyin:</span><span class="sxs-lookup"><span data-stu-id="bdeef-191">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="bdeef-192">Adlı IConfigureNamedOptions seçenekleri desteği</span><span class="sxs-lookup"><span data-stu-id="bdeef-192">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="bdeef-193">Seçenekleri desteğiyle adlı <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> örnek olarak gösterilmiştir &num;örnek uygulamada 6.</span><span class="sxs-lookup"><span data-stu-id="bdeef-193">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="bdeef-194">*Seçenekleri adlı* adlandırılmış seçeneklerini yapılandırmaları arasında ayrım yapmak uygulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bdeef-194">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="bdeef-195">Örnek uygulamada, adlandırılmış seçenekleri ile bildirilen [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), çağıran [ConfigureNamedOptions\<TOptions >. Yapılandırma](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bdeef-195">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="bdeef-196">Örnek uygulamayı adlandırılmış seçeneklerle erişen <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdeef-196">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="bdeef-197">Örnek uygulamayı çalıştırma, adlandırılmış seçenekleri döndürülür:</span><span class="sxs-lookup"><span data-stu-id="bdeef-197">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="bdeef-198">`named_options_1` hangi yüklenen değerleri yapılandırmadan okuyoruz, sağlanan *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="bdeef-198">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="bdeef-199">`named_options_2` değerler tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="bdeef-199">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="bdeef-200">`named_options_2` İçindeki temsilci `ConfigureServices` için `Option1`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-200">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="bdeef-201">İçin varsayılan değer `Option2` tarafından sağlanan `MyOptions` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bdeef-201">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="bdeef-202">Tüm seçenekleri ConfigureAll yöntemi</span><span class="sxs-lookup"><span data-stu-id="bdeef-202">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="bdeef-203">Tüm seçenekleri örnekleriyle yapılandırma <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bdeef-203">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="bdeef-204">Aşağıdaki kod yapılandırır `Option1` için tüm yapılandırma örnekleri ortak bir değere sahip.</span><span class="sxs-lookup"><span data-stu-id="bdeef-204">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="bdeef-205">Aşağıdaki kodu el ile ekleyin `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bdeef-205">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="bdeef-206">Kod ekledikten sonra örnek uygulamayı çalıştırma aşağıdaki sonucu verir:</span><span class="sxs-lookup"><span data-stu-id="bdeef-206">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="bdeef-207">Tüm seçenekleri örneği olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-207">All options are named instances.</span></span> <span data-ttu-id="bdeef-208">Varolan <xref:Microsoft.Extensions.Options.IConfigureOptions%601> örnekleri hedefleyen olarak kabul edilir `Options.DefaultName` olan örneği `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-208">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="bdeef-209"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> Ayrıca uygulayan <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="bdeef-209"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="bdeef-210">Varsayılan uygulaması <xref:Microsoft.Extensions.Options.IOptionsFactory%601> her uygun şekilde kullanmak için mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-210">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="bdeef-211">`null` Adlandırılmış seçeneği tüm adlandırılmış örnek yerine adlandırılmış örneği belirli bir hedef için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).</span><span class="sxs-lookup"><span data-stu-id="bdeef-211">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="bdeef-212">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="bdeef-212">OptionsBuilder API</span></span>

<span data-ttu-id="bdeef-213"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> yapılandırmak için kullanılan `TOptions` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="bdeef-213"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="bdeef-214">`OptionsBuilder` yalnızca tek bir parametre için ilk olarak seçenekleri adlı oluşturma kolaylaştırır `AddOptions<TOptions>(string optionsName)` çağırmak yerine görünen tüm sonraki çağrılar.</span><span class="sxs-lookup"><span data-stu-id="bdeef-214">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="bdeef-215">Doğrulama seçenekleri ve `ConfigureOptions` Hizmet bağımlılıkları kabul eden aşırı yüklemeler aracılığıyla yalnızca `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-215">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="bdeef-216">Seçeneklerini yapılandırmak için DI hizmetlerini kullanma</span><span class="sxs-lookup"><span data-stu-id="bdeef-216">Use DI services to configure options</span></span>

<span data-ttu-id="bdeef-217">İki yolla Seçenekleri'ni yapılandırırken bağımlılık ekleme diğer hizmetlere erişebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bdeef-217">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="bdeef-218">Bir yapılandırma temsilciye geçirmek [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) üzerinde [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="bdeef-218">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="bdeef-219">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) aşırı sağlar [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) seçeneklerini yapılandırmak için en fazla beş kullanmanıza izin ver:</span><span class="sxs-lookup"><span data-stu-id="bdeef-219">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="bdeef-220">Uygulayan kendi tür oluşturma <xref:Microsoft.Extensions.Options.IConfigureOptions%601> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> ve türünü bir hizmet olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="bdeef-220">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="bdeef-221">Bir yapılandırma temsilciye geçirme öneririz [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), hizmet oluşturma daha karmaşık olduğundan.</span><span class="sxs-lookup"><span data-stu-id="bdeef-221">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="bdeef-222">Kendi tür oluşturma kullandığınızda framework sizin için ne yaptığını için eşdeğer [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="bdeef-222">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="bdeef-223">Çağırma [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) geçici genel kaydeder <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, genel hizmet türlerini kabul eden bir oluşturucuya belirtildiği.</span><span class="sxs-lookup"><span data-stu-id="bdeef-223">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="bdeef-224">Doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="bdeef-224">Options validation</span></span>

<span data-ttu-id="bdeef-225">Seçenekleri doğrulama seçenekleri yapılandırıldığında seçenekleri doğrulamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bdeef-225">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="bdeef-226">Çağrı `Validate` döndüren bir doğrulama yöntemiyle `true` seçenekleri geçerliyse ve `false` geçerli değilse:</span><span class="sxs-lookup"><span data-stu-id="bdeef-226">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="bdeef-227">Yukarıdaki örnekte adlandırılmış seçenekleri örneği ayarlar `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-227">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="bdeef-228">Varsayılan seçenekleri örneği `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-228">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="bdeef-229">Doğrulama seçenekleri örneği oluşturulduğunda çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-229">Validation runs when the options instance is created.</span></span> <span data-ttu-id="bdeef-230">Seçenekleri örneğinizin erişilebilir doğrulama ilk zaman geçmesi garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-230">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bdeef-231">Seçenekler başlangıçta yapılandırılmış ve doğrulanmış sonra seçeneklerini doğrulama seçenekleri değişikliklere karşı önlem değil.</span><span class="sxs-lookup"><span data-stu-id="bdeef-231">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="bdeef-232">`Validate` Yöntemi kabul bir `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-232">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="bdeef-233">Doğrulama tamamen özelleştirmek için uygulama `IValidateOptions<TOptions>`, sağlar:</span><span class="sxs-lookup"><span data-stu-id="bdeef-233">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="bdeef-234">Seçenekleri birden çok doğrulama: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="bdeef-234">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="bdeef-235">Bu doğrulama başka bir seçenek türüne bağlıdır: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="bdeef-235">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="bdeef-236">`IValidateOptions` doğrular:</span><span class="sxs-lookup"><span data-stu-id="bdeef-236">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="bdeef-237">Belirli bir adlandırılmış seçenekleri örneği.</span><span class="sxs-lookup"><span data-stu-id="bdeef-237">A specific named options instance.</span></span>
* <span data-ttu-id="bdeef-238">Tüm seçenekleri zaman `name` olduğu `null`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-238">All options when `name` is `null`.</span></span>

<span data-ttu-id="bdeef-239">Döndürür bir `ValidateOptionsResult` arabiriminin, bir uygulamadan:</span><span class="sxs-lookup"><span data-stu-id="bdeef-239">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="bdeef-240">Veri ek açıklama tabanlı doğrulama kullanılabilir [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) çağırarak paket `ValidateDataAnnotations` metodunda `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-240">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the `ValidateDataAnnotations` method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="bdeef-241">`Microsoft.Extensions.Options.DataAnnotations` yer aldığı [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="bdeef-241">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

```csharp
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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="bdeef-242">Gelecek sürümlerden meselesi istekli doğrulama (hızlı başlangıçta başarısız) altındadır.</span><span class="sxs-lookup"><span data-stu-id="bdeef-242">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

::: moniker-end

## <a name="options-post-configuration"></a><span data-ttu-id="bdeef-243">Yapılandırma sonrası seçenekleri</span><span class="sxs-lookup"><span data-stu-id="bdeef-243">Options post-configuration</span></span>

<span data-ttu-id="bdeef-244">Yapılandırma sonrası ile ayarlanmış <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="bdeef-244">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="bdeef-245">Yapılandırma sonrası çalıştıran tüm <xref:Microsoft.Extensions.Options.IConfigureOptions%601> yapılandırma gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="bdeef-245">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bdeef-246"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> Adlandırılmış seçeneklerini sonrası yapılandırmak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bdeef-246"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bdeef-247">Kullanım <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> sonrası tüm yapılandırma örnekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="bdeef-247">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="bdeef-248">Başlatma sırasında erişilebilirlik seçenekleri</span><span class="sxs-lookup"><span data-stu-id="bdeef-248">Accessing options during startup</span></span>

<span data-ttu-id="bdeef-249"><xref:Microsoft.Extensions.Options.IOptions%601> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> kullanılabilir `Startup.Configure`, hizmetleri önce oluşturulan bu yana `Configure` yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="bdeef-249"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="bdeef-250">Kullanmayın <xref:Microsoft.Extensions.Options.IOptions%601> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bdeef-250">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bdeef-251">Hizmet Kayıtları sıralama nedeniyle tutarsız seçenekleri durumu bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="bdeef-251">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bdeef-252">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bdeef-252">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
