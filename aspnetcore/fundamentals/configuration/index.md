---
title: "ASP.NET çekirdek yapılandırması"
author: rick-anderson
description: "Yapılandırma API'si tarafından birden çok yöntem ASP.NET Core uygulamayı yapılandırmak için kullanın."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 20c75d202d67a491890d87cebf549585e0313da0
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="6ea39-103">Bir ASP.NET Core uygulamayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6ea39-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="6ea39-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [işareti Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6ea39-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6ea39-105">Yapılandırma API'si ad-değer çiftlerinin listesini temel web uygulamasını ASP.NET Core yapılandırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="6ea39-106">Yapılandırma, birden fazla kaynaktan çalışma zamanında okuyun.</span><span class="sxs-lookup"><span data-stu-id="6ea39-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="6ea39-107">Ad-değer çiftleri çok düzeyli bir hiyerarşiye gruplandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="6ea39-108">İçin yapılandırma sağlayıcısı vardır:</span><span class="sxs-lookup"><span data-stu-id="6ea39-108">There are configuration providers for:</span></span>

* <span data-ttu-id="6ea39-109">Dosya biçimleri (INI, JSON ve XML)</span><span class="sxs-lookup"><span data-stu-id="6ea39-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="6ea39-110">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6ea39-110">Command-line arguments</span></span>
* <span data-ttu-id="6ea39-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6ea39-111">Environment variables</span></span>
* <span data-ttu-id="6ea39-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="6ea39-112">In-memory .NET objects</span></span>
* <span data-ttu-id="6ea39-113">Bir şifrelenmiş kullanıcı deposu</span><span class="sxs-lookup"><span data-stu-id="6ea39-113">An encrypted user store</span></span>
* [<span data-ttu-id="6ea39-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="6ea39-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="6ea39-115">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="6ea39-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="6ea39-116">Her yapılandırma değeri bir dize anahtarı eşler.</span><span class="sxs-lookup"><span data-stu-id="6ea39-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="6ea39-117">Özel bir ayarları seri durumdan çıkarılacak yerleşik bağlama Destek [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesne (basit bir .NET sınıf özelliklerine sahip).</span><span class="sxs-lookup"><span data-stu-id="6ea39-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="6ea39-118">Seçenekler düzeni seçenekleri sınıfları ilgili ayar gruplarını göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="6ea39-119">Seçenekler kullanılması daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="6ea39-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="6ea39-120">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6ea39-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="6ea39-121">JSON yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6ea39-121">JSON configuration</span></span>

<span data-ttu-id="6ea39-122">Aşağıdaki konsol uygulaması JSON yapılandırma sağlayıcısı kullanır:</span><span class="sxs-lookup"><span data-stu-id="6ea39-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="6ea39-123">Uygulama okur ve aşağıdaki yapılandırma ayarları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6ea39-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="6ea39-124">Hiyerarşik ad-değer çiftleri düğümleri bir virgülle ayrılmış listesini yapılandırması oluşur.</span><span class="sxs-lookup"><span data-stu-id="6ea39-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="6ea39-125">Bir değer almak için erişim `Configuration` karşılık gelen öğenin anahtarı ile dizinleyici:</span><span class="sxs-lookup"><span data-stu-id="6ea39-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="6ea39-126">JSON biçimli yapılandırma kaynaklarını dizilerde çalışmak için iki nokta üst üste ayrılmış dize bir parçası olarak bir dizi dizini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6ea39-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="6ea39-127">Aşağıdaki örnek önceki ilk öğe adını alır `wizards` dizi:</span><span class="sxs-lookup"><span data-stu-id="6ea39-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="6ea39-128">Ad-değer çiftleri için yerleşik yazılmış [yapılandırma](/dotnet/api/microsoft.extensions.configuration) sağlayıcılarıdır **değil** kalıcı.</span><span class="sxs-lookup"><span data-stu-id="6ea39-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="6ea39-129">Ancak, değerleri kaydeder özel bir sağlayıcı oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="6ea39-130">Bkz: [özel yapılandırma sağlayıcısının](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="6ea39-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="6ea39-131">Yukarıdaki örnek yapılandırma dizin oluşturucu değerleri okumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="6ea39-132">Erişim yapılandırması dışında `Startup`, kullanın *seçenekleri düzeni*.</span><span class="sxs-lookup"><span data-stu-id="6ea39-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="6ea39-133">Daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="6ea39-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="6ea39-134">Ortam yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6ea39-134">Configuration by environment</span></span>

<span data-ttu-id="6ea39-135">Örneğin, geliştirme, test ve üretim farklı ortamlar için farklı yapılandırma ayarlarını sağlamak için genel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="6ea39-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="6ea39-136">`CreateDefaultBuilder` Bir ASP.NET Core 2.x uygulamasında genişletme yöntemi (veya kullanarak `AddJsonFile` ve `AddEnvironmentVariables` bir ASP.NET Core 1.x uygulamasındaki doğrudan) JSON dosyaları ve sistem yapılandırma kaynaklarını okumak için bir yapılandırma sağlayıcısı ekler:</span><span class="sxs-lookup"><span data-stu-id="6ea39-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="6ea39-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="6ea39-137">*appsettings.json*</span></span>
* <span data-ttu-id="6ea39-138">*appSettings. \<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="6ea39-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="6ea39-139">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6ea39-139">Environment variables</span></span>

<span data-ttu-id="6ea39-140">ASP.NET Core 1.x uygulamalarına ihtiyacı çağırmak `AddJsonFile` ve [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="6ea39-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="6ea39-141">Bkz: [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) için bir açıklama parametreleri.</span><span class="sxs-lookup"><span data-stu-id="6ea39-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="6ea39-142">`reloadOnChange`yalnızca ASP.NET Core 1.1 ve sonraki sürümlerinde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="6ea39-143">Yapılandırma kaynaklarını belirtilen sırada salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="6ea39-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="6ea39-144">Önceki kod, ortam değişkenleri son salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="6ea39-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="6ea39-145">Herhangi bir yapılandırma değeri ortamı Değiştir iki önceki sağlayıcıları belirlenen ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="6ea39-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="6ea39-146">Aşağıdakileri göz önünde bulundurun *appsettings. Staging.JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6ea39-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="6ea39-147">Ortam ayarlandığında `Staging`, aşağıdaki `Configure` yöntemi okur değerini `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="6ea39-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="6ea39-148">Ortam genellikle ayarlamak `Development`, `Staging`, veya `Production`.</span><span class="sxs-lookup"><span data-stu-id="6ea39-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="6ea39-149">Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6ea39-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="6ea39-150">Yapılandırma dikkate alınacak noktalar:</span><span class="sxs-lookup"><span data-stu-id="6ea39-150">Configuration considerations:</span></span>

* <span data-ttu-id="6ea39-151">`IOptionsSnapshot`değişiklik yapıldığında yapılandırma verileri yeniden yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ea39-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="6ea39-152">Daha fazla bilgi için bkz: [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span><span class="sxs-lookup"><span data-stu-id="6ea39-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="6ea39-153">Yapılandırma anahtarları **değil** büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="6ea39-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="6ea39-154">**Hiçbir zaman** parolalar ve diğer hassas verileri yapılandırma sağlayıcısı kodu veya düz metin yapılandırma dosyalarını depolar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="6ea39-155">Verme geliştirme üretim gizlilikleri kullanın veya sınama ortamlarında.</span><span class="sxs-lookup"><span data-stu-id="6ea39-155">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="6ea39-156">Böylece, bir kaynak kod deposuna yanlışlıkla uygulanamıyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="6ea39-156">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="6ea39-157">Daha fazla bilgi edinmek [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) ve yönetme [geliştirme sırasında uygulama sırrı güvenli depolama](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="6ea39-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="6ea39-158">İki nokta varsa (`:`) olamaz bir sistem ortam değişkenlerini kullanıldığında, iki nokta üst üste değiştirin (`:`) ile bir çift alt çizgi (`__`).</span><span class="sxs-lookup"><span data-stu-id="6ea39-158">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="6ea39-159">Bellek içi sağlayıcısı ve bağlama için bir POCO sınıfı</span><span class="sxs-lookup"><span data-stu-id="6ea39-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="6ea39-160">Aşağıdaki örnek, bellek içi Sağlayıcısı'nı kullanın ve bir sınıfa bağlamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6ea39-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="6ea39-161">Yapılandırma değerleri dize olarak döndürülür, ancak bağlama nesnelerin yapımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="6ea39-162">Bağlama POCO nesneleri veya hatta tüm nesne grafiklerinin alınmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-162">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="6ea39-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="6ea39-163">GetValue</span></span>

<span data-ttu-id="6ea39-164">Aşağıdaki örnek gösterilmektedir [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6ea39-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="6ea39-165">ConfigurationBinder's `GetValue<T>` yöntemi, varsayılan değer (80 örnekteki) belirtimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-165">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="6ea39-166">`GetValue<T>`Basit senaryolar için ve tüm bölümleri bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="6ea39-166">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="6ea39-167">`GetValue<T>`skaler değerleri alır `GetSection(key).Value` belirli bir türüne dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="6ea39-167">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="6ea39-168">Bir nesne grafiğinin bağlama</span><span class="sxs-lookup"><span data-stu-id="6ea39-168">Bind to an object graph</span></span>

<span data-ttu-id="6ea39-169">Bir sınıftaki her nesneyi yinelemeli olarak bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-169">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="6ea39-170">Aşağıdakileri göz önünde bulundurun `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="6ea39-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="6ea39-171">Aşağıdaki örnek bağlar `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="6ea39-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="6ea39-172">**ASP.NET Core 1.1** ve daha yüksek kullanabilirsiniz `Get<T>`, tüm bölümleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="6ea39-173">`Get<T>`kullanmaktan daha kullanışlı olabilir `Bind`.</span><span class="sxs-lookup"><span data-stu-id="6ea39-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="6ea39-174">Aşağıdaki kodu nasıl kullanılacağını gösterir `Get<T>` önceki örnekle:</span><span class="sxs-lookup"><span data-stu-id="6ea39-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="6ea39-175">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6ea39-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="6ea39-176">Program görüntüler `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="6ea39-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="6ea39-177">Aşağıdaki kod birimine kullanılabilir test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="6ea39-177">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="6ea39-178">Entity Framework özel sağlayıcı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6ea39-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="6ea39-179">Bu bölümde, ad-değer çiftleri EF kullanan bir veritabanından okur bir temel yapılandırma sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6ea39-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="6ea39-180">Tanımlayan bir `ConfigurationValue` yapılandırma değerlerini veritabanında depolamak için varlık:</span><span class="sxs-lookup"><span data-stu-id="6ea39-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="6ea39-181">Ekleme bir `ConfigurationContext` depolamak ve yapılandırılmış değerlerine erişmek için:</span><span class="sxs-lookup"><span data-stu-id="6ea39-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="6ea39-182">Arabirimini uygulayan bir sınıf oluşturun [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="6ea39-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="6ea39-183">Özel yapılandırma sağlayıcısının devralarak oluşturma [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="6ea39-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="6ea39-184">Boş olduğunda yapılandırma sağlayıcısı veritabanı başlatır:</span><span class="sxs-lookup"><span data-stu-id="6ea39-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="6ea39-185">Örneği çalıştırdığınızda ("value_from_ef_1" ve "value_from_ef_2") veritabanından vurgulanan değerler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="6ea39-186">Bir `EFConfigSource` genişletme yöntemi yapılandırma kaynağı eklemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="6ea39-186">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="6ea39-187">Aşağıdaki kod özel kullanmayı gösterir `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="6ea39-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="6ea39-188">Örnek ekler özel Not `EFConfigProvider` JSON sağlayıcısı sonra bu nedenle veritabanından herhangi bir ayarı geçersiz kılar ayarlarından *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="6ea39-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="6ea39-189">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6ea39-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="6ea39-190">Şu çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6ea39-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="6ea39-191">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6ea39-191">CommandLine configuration provider</span></span>

<span data-ttu-id="6ea39-192">[CommandLine yapılandırma sağlayıcısı](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) çalışma zamanında yapılandırması için komut satırı bağımsız değişkeni anahtar-değer çiftleri alır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="6ea39-193">Görüntülemek veya komut satırı yapılandırma örneği indirin</span><span class="sxs-lookup"><span data-stu-id="6ea39-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="6ea39-194">Kurulum ve komut satırı yapılandırma sağlayıcısı kullanın</span><span class="sxs-lookup"><span data-stu-id="6ea39-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="6ea39-195">Temel yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6ea39-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="6ea39-196">Komut satırı yapılandırmasını etkinleştirmek için arama `AddCommandLine` genişletme yöntemi örneği üzerinde [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="6ea39-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="6ea39-197">Kod çalıştırmadan, aşağıdaki çıkış görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6ea39-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="6ea39-198">Komut satırında anahtar-değer çiftleri bağımsız değişken geçirme değerlerini değiştirir `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="6ea39-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="6ea39-199">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6ea39-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="6ea39-200">Komut satırı yapılandırması ile diğer yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için arama `AddCommandLine` üzerinde son `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="6ea39-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ea39-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ea39-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6ea39-202">Tipik ASP.NET Core 2.x uygulamaları kullanma statik kolaylık metodunun `CreateDefaultBuilder` konak oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="6ea39-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="6ea39-203">`CreateDefaultBuilder`İsteğe bağlı yapılandırmasından yükler *appsettings.json*, *appsettings. { Ortam} .json*, [kullanıcı parolaları](xref:security/app-secrets) (içinde `Development` ortamı), ortam değişkenleri ve komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="6ea39-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="6ea39-204">Komut satırı yapılandırma sağlayıcısı son çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="6ea39-205">Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="6ea39-206">İçin *appsettings* where dosyaları:</span><span class="sxs-lookup"><span data-stu-id="6ea39-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="6ea39-207">`reloadOnChange`etkin.</span><span class="sxs-lookup"><span data-stu-id="6ea39-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="6ea39-208">Komut satırı bağımsız değişkenleri aynı ayar içerir ve bir *appsettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="6ea39-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="6ea39-209">*Appsettings* eşleşen komut satırı bağımsız değişkeni içeren bir dosya, uygulama başladıktan sonra değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="6ea39-210">Önceki koşulların tümü doğruysa, komut satırı bağımsız değişkenleri geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="6ea39-211">ASP.NET Core 2.x uygulama kullanabileceğiniz [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) yerine `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6ea39-211">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6ea39-212">Kullanırken `WebHostBuilder`, el ile kümesi yapılandırması ile [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="6ea39-212">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="6ea39-213">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="6ea39-213">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ea39-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ea39-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6ea39-215">Oluşturma bir [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ve arama `AddCommandLine` yöntemi CommandLine yapılandırma sağlayıcısı kullanın.</span><span class="sxs-lookup"><span data-stu-id="6ea39-215">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="6ea39-216">Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-216">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="6ea39-217">Yapılandırmasını uygulamak [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6ea39-217">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="6ea39-218">Arguments</span><span class="sxs-lookup"><span data-stu-id="6ea39-218">Arguments</span></span>

<span data-ttu-id="6ea39-219">Komut satırına geçirilen bağımsız değişkenler aşağıdaki tabloda gösterilen iki biçim birine uymalıdır:</span><span class="sxs-lookup"><span data-stu-id="6ea39-219">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="6ea39-220">Bağımsız değişken biçimi</span><span class="sxs-lookup"><span data-stu-id="6ea39-220">Argument format</span></span>                                                     | <span data-ttu-id="6ea39-221">Örnek</span><span class="sxs-lookup"><span data-stu-id="6ea39-221">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="6ea39-222">Tek bağımsız değişken: bir anahtar-değer çifti ayrılmış bir eşittir işareti (`=`)</span><span class="sxs-lookup"><span data-stu-id="6ea39-222">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="6ea39-223">İki bağımsız değişken bir dizi: boşlukla ayrılmış bir anahtar-değer çifti</span><span class="sxs-lookup"><span data-stu-id="6ea39-223">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="6ea39-224">**Tek bir bağımsız değişken**</span><span class="sxs-lookup"><span data-stu-id="6ea39-224">**Single argument**</span></span>

<span data-ttu-id="6ea39-225">Değer bir eşittir işareti gelmelidir (`=`).</span><span class="sxs-lookup"><span data-stu-id="6ea39-225">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="6ea39-226">Değer null olabilir (örneğin, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="6ea39-226">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="6ea39-227">Anahtar bir önek olabilir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-227">The key may have a prefix.</span></span>

| <span data-ttu-id="6ea39-228">Anahtar öneki</span><span class="sxs-lookup"><span data-stu-id="6ea39-228">Key prefix</span></span>               | <span data-ttu-id="6ea39-229">Örnek</span><span class="sxs-lookup"><span data-stu-id="6ea39-229">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="6ea39-230">Önek</span><span class="sxs-lookup"><span data-stu-id="6ea39-230">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="6ea39-231">Tek bir tire (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="6ea39-231">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="6ea39-232">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="6ea39-232">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="6ea39-233">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="6ea39-233">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="6ea39-234">&#8224; Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-234">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="6ea39-235">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="6ea39-235">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="6ea39-236">Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6ea39-236">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="6ea39-237">**İki bağımsız değişken dizisi**</span><span class="sxs-lookup"><span data-stu-id="6ea39-237">**Sequence of two arguments**</span></span>

<span data-ttu-id="6ea39-238">Değer null olamaz ve boşlukla ayrılmış anahtar izlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-238">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="6ea39-239">Anahtar bir önekine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-239">The key must have a prefix.</span></span>

| <span data-ttu-id="6ea39-240">Anahtar öneki</span><span class="sxs-lookup"><span data-stu-id="6ea39-240">Key prefix</span></span>               | <span data-ttu-id="6ea39-241">Örnek</span><span class="sxs-lookup"><span data-stu-id="6ea39-241">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="6ea39-242">Tek bir tire (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="6ea39-242">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="6ea39-243">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="6ea39-243">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="6ea39-244">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="6ea39-244">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="6ea39-245">&#8224; Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-245">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="6ea39-246">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="6ea39-246">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="6ea39-247">Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6ea39-247">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="6ea39-248">Yinelenen anahtarlar</span><span class="sxs-lookup"><span data-stu-id="6ea39-248">Duplicate keys</span></span>

<span data-ttu-id="6ea39-249">Yinelenen anahtarlarla verdiyse, son anahtar-değer çifti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-249">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="6ea39-250">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="6ea39-250">Switch mappings</span></span>

<span data-ttu-id="6ea39-251">El ile yapılandırma ile oluştururken `ConfigurationBuilder`, bir anahtar eşlemeleri sözlüğü eklenebilir `AddCommandLine` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6ea39-251">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="6ea39-252">Anahtar eşlemeleri anahtar adı değiştirme mantığı izin verir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-252">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="6ea39-253">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük komut satırı bağımsız değişkeni tarafından sağlanan anahtarıyla eşleşen bir anahtarı için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-253">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="6ea39-254">Komut satırı anahtarı sözlük içinde bulunursa, sözlük değeri (Anahtar değişimini) yapılandırma döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-254">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="6ea39-255">Tek bir çizgiyle önekli herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="6ea39-255">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="6ea39-256">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="6ea39-256">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="6ea39-257">Anahtarları kısa çizgiyle başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="6ea39-257">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="6ea39-258">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-258">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="6ea39-259">Aşağıdaki örnekte, `GetSwitchMappings` yöntemi sağlayan tek bir çizgi kullanmak komut satırı bağımsız değişkenleri (`-`) anahtarı öneki ve önde gelen alt anahtar önekleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="6ea39-259">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="6ea39-260">Komut satırı bağımsız değişkenleri sağlamadan sözlüğü için sağlanan `AddInMemoryCollection` yapılandırma değerlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="6ea39-260">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="6ea39-261">Uygulama ile aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6ea39-261">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="6ea39-262">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6ea39-262">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="6ea39-263">Yapılandırma ayarlarını geçirmek için aşağıdakileri kullanın:</span><span class="sxs-lookup"><span data-stu-id="6ea39-263">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="6ea39-264">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6ea39-264">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="6ea39-265">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra aşağıdaki tabloda gösterilen veriler içerir:</span><span class="sxs-lookup"><span data-stu-id="6ea39-265">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="6ea39-266">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6ea39-266">Key</span></span>            | <span data-ttu-id="6ea39-267">Değer</span><span class="sxs-lookup"><span data-stu-id="6ea39-267">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="6ea39-268">Anahtar geçişi sözlüğünü kullanarak göstermek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6ea39-268">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="6ea39-269">Komut satırı anahtarları değiştirilen.</span><span class="sxs-lookup"><span data-stu-id="6ea39-269">The command-line keys are swapped.</span></span> <span data-ttu-id="6ea39-270">Konsol penceresinde yapılandırma değerlerini görüntüler `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="6ea39-270">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="6ea39-271">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="6ea39-271">The web.config file</span></span>

<span data-ttu-id="6ea39-272">A *web.config* dosya, IIS veya IIS Express uygulamasında barındırdığında gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-272">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="6ea39-273">Ayarlarında *web.config* etkinleştirmek [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) uygulamayı başlatın ve diğer IIS ayarlarını ve modülleri yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="6ea39-273">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="6ea39-274">Varsa *web.config* dosyası mevcut değil ve proje dosyasını içeren `<Project Sdk="Microsoft.NET.Sdk.Web">`, projeyi yayımlama oluşturur bir *web.config* yayımlanan çıktı dosyasında ( *Yayımlama* klasörü).</span><span class="sxs-lookup"><span data-stu-id="6ea39-274">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="6ea39-275">Daha fazla bilgi için bkz: [konak ASP.NET Core IIS ile Windows](xref:host-and-deploy/iis/index#webconfig).</span><span class="sxs-lookup"><span data-stu-id="6ea39-275">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="6ea39-276">Başlatma sırasında yapılandırma erişme</span><span class="sxs-lookup"><span data-stu-id="6ea39-276">Accessing configuration during startup</span></span>

<span data-ttu-id="6ea39-277">Erişim Yapılandırması içinde `ConfigureServices` veya `Configure` başlatma sırasında örneklere bakın [uygulama başlangıç](xref:fundamentals/startup) konu.</span><span class="sxs-lookup"><span data-stu-id="6ea39-277">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="6ea39-278">Ek Notlar</span><span class="sxs-lookup"><span data-stu-id="6ea39-278">Additional notes</span></span>

* <span data-ttu-id="6ea39-279">Bağımlılık ekleme (dı) değil ayarlamak kadar yukarı sonra `ConfigureServices` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-279">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="6ea39-280">Yapılandırma sistemi dı farkında değildir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-280">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="6ea39-281">`IConfiguration`iki özelleştirmeleri sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6ea39-281">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="6ea39-282">`IConfigurationRoot`Kök düğüm için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-282">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="6ea39-283">Bir yeniden yükleme tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6ea39-283">Can trigger a reload.</span></span>
  * <span data-ttu-id="6ea39-284">`IConfigurationSection`Yapılandırma değerlerini bir bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6ea39-284">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="6ea39-285">`GetSection` Ve `GetChildren` yöntemleri döndürür bir `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="6ea39-285">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="6ea39-286">Kullanım [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) yapılandırma yeniden yükleme veya erişim her sağlayıcı için.</span><span class="sxs-lookup"><span data-stu-id="6ea39-286">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="6ea39-287">Bunlardan hiçbirine yaygındır.</span><span class="sxs-lookup"><span data-stu-id="6ea39-287">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ea39-288">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6ea39-288">Additional resources</span></span>

* [<span data-ttu-id="6ea39-289">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="6ea39-289">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="6ea39-290">Birden çok ortamı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="6ea39-290">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="6ea39-291">Geliştirme sırasında uygulama gizli anahtarlarının güvenli bir şekilde depolanması</span><span class="sxs-lookup"><span data-stu-id="6ea39-291">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="6ea39-292">ASP.NET çekirdek barındırma</span><span class="sxs-lookup"><span data-stu-id="6ea39-292">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="6ea39-293">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="6ea39-293">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="6ea39-294">Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6ea39-294">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
