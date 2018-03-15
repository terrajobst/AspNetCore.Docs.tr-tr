---
title: "ASP.NET çekirdek yapılandırması"
author: rick-anderson
description: "Yapılandırma API'si tarafından birden çok yöntem ASP.NET Core uygulamayı yapılandırmak için kullanın."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 7c41621db835b452c9aad9463a9ffccdf0c06484
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="51f97-103">Bir ASP.NET Core uygulamayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51f97-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="51f97-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [işareti Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="51f97-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="51f97-105">Yapılandırma API'si ad-değer çiftlerinin listesini temel web uygulamasını ASP.NET Core yapılandırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="51f97-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="51f97-106">Yapılandırma, birden fazla kaynaktan çalışma zamanında okuyun.</span><span class="sxs-lookup"><span data-stu-id="51f97-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="51f97-107">Ad-değer çiftleri çok düzeyli bir hiyerarşiye gruplandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="51f97-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="51f97-108">İçin yapılandırma sağlayıcısı vardır:</span><span class="sxs-lookup"><span data-stu-id="51f97-108">There are configuration providers for:</span></span>

* <span data-ttu-id="51f97-109">Dosya biçimleri (INI, JSON ve XML).</span><span class="sxs-lookup"><span data-stu-id="51f97-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="51f97-110">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="51f97-110">Command-line arguments.</span></span>
* <span data-ttu-id="51f97-111">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="51f97-111">Environment variables.</span></span>
* <span data-ttu-id="51f97-112">Bellek içi .NET nesneleri.</span><span class="sxs-lookup"><span data-stu-id="51f97-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="51f97-113">Şifrelenmemiş [gizli Yöneticisi](xref:security/app-secrets) depolama.</span><span class="sxs-lookup"><span data-stu-id="51f97-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="51f97-114">Şifrelenmiş bir kullanıcı depolamak, gibi [Azure anahtar kasası](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="51f97-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="51f97-115">(Yüklü veya oluşturulan) özel sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="51f97-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="51f97-116">Her yapılandırma değeri bir dize anahtarı eşler.</span><span class="sxs-lookup"><span data-stu-id="51f97-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="51f97-117">Özel bir ayarları seri durumdan çıkarılacak yerleşik bağlama Destek [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesne (basit bir .NET sınıf özelliklerine sahip).</span><span class="sxs-lookup"><span data-stu-id="51f97-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="51f97-118">Seçenekler düzeni seçenekleri sınıfları ilgili ayar gruplarını göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="51f97-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="51f97-119">Seçenekler kullanılması daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="51f97-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="51f97-120">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51f97-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="51f97-121">JSON yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51f97-121">JSON configuration</span></span>

<span data-ttu-id="51f97-122">Aşağıdaki konsol uygulaması JSON yapılandırma sağlayıcısı kullanır:</span><span class="sxs-lookup"><span data-stu-id="51f97-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="51f97-123">Uygulama okur ve aşağıdaki yapılandırma ayarları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="51f97-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="51f97-124">Hiyerarşik ad-değer çiftleri düğümleri bir virgülle ayrılmış listesini yapılandırması oluşur.</span><span class="sxs-lookup"><span data-stu-id="51f97-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="51f97-125">Bir değer almak için erişim `Configuration` karşılık gelen öğenin anahtarı ile dizinleyici:</span><span class="sxs-lookup"><span data-stu-id="51f97-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="51f97-126">JSON biçimli yapılandırma kaynaklarını dizilerde çalışmak için iki nokta üst üste ayrılmış dize bir parçası olarak bir dizi dizini kullanın.</span><span class="sxs-lookup"><span data-stu-id="51f97-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="51f97-127">Aşağıdaki örnek önceki ilk öğe adını alır `wizards` dizi:</span><span class="sxs-lookup"><span data-stu-id="51f97-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="51f97-128">Ad-değer çiftleri için yerleşik yazılmış [yapılandırma](/dotnet/api/microsoft.extensions.configuration) sağlayıcılarıdır **değil** kalıcı.</span><span class="sxs-lookup"><span data-stu-id="51f97-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="51f97-129">Ancak, değerleri kaydeder özel bir sağlayıcı oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="51f97-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="51f97-130">Bkz: [özel yapılandırma sağlayıcısının](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="51f97-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="51f97-131">Yukarıdaki örnek yapılandırma dizin oluşturucu değerleri okumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="51f97-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="51f97-132">Erişim yapılandırması dışında `Startup`, kullanın *seçenekleri düzeni*.</span><span class="sxs-lookup"><span data-stu-id="51f97-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="51f97-133">Daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="51f97-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="51f97-134">XML yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51f97-134">XML configuration</span></span>

<span data-ttu-id="51f97-135">XML biçimli yapılandırma kaynaklarını dizilerde çalışmak için sağlayan bir `name` her öğenin dizini.</span><span class="sxs-lookup"><span data-stu-id="51f97-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="51f97-136">Dizin değerlerine erişmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="51f97-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="51f97-137">Ortam yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51f97-137">Configuration by environment</span></span>

<span data-ttu-id="51f97-138">Örneğin, geliştirme, test ve üretim farklı ortamlar için farklı yapılandırma ayarlarını sağlamak için genel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="51f97-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="51f97-139">`CreateDefaultBuilder` Bir ASP.NET Core 2.x uygulamasında genişletme yöntemi (veya kullanarak `AddJsonFile` ve `AddEnvironmentVariables` bir ASP.NET Core 1.x uygulamasındaki doğrudan) JSON dosyaları ve sistem yapılandırma kaynaklarını okumak için bir yapılandırma sağlayıcısı ekler:</span><span class="sxs-lookup"><span data-stu-id="51f97-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="51f97-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="51f97-140">*appsettings.json*</span></span>
* <span data-ttu-id="51f97-141">*appSettings. \<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="51f97-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="51f97-142">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="51f97-142">Environment variables</span></span>

<span data-ttu-id="51f97-143">ASP.NET Core 1.x uygulamalarına ihtiyacı çağırmak `AddJsonFile` ve [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="51f97-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="51f97-144">Bkz: [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) için bir açıklama parametreleri.</span><span class="sxs-lookup"><span data-stu-id="51f97-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="51f97-145">`reloadOnChange` yalnızca ASP.NET Core 1.1 ve sonraki sürümlerinde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="51f97-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="51f97-146">Yapılandırma kaynaklarını belirtilen sırada salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="51f97-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="51f97-147">Önceki kod, ortam değişkenleri son salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="51f97-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="51f97-148">Herhangi bir yapılandırma değeri ortamı Değiştir iki önceki sağlayıcıları belirlenen ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="51f97-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="51f97-149">Aşağıdakileri göz önünde bulundurun *appsettings. Staging.JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="51f97-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="51f97-150">Ortam ayarlandığında `Staging`, aşağıdaki `Configure` yöntemi okur değerini `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="51f97-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="51f97-151">Ortam genellikle ayarlamak `Development`, `Staging`, veya `Production`.</span><span class="sxs-lookup"><span data-stu-id="51f97-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="51f97-152">Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="51f97-152">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="51f97-153">Yapılandırma dikkate alınacak noktalar:</span><span class="sxs-lookup"><span data-stu-id="51f97-153">Configuration considerations:</span></span>

* <span data-ttu-id="51f97-154">`IOptionsSnapshot` değişiklik yapıldığında yapılandırma verileri yeniden yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51f97-154">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="51f97-155">Daha fazla bilgi için bkz: [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span><span class="sxs-lookup"><span data-stu-id="51f97-155">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="51f97-156">Yapılandırma anahtarları **değil** büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="51f97-156">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="51f97-157">**Hiçbir zaman** parolalar ve diğer hassas verileri yapılandırma sağlayıcısı kodu veya düz metin yapılandırma dosyalarını depolar.</span><span class="sxs-lookup"><span data-stu-id="51f97-157">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="51f97-158">Verme geliştirme üretim gizlilikleri kullanın veya sınama ortamlarında.</span><span class="sxs-lookup"><span data-stu-id="51f97-158">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="51f97-159">Böylece, bir kaynak kod deposuna yanlışlıkla uygulanamıyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="51f97-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="51f97-160">Daha fazla bilgi edinmek [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) ve yönetme [geliştirme sırasında uygulama sırrı güvenli depolama](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="51f97-160">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="51f97-161">İki nokta varsa (`:`) olamaz bir sistem ortam değişkenlerini kullanıldığında, iki nokta üst üste değiştirin (`:`) ile bir çift alt çizgi (`__`).</span><span class="sxs-lookup"><span data-stu-id="51f97-161">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="51f97-162">Bellek içi sağlayıcısı ve bağlama için bir POCO sınıfı</span><span class="sxs-lookup"><span data-stu-id="51f97-162">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="51f97-163">Aşağıdaki örnek, bellek içi Sağlayıcısı'nı kullanın ve bir sınıfa bağlamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="51f97-163">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="51f97-164">Yapılandırma değerleri dize olarak döndürülür, ancak bağlama nesnelerin yapımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="51f97-164">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="51f97-165">Bağlama POCO nesneleri veya hatta tüm nesne grafiklerinin alınmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="51f97-165">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="51f97-166">GetValue</span><span class="sxs-lookup"><span data-stu-id="51f97-166">GetValue</span></span>

<span data-ttu-id="51f97-167">Aşağıdaki örnek gösterilmektedir [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="51f97-167">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="51f97-168">ConfigurationBinder's `GetValue<T>` yöntemi, varsayılan değer (80 örnekteki) belirtimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="51f97-168">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="51f97-169">`GetValue<T>` Basit senaryolar için ve tüm bölümleri bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="51f97-169">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="51f97-170">`GetValue<T>` skaler değerleri alır `GetSection(key).Value` belirli bir türüne dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="51f97-170">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="51f97-171">Bir nesne grafiğinin bağlama</span><span class="sxs-lookup"><span data-stu-id="51f97-171">Bind to an object graph</span></span>

<span data-ttu-id="51f97-172">Bir sınıftaki her nesneyi yinelemeli olarak bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="51f97-172">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="51f97-173">Aşağıdakileri göz önünde bulundurun `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="51f97-173">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="51f97-174">Aşağıdaki örnek bağlar `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="51f97-174">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="51f97-175">**ASP.NET Core 1.1** ve daha yüksek kullanabilirsiniz `Get<T>`, tüm bölümleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="51f97-175">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="51f97-176">`Get<T>` kullanmaktan daha kullanışlı olabilir `Bind`.</span><span class="sxs-lookup"><span data-stu-id="51f97-176">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="51f97-177">Aşağıdaki kodu nasıl kullanılacağını gösterir `Get<T>` önceki örnekle:</span><span class="sxs-lookup"><span data-stu-id="51f97-177">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="51f97-178">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="51f97-178">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="51f97-179">Program görüntüler `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="51f97-179">The program displays `Height 11`.</span></span>

<span data-ttu-id="51f97-180">Aşağıdaki kod birimine kullanılabilir test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="51f97-180">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="51f97-181">Entity Framework özel sağlayıcı oluşturma</span><span class="sxs-lookup"><span data-stu-id="51f97-181">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="51f97-182">Bu bölümde, ad-değer çiftleri EF kullanan bir veritabanından okur bir temel yapılandırma sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="51f97-182">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="51f97-183">Tanımlayan bir `ConfigurationValue` yapılandırma değerlerini veritabanında depolamak için varlık:</span><span class="sxs-lookup"><span data-stu-id="51f97-183">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="51f97-184">Ekleme bir `ConfigurationContext` depolamak ve yapılandırılmış değerlerine erişmek için:</span><span class="sxs-lookup"><span data-stu-id="51f97-184">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="51f97-185">Arabirimini uygulayan bir sınıf oluşturun [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="51f97-185">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="51f97-186">Özel yapılandırma sağlayıcısının devralarak oluşturma [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="51f97-186">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="51f97-187">Boş olduğunda yapılandırma sağlayıcısı veritabanı başlatır:</span><span class="sxs-lookup"><span data-stu-id="51f97-187">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="51f97-188">Örneği çalıştırdığınızda ("value_from_ef_1" ve "value_from_ef_2") veritabanından vurgulanan değerler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="51f97-188">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="51f97-189">Bir `EFConfigSource` genişletme yöntemi yapılandırma kaynağı eklemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="51f97-189">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="51f97-190">Aşağıdaki kod özel kullanmayı gösterir `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="51f97-190">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="51f97-191">Örnek ekler özel Not `EFConfigProvider` JSON sağlayıcısı sonra bu nedenle veritabanından herhangi bir ayarı geçersiz kılar ayarlarından *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="51f97-191">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="51f97-192">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="51f97-192">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="51f97-193">Şu çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="51f97-193">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="51f97-194">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="51f97-194">CommandLine configuration provider</span></span>

<span data-ttu-id="51f97-195">[CommandLine yapılandırma sağlayıcısı](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) çalışma zamanında yapılandırması için komut satırı bağımsız değişkeni anahtar-değer çiftleri alır.</span><span class="sxs-lookup"><span data-stu-id="51f97-195">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="51f97-196">Görüntülemek veya komut satırı yapılandırma örneği indirin</span><span class="sxs-lookup"><span data-stu-id="51f97-196">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="51f97-197">Kurulum ve komut satırı yapılandırma sağlayıcısı kullanın</span><span class="sxs-lookup"><span data-stu-id="51f97-197">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="51f97-198">Temel yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51f97-198">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="51f97-199">Komut satırı yapılandırmasını etkinleştirmek için arama `AddCommandLine` genişletme yöntemi örneği üzerinde [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="51f97-199">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="51f97-200">Kod çalıştırmadan, aşağıdaki çıkış görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="51f97-200">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="51f97-201">Komut satırında anahtar-değer çiftleri bağımsız değişken geçirme değerlerini değiştirir `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="51f97-201">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="51f97-202">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="51f97-202">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="51f97-203">Komut satırı yapılandırması ile diğer yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için arama `AddCommandLine` üzerinde son `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="51f97-203">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51f97-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51f97-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="51f97-205">Tipik ASP.NET Core 2.x uygulamaları kullanma statik kolaylık metodunun `CreateDefaultBuilder` konak oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="51f97-205">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="51f97-206">`CreateDefaultBuilder` İsteğe bağlı yapılandırmasından yükler *appsettings.json*, *appsettings. { Ortam} .json*, [kullanıcı parolaları](xref:security/app-secrets) (içinde `Development` ortamı), ortam değişkenleri ve komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="51f97-206">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="51f97-207">Komut satırı yapılandırma sağlayıcısı son çağrılır.</span><span class="sxs-lookup"><span data-stu-id="51f97-207">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="51f97-208">Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="51f97-208">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="51f97-209">İçin *appsettings* where dosyaları:</span><span class="sxs-lookup"><span data-stu-id="51f97-209">For *appsettings* files where:</span></span>

* <span data-ttu-id="51f97-210">`reloadOnChange` etkin.</span><span class="sxs-lookup"><span data-stu-id="51f97-210">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="51f97-211">Komut satırı bağımsız değişkenleri aynı ayar içerir ve bir *appsettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="51f97-211">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="51f97-212">*Appsettings* eşleşen komut satırı bağımsız değişkeni içeren bir dosya, uygulama başladıktan sonra değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="51f97-212">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="51f97-213">Önceki koşulların tümü doğruysa, komut satırı bağımsız değişkenleri geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="51f97-213">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="51f97-214">ASP.NET Core 2.x uygulama kullanabileceğiniz [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) yerine `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="51f97-214">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="51f97-215">Kullanırken `WebHostBuilder`, el ile kümesi yapılandırması ile [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="51f97-215">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="51f97-216">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="51f97-216">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51f97-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51f97-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="51f97-218">Oluşturma bir [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ve arama `AddCommandLine` yöntemi CommandLine yapılandırma sağlayıcısı kullanın.</span><span class="sxs-lookup"><span data-stu-id="51f97-218">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="51f97-219">Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="51f97-219">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="51f97-220">Yapılandırmasını uygulamak [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="51f97-220">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="51f97-221">Arguments</span><span class="sxs-lookup"><span data-stu-id="51f97-221">Arguments</span></span>

<span data-ttu-id="51f97-222">Komut satırına geçirilen bağımsız değişkenler aşağıdaki tabloda gösterilen iki biçim birine uymalıdır:</span><span class="sxs-lookup"><span data-stu-id="51f97-222">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="51f97-223">Bağımsız değişken biçimi</span><span class="sxs-lookup"><span data-stu-id="51f97-223">Argument format</span></span>                                                     | <span data-ttu-id="51f97-224">Örnek</span><span class="sxs-lookup"><span data-stu-id="51f97-224">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="51f97-225">Tek bağımsız değişken: bir anahtar-değer çifti ayrılmış bir eşittir işareti (`=`)</span><span class="sxs-lookup"><span data-stu-id="51f97-225">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="51f97-226">İki bağımsız değişken bir dizi: boşlukla ayrılmış bir anahtar-değer çifti</span><span class="sxs-lookup"><span data-stu-id="51f97-226">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="51f97-227">**Tek bir bağımsız değişken**</span><span class="sxs-lookup"><span data-stu-id="51f97-227">**Single argument**</span></span>

<span data-ttu-id="51f97-228">Değer bir eşittir işareti gelmelidir (`=`).</span><span class="sxs-lookup"><span data-stu-id="51f97-228">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="51f97-229">Değer null olabilir (örneğin, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="51f97-229">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="51f97-230">Anahtar bir önek olabilir.</span><span class="sxs-lookup"><span data-stu-id="51f97-230">The key may have a prefix.</span></span>

| <span data-ttu-id="51f97-231">Anahtar öneki</span><span class="sxs-lookup"><span data-stu-id="51f97-231">Key prefix</span></span>               | <span data-ttu-id="51f97-232">Örnek</span><span class="sxs-lookup"><span data-stu-id="51f97-232">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="51f97-233">Önek</span><span class="sxs-lookup"><span data-stu-id="51f97-233">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="51f97-234">Tek bir tire (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="51f97-234">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="51f97-235">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="51f97-235">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="51f97-236">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="51f97-236">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="51f97-237">&#8224;Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="51f97-237">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="51f97-238">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="51f97-238">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="51f97-239">Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="51f97-239">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="51f97-240">**İki bağımsız değişken dizisi**</span><span class="sxs-lookup"><span data-stu-id="51f97-240">**Sequence of two arguments**</span></span>

<span data-ttu-id="51f97-241">Değer null olamaz ve boşlukla ayrılmış anahtar izlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="51f97-241">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="51f97-242">Anahtar bir önekine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="51f97-242">The key must have a prefix.</span></span>

| <span data-ttu-id="51f97-243">Anahtar öneki</span><span class="sxs-lookup"><span data-stu-id="51f97-243">Key prefix</span></span>               | <span data-ttu-id="51f97-244">Örnek</span><span class="sxs-lookup"><span data-stu-id="51f97-244">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="51f97-245">Tek bir tire (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="51f97-245">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="51f97-246">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="51f97-246">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="51f97-247">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="51f97-247">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="51f97-248">&#8224;Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="51f97-248">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="51f97-249">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="51f97-249">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="51f97-250">Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="51f97-250">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="51f97-251">Yinelenen anahtarlar</span><span class="sxs-lookup"><span data-stu-id="51f97-251">Duplicate keys</span></span>

<span data-ttu-id="51f97-252">Yinelenen anahtarlarla verdiyse, son anahtar-değer çifti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51f97-252">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="51f97-253">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="51f97-253">Switch mappings</span></span>

<span data-ttu-id="51f97-254">El ile yapılandırma ile oluştururken `ConfigurationBuilder`, bir anahtar eşlemeleri sözlüğü eklenebilir `AddCommandLine` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="51f97-254">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="51f97-255">Anahtar eşlemeleri anahtar adı değiştirme mantığı izin verir.</span><span class="sxs-lookup"><span data-stu-id="51f97-255">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="51f97-256">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük komut satırı bağımsız değişkeni tarafından sağlanan anahtarıyla eşleşen bir anahtarı için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="51f97-256">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="51f97-257">Komut satırı anahtarı sözlük içinde bulunursa, sözlük değeri (Anahtar değişimini) yapılandırma döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="51f97-257">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="51f97-258">Tek bir çizgiyle önekli herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="51f97-258">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="51f97-259">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="51f97-259">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="51f97-260">Anahtarları kısa çizgiyle başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="51f97-260">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="51f97-261">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="51f97-261">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="51f97-262">Aşağıdaki örnekte, `GetSwitchMappings` yöntemi sağlayan tek bir çizgi kullanmak komut satırı bağımsız değişkenleri (`-`) anahtarı öneki ve önde gelen alt anahtar önekleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="51f97-262">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="51f97-263">Komut satırı bağımsız değişkenleri sağlamadan sözlüğü için sağlanan `AddInMemoryCollection` yapılandırma değerlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="51f97-263">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="51f97-264">Uygulama ile aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="51f97-264">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="51f97-265">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="51f97-265">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="51f97-266">Yapılandırma ayarlarını geçirmek için aşağıdakileri kullanın:</span><span class="sxs-lookup"><span data-stu-id="51f97-266">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="51f97-267">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="51f97-267">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="51f97-268">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra aşağıdaki tabloda gösterilen veriler içerir:</span><span class="sxs-lookup"><span data-stu-id="51f97-268">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="51f97-269">Anahtar</span><span class="sxs-lookup"><span data-stu-id="51f97-269">Key</span></span>            | <span data-ttu-id="51f97-270">Değer</span><span class="sxs-lookup"><span data-stu-id="51f97-270">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="51f97-271">Anahtar geçişi sözlüğünü kullanarak göstermek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="51f97-271">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="51f97-272">Komut satırı anahtarları değiştirilen.</span><span class="sxs-lookup"><span data-stu-id="51f97-272">The command-line keys are swapped.</span></span> <span data-ttu-id="51f97-273">Konsol penceresinde yapılandırma değerlerini görüntüler `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="51f97-273">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="51f97-274">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="51f97-274">web.config file</span></span>

<span data-ttu-id="51f97-275">A *web.config* dosya, IIS veya IIS Express uygulamasında barındırdığında gereklidir.</span><span class="sxs-lookup"><span data-stu-id="51f97-275">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="51f97-276">Ayarlarında *web.config* etkinleştirmek [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) uygulamayı başlatın ve diğer IIS ayarlarını ve modülleri yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="51f97-276">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="51f97-277">Varsa *web.config* dosyası mevcut değil ve proje dosyasını içeren `<Project Sdk="Microsoft.NET.Sdk.Web">`, projeyi yayımlama oluşturur bir *web.config* yayımlanan çıktı dosyasında ( *Yayımlama* klasörü).</span><span class="sxs-lookup"><span data-stu-id="51f97-277">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="51f97-278">Daha fazla bilgi için bkz: [konak ASP.NET Core IIS ile Windows](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="51f97-278">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="51f97-279">Başlatma sırasında yapılandırma erişme</span><span class="sxs-lookup"><span data-stu-id="51f97-279">Accessing configuration during startup</span></span>

<span data-ttu-id="51f97-280">Erişim Yapılandırması içinde `ConfigureServices` veya `Configure` başlatma sırasında örneklere bakın [uygulama başlangıç](xref:fundamentals/startup) konu.</span><span class="sxs-lookup"><span data-stu-id="51f97-280">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="51f97-281">Ek Notlar</span><span class="sxs-lookup"><span data-stu-id="51f97-281">Additional notes</span></span>

* <span data-ttu-id="51f97-282">Bağımlılık ekleme (dı) değil ayarlamak kadar yukarı sonra `ConfigureServices` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="51f97-282">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="51f97-283">Yapılandırma sistemi dı farkında değildir.</span><span class="sxs-lookup"><span data-stu-id="51f97-283">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="51f97-284">`IConfiguration` iki özelleştirmeleri sahiptir:</span><span class="sxs-lookup"><span data-stu-id="51f97-284">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="51f97-285">`IConfigurationRoot` Kök düğüm için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51f97-285">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="51f97-286">Bir yeniden yükleme tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="51f97-286">Can trigger a reload.</span></span>
  * <span data-ttu-id="51f97-287">`IConfigurationSection` Yapılandırma değerlerini bir bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="51f97-287">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="51f97-288">`GetSection` Ve `GetChildren` yöntemleri döndürür bir `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="51f97-288">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="51f97-289">Kullanım [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) yapılandırma yeniden yükleme veya erişim her sağlayıcı için.</span><span class="sxs-lookup"><span data-stu-id="51f97-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="51f97-290">Bunlardan hiçbirine yaygındır.</span><span class="sxs-lookup"><span data-stu-id="51f97-290">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51f97-291">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="51f97-291">Additional resources</span></span>

* [<span data-ttu-id="51f97-292">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51f97-292">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="51f97-293">Birden çok ortamı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="51f97-293">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="51f97-294">Geliştirme sırasında uygulama gizli anahtarlarının güvenli bir şekilde depolanması</span><span class="sxs-lookup"><span data-stu-id="51f97-294">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="51f97-295">ASP.NET çekirdek barındırma</span><span class="sxs-lookup"><span data-stu-id="51f97-295">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="51f97-296">Bağımlılık Ekleme</span><span class="sxs-lookup"><span data-stu-id="51f97-296">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="51f97-297">Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="51f97-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
