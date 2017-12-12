---
title: "ASP.NET çekirdek yapılandırması"
author: rick-anderson
description: "Yapılandırma API'si tarafından birden çok yöntem ASP.NET Core uygulamayı yapılandırmak için kullanın."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="3180f-103">Bir ASP.NET Core uygulamayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3180f-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="3180f-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [işareti Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3180f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3180f-105">Yapılandırma API'si ad-değer çiftlerinin listesini temel web uygulamasını ASP.NET Core yapılandırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="3180f-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="3180f-106">Yapılandırma, birden fazla kaynaktan çalışma zamanında okuyun.</span><span class="sxs-lookup"><span data-stu-id="3180f-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="3180f-107">Bu ad-değer çiftleri çok düzeyli bir hiyerarşi gruplandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3180f-107">You can group these name-value pairs into a multi-level hierarchy.</span></span> 

<span data-ttu-id="3180f-108">İçin yapılandırma sağlayıcısı vardır:</span><span class="sxs-lookup"><span data-stu-id="3180f-108">There are configuration providers for:</span></span>

* <span data-ttu-id="3180f-109">Dosya biçimleri (INI, JSON ve XML)</span><span class="sxs-lookup"><span data-stu-id="3180f-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="3180f-110">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="3180f-110">Command-line arguments</span></span>
* <span data-ttu-id="3180f-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="3180f-111">Environment variables</span></span>
* <span data-ttu-id="3180f-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="3180f-112">In-memory .NET objects</span></span>
* <span data-ttu-id="3180f-113">Bir şifrelenmiş kullanıcı deposu</span><span class="sxs-lookup"><span data-stu-id="3180f-113">An encrypted user store</span></span>
* [<span data-ttu-id="3180f-114">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="3180f-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="3180f-115">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="3180f-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="3180f-116">Her yapılandırma değeri bir dize anahtarı eşler.</span><span class="sxs-lookup"><span data-stu-id="3180f-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="3180f-117">Özel bir ayarları seri durumdan çıkarılacak yerleşik bağlama Destek [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesne (basit bir .NET sınıf özelliklerine sahip).</span><span class="sxs-lookup"><span data-stu-id="3180f-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="3180f-118">Seçenekler düzeni seçenekleri sınıfları ilgili ayar gruplarını göstermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="3180f-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="3180f-119">Seçenekler kullanılması daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="3180f-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="3180f-120">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3180f-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="3180f-121">JSON yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3180f-121">JSON configuration</span></span>

<span data-ttu-id="3180f-122">Aşağıdaki konsol uygulaması JSON yapılandırma sağlayıcısı kullanır:</span><span class="sxs-lookup"><span data-stu-id="3180f-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="3180f-123">Uygulama okur ve aşağıdaki yapılandırma ayarları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="3180f-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="3180f-124">Hiyerarşik ad-değer çiftleri düğümleri bir virgülle ayrılmış listesini yapılandırması oluşur.</span><span class="sxs-lookup"><span data-stu-id="3180f-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="3180f-125">Bir değer almak için erişim `Configuration` karşılık gelen öğenin anahtarı ile dizinleyici:</span><span class="sxs-lookup"><span data-stu-id="3180f-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="3180f-126">JSON biçimli yapılandırma kaynaklarını dizilerde çalışmak için iki nokta üst üste ayrılmış dize bir parçası olarak bir dizi dizini kullanın.</span><span class="sxs-lookup"><span data-stu-id="3180f-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="3180f-127">Aşağıdaki örnek önceki ilk öğe adını alır `wizards` dizi:</span><span class="sxs-lookup"><span data-stu-id="3180f-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="3180f-128">Ad-değer çiftleri için yerleşik yazılmış `Configuration` sağlayıcılarıdır **değil** kalıcı.</span><span class="sxs-lookup"><span data-stu-id="3180f-128">Name-value pairs written to the built-in `Configuration` providers are **not** persisted.</span></span> <span data-ttu-id="3180f-129">Ancak, değerleri kaydeder özel bir sağlayıcı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3180f-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="3180f-130">Bkz: [özel yapılandırma sağlayıcısının](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="3180f-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="3180f-131">Yukarıdaki örnek yapılandırma dizin oluşturucu değerleri okumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="3180f-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="3180f-132">Erişim yapılandırması dışında `Startup`, kullanın *seçenekleri düzeni*.</span><span class="sxs-lookup"><span data-stu-id="3180f-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="3180f-133">Daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="3180f-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="3180f-134">Örneğin, geliştirme, test ve üretim farklı ortamlar için farklı yapılandırma ayarlarını sağlamak için genel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3180f-134">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="3180f-135">`CreateDefaultBuilder` Bir ASP.NET Core 2.x uygulamasında genişletme yöntemi (veya kullanarak `AddJsonFile` ve `AddEnvironmentVariables` bir ASP.NET Core 1.x uygulamasındaki doğrudan) JSON dosyaları ve sistem yapılandırma kaynaklarını okumak için bir yapılandırma sağlayıcısı ekler:</span><span class="sxs-lookup"><span data-stu-id="3180f-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="3180f-136">*appSettings.JSON*</span><span class="sxs-lookup"><span data-stu-id="3180f-136">*appsettings.json*</span></span>
* <span data-ttu-id="3180f-137">*appSettings. \<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="3180f-137">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="3180f-138">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="3180f-138">Environment variables</span></span>

<span data-ttu-id="3180f-139">Bkz: [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) için bir açıklama parametreleri.</span><span class="sxs-lookup"><span data-stu-id="3180f-139">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="3180f-140">`reloadOnChange`yalnızca ASP.NET Core 1.1 ve sonraki sürümlerinde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3180f-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span> 

<span data-ttu-id="3180f-141">Yapılandırma kaynaklarını belirtilen sırada salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="3180f-141">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="3180f-142">Yukarıdaki kod, ortam değişkenleri son salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="3180f-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="3180f-143">Herhangi bir yapılandırma değeri ortamı Değiştir iki önceki sağlayıcıları belirlenen ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3180f-143">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="3180f-144">Ortam genellikle ayarlamak `Development`, `Staging`, veya `Production`.</span><span class="sxs-lookup"><span data-stu-id="3180f-144">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="3180f-145">Bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3180f-145">See [Working with multiple environments](xref:fundamentals/environments) for more information.</span></span>

<span data-ttu-id="3180f-146">Yapılandırma dikkate alınacak noktalar:</span><span class="sxs-lookup"><span data-stu-id="3180f-146">Configuration considerations:</span></span>

* <span data-ttu-id="3180f-147">`IOptionsSnapshot`değişiklik yapıldığında yapılandırma verileri yeniden yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3180f-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="3180f-148">Bkz: [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3180f-148">See [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="3180f-149">Yapılandırma anahtarları büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="3180f-149">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="3180f-150">Yerel ortamdaki dağıtılan yapılandırma dosyalarında ayarlarını geçersiz kılabilir için ortam değişkenleri son belirtin.</span><span class="sxs-lookup"><span data-stu-id="3180f-150">Specify environment variables last so that the local environment can override settings in deployed configuration files.</span></span>
* <span data-ttu-id="3180f-151">**Hiçbir zaman** parolalar ve diğer hassas verileri yapılandırma sağlayıcısı kodu veya düz metin yapılandırma dosyalarını depolar.</span><span class="sxs-lookup"><span data-stu-id="3180f-151">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="3180f-152">Verme geliştirmede üretim gizlilikleri kullanın veya sınama ortamlarında.</span><span class="sxs-lookup"><span data-stu-id="3180f-152">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="3180f-153">Bunun yerine, böylece bunlar deponuza yanlışlıkla uygulanamıyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="3180f-153">Instead, specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="3180f-154">Daha fazla bilgi edinmek [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) ve yönetme [geliştirme sırasında uygulama sırrı güvenli depolama](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3180f-154">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="3180f-155">İki nokta varsa (`:`) olamaz, sistem ortam değişkenlerini kullanıldığında, iki nokta üst üste değiştirin (`:`) bir çift alt çizgi (`__`).</span><span class="sxs-lookup"><span data-stu-id="3180f-155">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="3180f-156">Bellek içi sağlayıcısı ve bağlama için bir POCO sınıfı</span><span class="sxs-lookup"><span data-stu-id="3180f-156">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="3180f-157">Aşağıdaki örnek, bellek içi Sağlayıcısı'nı kullanın ve bir sınıfa bağlamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="3180f-157">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="3180f-158">Yapılandırma değerleri dize olarak döndürülür, ancak bağlama nesnelerin yapımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3180f-158">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="3180f-159">Bağlama POCO nesneleri veya hatta tüm nesne grafiklerinin almanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3180f-159">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="3180f-160">GetValue</span><span class="sxs-lookup"><span data-stu-id="3180f-160">GetValue</span></span>

<span data-ttu-id="3180f-161">Aşağıdaki örnek gösterilmektedir [GetValue&lt;T&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3180f-161">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="3180f-162">ConfigurationBinder's `GetValue<T>` yöntemi, varsayılan değer (80 örnekteki) belirtmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="3180f-162">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="3180f-163">`GetValue<T>`Basit senaryolar için ve tüm bölümleri bağlamaz.</span><span class="sxs-lookup"><span data-stu-id="3180f-163">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="3180f-164">`GetValue<T>`skaler değerleri alır `GetSection(key).Value` belirli bir türüne dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="3180f-164">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="3180f-165">Bir nesne grafiğinin bağlama</span><span class="sxs-lookup"><span data-stu-id="3180f-165">Bind to an object graph</span></span>

<span data-ttu-id="3180f-166">Bir sınıftaki her nesneyi yinelemeli olarak bağlama kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3180f-166">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="3180f-167">Aşağıdakileri göz önünde bulundurun `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3180f-167">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="3180f-168">Aşağıdaki örnek bağlar `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3180f-168">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="3180f-169">**ASP.NET Core 1.1** ve daha yüksek kullanabilirsiniz `Get<T>`, tüm bölümleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="3180f-169">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="3180f-170">`Get<T>`kullanmaktan daha kullanışlı olabilir `Bind`.</span><span class="sxs-lookup"><span data-stu-id="3180f-170">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="3180f-171">Aşağıdaki kodu nasıl kullanılacağını gösterir `Get<T>` yukarıdaki örnekle:</span><span class="sxs-lookup"><span data-stu-id="3180f-171">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="3180f-172">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3180f-172">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="3180f-173">Program görüntüler `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="3180f-173">The program displays `Height 11`.</span></span>

<span data-ttu-id="3180f-174">Aşağıdaki kod birimine kullanılabilir test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="3180f-174">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="3180f-175">Entity Framework özel sağlayıcı oluşturma</span><span class="sxs-lookup"><span data-stu-id="3180f-175">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="3180f-176">Bu bölümde, ad-değer çiftleri EF kullanan bir veritabanından okur bir temel yapılandırma sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3180f-176">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="3180f-177">Tanımlayan bir `ConfigurationValue` yapılandırma değerlerini veritabanında depolamak için varlık:</span><span class="sxs-lookup"><span data-stu-id="3180f-177">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="3180f-178">Ekleme bir `ConfigurationContext` depolamak ve yapılandırılmış değerlerine erişmek için:</span><span class="sxs-lookup"><span data-stu-id="3180f-178">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="3180f-179">Arabirimini uygulayan bir sınıf oluşturun [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="3180f-179">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="3180f-180">Özel yapılandırma sağlayıcısının devralarak oluşturma [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="3180f-180">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="3180f-181">Boş olduğunda yapılandırma sağlayıcısı veritabanı başlatır:</span><span class="sxs-lookup"><span data-stu-id="3180f-181">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="3180f-182">Örneği çalıştırdığınızda ("value_from_ef_1" ve "value_from_ef_2") veritabanından vurgulanan değerler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3180f-182">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="3180f-183">Ekleyebileceğiniz bir `EFConfigSource` genişletme yöntemi yapılandırma kaynağı eklemek için:</span><span class="sxs-lookup"><span data-stu-id="3180f-183">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="3180f-184">Aşağıdaki kod özel kullanmayı gösterir `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="3180f-184">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="3180f-185">Örnek ekler özel Not `EFConfigProvider` JSON sağlayıcısı sonra bu nedenle veritabanından herhangi bir ayarı geçersiz kılar ayarlarından *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="3180f-185">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="3180f-186">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3180f-186">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="3180f-187">Aşağıdaki görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="3180f-187">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="3180f-188">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="3180f-188">CommandLine configuration provider</span></span>

<span data-ttu-id="3180f-189">[CommandLine yapılandırma sağlayıcısı](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) çalışma zamanında yapılandırması için komut satırı bağımsız değişkeni anahtar-değer çiftleri alır.</span><span class="sxs-lookup"><span data-stu-id="3180f-189">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="3180f-190">Görüntülemek veya komut satırı yapılandırma örneği indirin</span><span class="sxs-lookup"><span data-stu-id="3180f-190">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="3180f-191">Kurulum ve komut satırı yapılandırma sağlayıcısı kullanın</span><span class="sxs-lookup"><span data-stu-id="3180f-191">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="3180f-192">Temel yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3180f-192">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="3180f-193">Komut satırı yapılandırmasını etkinleştirmek için arama `AddCommandLine` genişletme yöntemi örneği üzerinde [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="3180f-193">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="3180f-194">Kod çalıştırmadan, aşağıdaki çıkış görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="3180f-194">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="3180f-195">Komut satırında anahtar-değer çiftleri bağımsız değişken geçirme değerlerini değiştirir `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="3180f-195">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="3180f-196">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="3180f-196">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="3180f-197">Komut satırı yapılandırması ile diğer yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için arama `AddCommandLine` üzerinde son `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3180f-197">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3180f-198">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="3180f-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3180f-199">Tipik ASP.NET Core 2.x uygulamaları kullanma statik kolaylık metodunun `CreateDefaultBuilder` konak oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="3180f-199">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="3180f-200">`CreateDefaultBuilder`İsteğe bağlı yapılandırmasından yükler *appsettings.json*, *appsettings. { Ortam} .json*, [kullanıcı parolaları](xref:security/app-secrets) (içinde `Development` ortamı), ortam değişkenleri ve komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="3180f-200">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="3180f-201">Komut satırı yapılandırma sağlayıcısı son çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3180f-201">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="3180f-202">Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3180f-202">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="3180f-203">İçin unutmayın *appsettings* dosyaları `reloadOnChange` etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3180f-203">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="3180f-204">Eşleşen bir yapılandırma değeri, komut satırı bağımsız değişkenleri geçersiz kılınmış bir *appsettings* dosya uygulama başladıktan sonra değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="3180f-204">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="3180f-205">Kullanmaya alternatif olarak `CreateDefaultBuilder` kullanarak bir ana bilgisayar oluşturma yöntemi, [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ve el ile yapılandırma oluşturma [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ASP.NET Core desteklenen 2.x.</span><span class="sxs-lookup"><span data-stu-id="3180f-205">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="3180f-206">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="3180f-206">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3180f-207">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="3180f-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3180f-208">Oluşturma bir [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ve arama `AddCommandLine` yöntemi CommandLine yapılandırma sağlayıcısı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3180f-208">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="3180f-209">Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3180f-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="3180f-210">Yapılandırmasını uygulamak [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3180f-210">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="3180f-211">Arguments</span><span class="sxs-lookup"><span data-stu-id="3180f-211">Arguments</span></span>

<span data-ttu-id="3180f-212">Komut satırına geçirilen bağımsız değişkenler aşağıdaki tabloda gösterilen iki biçim birine uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="3180f-212">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="3180f-213">Bağımsız değişken biçimi</span><span class="sxs-lookup"><span data-stu-id="3180f-213">Argument format</span></span>                                                     | <span data-ttu-id="3180f-214">Örnek</span><span class="sxs-lookup"><span data-stu-id="3180f-214">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="3180f-215">Tek bağımsız değişken: bir anahtar-değer çifti ayrılmış bir eşittir işareti (`=`)</span><span class="sxs-lookup"><span data-stu-id="3180f-215">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="3180f-216">İki bağımsız değişken bir dizi: boşlukla ayrılmış bir anahtar-değer çifti</span><span class="sxs-lookup"><span data-stu-id="3180f-216">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="3180f-217">**Tek bir bağımsız değişken**</span><span class="sxs-lookup"><span data-stu-id="3180f-217">**Single argument**</span></span>

<span data-ttu-id="3180f-218">Değer bir eşittir işareti gelmelidir (`=`).</span><span class="sxs-lookup"><span data-stu-id="3180f-218">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="3180f-219">Değer null olabilir (örneğin, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="3180f-219">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="3180f-220">Anahtar bir önek olabilir.</span><span class="sxs-lookup"><span data-stu-id="3180f-220">The key may have a prefix.</span></span>

| <span data-ttu-id="3180f-221">Anahtar öneki</span><span class="sxs-lookup"><span data-stu-id="3180f-221">Key prefix</span></span>               | <span data-ttu-id="3180f-222">Örnek</span><span class="sxs-lookup"><span data-stu-id="3180f-222">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="3180f-223">Önek</span><span class="sxs-lookup"><span data-stu-id="3180f-223">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="3180f-224">Tek bir tire (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="3180f-224">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="3180f-225">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="3180f-225">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="3180f-226">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="3180f-226">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="3180f-227">&#8224; Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3180f-227">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="3180f-228">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="3180f-228">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="3180f-229">Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3180f-229">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="3180f-230">**İki bağımsız değişken dizisi**</span><span class="sxs-lookup"><span data-stu-id="3180f-230">**Sequence of two arguments**</span></span>

<span data-ttu-id="3180f-231">Değer null olamaz ve boşlukla ayrılmış anahtar izlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3180f-231">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="3180f-232">Anahtar bir önekine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3180f-232">The key must have a prefix.</span></span>

| <span data-ttu-id="3180f-233">Anahtar öneki</span><span class="sxs-lookup"><span data-stu-id="3180f-233">Key prefix</span></span>               | <span data-ttu-id="3180f-234">Örnek</span><span class="sxs-lookup"><span data-stu-id="3180f-234">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="3180f-235">Tek bir tire (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="3180f-235">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="3180f-236">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="3180f-236">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="3180f-237">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="3180f-237">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="3180f-238">&#8224; Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3180f-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="3180f-239">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="3180f-239">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="3180f-240">Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3180f-240">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="3180f-241">Yinelenen anahtarlar</span><span class="sxs-lookup"><span data-stu-id="3180f-241">Duplicate keys</span></span>

<span data-ttu-id="3180f-242">Yinelenen anahtarlarla verdiyse, son anahtar-değer çifti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3180f-242">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="3180f-243">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="3180f-243">Switch mappings</span></span>

<span data-ttu-id="3180f-244">El ile yapılandırma ile oluştururken `ConfigurationBuilder`, isteğe bağlı olarak bir anahtar eşlemeleri sözlüğe sağlayabilir `AddCommandLine` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3180f-244">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="3180f-245">Anahtar eşlemeleri anahtar adı değiştirme mantığın olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="3180f-245">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="3180f-246">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük komut satırı bağımsız değişkeni tarafından sağlanan anahtarıyla eşleşen bir anahtarı için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="3180f-246">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="3180f-247">Komut satırı anahtarı sözlük içinde bulunursa, sözlük değeri (Anahtar değişimini) yapılandırma döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3180f-247">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="3180f-248">Tek bir çizgiyle önekli herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="3180f-248">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="3180f-249">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="3180f-249">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="3180f-250">Anahtarları kısa çizgiyle başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="3180f-250">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="3180f-251">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="3180f-251">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="3180f-252">Aşağıdaki örnekte, `GetSwitchMappings` yöntemi sağlayan tek bir çizgi kullanmak, komut satırı bağımsız değişkenleri (`-`) anahtarı öneki ve önde gelen alt anahtar önekleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="3180f-252">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="3180f-253">Komut satırı bağımsız değişkenleri sağlamadan sözlüğü için sağlanan `AddInMemoryCollection` yapılandırma değerlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3180f-253">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="3180f-254">Uygulama ile aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3180f-254">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="3180f-255">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="3180f-255">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="3180f-256">Yapılandırma ayarlarını geçirmek için aşağıdakileri kullanın:</span><span class="sxs-lookup"><span data-stu-id="3180f-256">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="3180f-257">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="3180f-257">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="3180f-258">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra aşağıdaki tabloda gösterilen veriler içerir.</span><span class="sxs-lookup"><span data-stu-id="3180f-258">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="3180f-259">Anahtar</span><span class="sxs-lookup"><span data-stu-id="3180f-259">Key</span></span>            | <span data-ttu-id="3180f-260">Değer</span><span class="sxs-lookup"><span data-stu-id="3180f-260">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="3180f-261">Anahtar geçişi sözlüğünü kullanarak göstermek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3180f-261">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="3180f-262">Komut satırı anahtarları değiştirilen.</span><span class="sxs-lookup"><span data-stu-id="3180f-262">The command-line keys are swapped.</span></span> <span data-ttu-id="3180f-263">Konsol penceresinde yapılandırma değerlerini görüntüler `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="3180f-263">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="3180f-264">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="3180f-264">The web.config file</span></span>

<span data-ttu-id="3180f-265">A *web.config* dosya, IIS veya IIS Express uygulamasında barındırdığınızda gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3180f-265">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="3180f-266">*Web.config* uygulamanızı başlatma IIS'de AspNetCoreModule açar.</span><span class="sxs-lookup"><span data-stu-id="3180f-266">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="3180f-267">Ayarlarında *web.config* uygulamanızı başlatın ve diğer IIS ayarlarını ve modülleri yapılandırmak için IIS'de AspNetCoreModule etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="3180f-267">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="3180f-268">Visual Studio kullanarak ve silerseniz *web.config*, Visual Studio yeni bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3180f-268">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="3180f-269">Ek Notlar</span><span class="sxs-lookup"><span data-stu-id="3180f-269">Additional notes</span></span>

* <span data-ttu-id="3180f-270">Bağımlılık ekleme (dı) ayarlı değil kadar yukarı sonra `ConfigureServices` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3180f-270">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="3180f-271">Yapılandırma sistemi dı uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="3180f-271">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="3180f-272">`IConfiguration`iki özelleştirmeleri sahiptir:</span><span class="sxs-lookup"><span data-stu-id="3180f-272">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="3180f-273">`IConfigurationRoot`Kök düğüm için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3180f-273">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="3180f-274">Bir yeniden yükleme tetikleyebilir.</span><span class="sxs-lookup"><span data-stu-id="3180f-274">Can trigger a reload.</span></span>
  * <span data-ttu-id="3180f-275">`IConfigurationSection`Yapılandırma değerlerini bir bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3180f-275">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="3180f-276">`GetSection` Ve `GetChildren` yöntemleri döndürür bir `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="3180f-276">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3180f-277">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3180f-277">Additional resources</span></span>

* [<span data-ttu-id="3180f-278">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="3180f-278">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="3180f-279">Birden çok ortamı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="3180f-279">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="3180f-280">Geliştirme sırasında uygulama sırrı güvenli depolama</span><span class="sxs-lookup"><span data-stu-id="3180f-280">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="3180f-281">ASP.NET çekirdek barındırma</span><span class="sxs-lookup"><span data-stu-id="3180f-281">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="3180f-282">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="3180f-282">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="3180f-283">Azure anahtar kasası yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="3180f-283">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
