---
title: ASP.NET core'da yapılandırma
author: rick-anderson
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228630"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="0efe7-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0efe7-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="0efe7-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [işareti Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0efe7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0efe7-105">Yapılandırma API ad-değer çiftlerinin listesini temel web uygulaması bir ASP.NET Core yapılandırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="0efe7-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="0efe7-106">Yapılandırma, çalışma zamanında birden çok kaynaktan okuyun.</span><span class="sxs-lookup"><span data-stu-id="0efe7-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="0efe7-107">Ad-değer çiftleri çok düzeyli bir hiyerarşi halinde gruplandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="0efe7-108">İçin yapılandırma sağlayıcısı vardır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-108">There are configuration providers for:</span></span>

* <span data-ttu-id="0efe7-109">Dosya biçimleri (INI, JSON ve XML).</span><span class="sxs-lookup"><span data-stu-id="0efe7-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="0efe7-110">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="0efe7-110">Command-line arguments.</span></span>
* <span data-ttu-id="0efe7-111">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="0efe7-111">Environment variables.</span></span>
* <span data-ttu-id="0efe7-112">Bellek içi .NET nesneleri.</span><span class="sxs-lookup"><span data-stu-id="0efe7-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="0efe7-113">Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolama.</span><span class="sxs-lookup"><span data-stu-id="0efe7-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="0efe7-114">Gibi bir şifrelenmiş kullanıcı depolamak [Azure anahtar kasası](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="0efe7-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="0efe7-115">Özel sağlayıcılar (veya oluşturulan yüklü).</span><span class="sxs-lookup"><span data-stu-id="0efe7-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="0efe7-116">Her yapılandırma değeri bir dize anahtarına eşler.</span><span class="sxs-lookup"><span data-stu-id="0efe7-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="0efe7-117">Ayarlar uygulamasına özel bir'seri durumdan çıkarmak için yerleşik bağlama desteği yoktur [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesne (basit bir .NET sınıf özelliklere sahip).</span><span class="sxs-lookup"><span data-stu-id="0efe7-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="0efe7-118">Seçenekleri deseni seçenekleri sınıfları, ilgili ayar gruplarını temsil etmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="0efe7-119">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="0efe7-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="0efe7-120">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0efe7-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0efe7-121">Bu konuda sağlanan örnekleri üzerine kullanır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-121">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="0efe7-122">Temel yol ile uygulama ayarı [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="0efe7-122">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="0efe7-123">`SetBasePath` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-123">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="0efe7-124">Yapılandırma dosyalarıyla bölümlerini çözümleme [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="0efe7-124">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="0efe7-125">`GetSection` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-125">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="0efe7-126">Bağlama yapılandırması ile [bağlama](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="0efe7-126">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="0efe7-127">`Bind` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-127">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="0efe7-128">Bu paketleri dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0efe7-128">These packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0efe7-129">Bu konuda sağlanan örnekleri üzerine kullanır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-129">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="0efe7-130">Temel yol ile uygulama ayarı [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="0efe7-130">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="0efe7-131">`SetBasePath` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-131">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="0efe7-132">Yapılandırma dosyalarıyla bölümlerini çözümleme [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="0efe7-132">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="0efe7-133">`GetSection` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-133">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="0efe7-134">Bağlama yapılandırması ile [bağlama](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="0efe7-134">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="0efe7-135">`Bind` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-135">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="0efe7-136">Bu paketleri dahil [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="0efe7-136">These packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="0efe7-137">Bu konuda sağlanan örnekleri üzerine kullanır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-137">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="0efe7-138">Temel yol ile uygulama ayarı [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span><span class="sxs-lookup"><span data-stu-id="0efe7-138">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="0efe7-139">`SetBasePath` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-139">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="0efe7-140">Yapılandırma dosyalarıyla bölümlerini çözümleme [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span><span class="sxs-lookup"><span data-stu-id="0efe7-140">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="0efe7-141">`GetSection` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-141">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="0efe7-142">Bağlama yapılandırması ile [bağlama](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span><span class="sxs-lookup"><span data-stu-id="0efe7-142">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="0efe7-143">`Bind` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.</span><span class="sxs-lookup"><span data-stu-id="0efe7-143">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

::: moniker-end

## <a name="json-configuration"></a><span data-ttu-id="0efe7-144">JSON yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0efe7-144">JSON configuration</span></span>

<span data-ttu-id="0efe7-145">Aşağıdaki konsol uygulaması, JSON yapılandırma sağlayıcısını kullanır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-145">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="0efe7-146">Uygulama okur ve aşağıdaki yapılandırma ayarları görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0efe7-146">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="0efe7-147">Yapılandırma, hiyerarşik düğümleri virgül ile ayrılır ad-değer çiftlerinin listesini oluşur (`:`).</span><span class="sxs-lookup"><span data-stu-id="0efe7-147">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="0efe7-148">Bir değer almak için erişim `Configuration` Dizin Oluşturucu ile ilgili öğenin anahtarı:</span><span class="sxs-lookup"><span data-stu-id="0efe7-148">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="0efe7-149">JSON biçimli yapılandırma kaynaklarını dizilerde çalışmak için iki nokta ile ayrılmış dizesinin parçası olarak bir dizi dizini kullanın.</span><span class="sxs-lookup"><span data-stu-id="0efe7-149">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="0efe7-150">Aşağıdaki örnek, önceki içindeki ilk öğeyi adını alır. `wizards` dizisi:</span><span class="sxs-lookup"><span data-stu-id="0efe7-150">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="0efe7-151">Ad-değer çiftleri yerleşik olarak yazılmış [yapılandırma](/dotnet/api/microsoft.extensions.configuration) sağlayıcılar **değil** kalıcı.</span><span class="sxs-lookup"><span data-stu-id="0efe7-151">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="0efe7-152">Ancak, değerler kaydeder özel bir sağlayıcı oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-152">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="0efe7-153">Bkz: [özel yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="0efe7-153">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="0efe7-154">Yukarıdaki örnek yapılandırma dizin oluşturucu değerleri okumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-154">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="0efe7-155">Erişimi yapılandırma dışında `Startup`, kullanın *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="0efe7-155">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="0efe7-156">Daha fazla bilgi için [seçenekleri](xref:fundamentals/configuration/options) konu.</span><span class="sxs-lookup"><span data-stu-id="0efe7-156">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="0efe7-157">XML yapılandırması</span><span class="sxs-lookup"><span data-stu-id="0efe7-157">XML configuration</span></span>

<span data-ttu-id="0efe7-158">XML biçimli yapılandırma kaynaklarını dizilerde çalışmak için sağlayan bir `name` her öğenin dizini.</span><span class="sxs-lookup"><span data-stu-id="0efe7-158">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="0efe7-159">Değerlere erişmek için dizini kullanın:</span><span class="sxs-lookup"><span data-stu-id="0efe7-159">Use the index to access the values:</span></span>

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

## <a name="configuration-by-environment"></a><span data-ttu-id="0efe7-160">Ortama göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0efe7-160">Configuration by environment</span></span>

<span data-ttu-id="0efe7-161">Örneğin, geliştirme, test ve üretim gibi farklı ortamlar için farklı yapılandırma ayarları için tipik bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="0efe7-161">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="0efe7-162">`CreateDefaultBuilder` Bir ASP.NET Core 2.x uygulamasında genişletme yöntemi (veya bu adı kullanıyor `AddJsonFile` ve `AddEnvironmentVariables` bir ASP.NET Core 1.x uygulamada doğrudan) JSON dosyalarını ve sistem yapılandırma kaynaklarını okumak için bir yapılandırma sağlayıcısı ekler:</span><span class="sxs-lookup"><span data-stu-id="0efe7-162">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="0efe7-163">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="0efe7-163">*appsettings.json*</span></span>
* <span data-ttu-id="0efe7-164">*appSettings. \<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="0efe7-164">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="0efe7-165">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="0efe7-165">Environment variables</span></span>

<span data-ttu-id="0efe7-166">Çağırmak için gereken ASP.NET Core 1.x uygulamaları `AddJsonFile` ve [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="0efe7-166">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="0efe7-167">Bkz: [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) parametre açıklaması.</span><span class="sxs-lookup"><span data-stu-id="0efe7-167">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="0efe7-168">`reloadOnChange` yalnızca ASP.NET Core 1.1 ve sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-168">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="0efe7-169">Yapılandırma kaynaklarını belirtilen sırada okunur.</span><span class="sxs-lookup"><span data-stu-id="0efe7-169">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="0efe7-170">Önceki kodda, ortam değişkenleri son okunur.</span><span class="sxs-lookup"><span data-stu-id="0efe7-170">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="0efe7-171">Tüm yapılandırma değerleri ile ortam değiştirme iki önceki sağlayıcıları ayarlananlara ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="0efe7-171">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="0efe7-172">Aşağıdakileri göz önünde bulundurun *appsettings. Staging.JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0efe7-172">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="0efe7-173">Aşağıdaki kodda, `Configure` değerini okur `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="0efe7-173">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="0efe7-174">Ortamı genellikle kümesine `Development`, `Staging`, veya `Production`.</span><span class="sxs-lookup"><span data-stu-id="0efe7-174">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="0efe7-175">Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="0efe7-175">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="0efe7-176">Yapılandırmada dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="0efe7-176">Configuration considerations:</span></span>

* <span data-ttu-id="0efe7-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) yapılandırma verileri değiştiğinde yeniden yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0efe7-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="0efe7-178">Yapılandırma anahtarları **değil** büyük küçük harfe duyarlı.</span><span class="sxs-lookup"><span data-stu-id="0efe7-178">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="0efe7-179">**Hiçbir zaman** yapılandırma sağlayıcısı kod veya yapılandırma dosyalarını düz metin parolalar ve diğer hassas verileri depolayın.</span><span class="sxs-lookup"><span data-stu-id="0efe7-179">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="0efe7-180">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="0efe7-180">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="0efe7-181">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="0efe7-181">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="0efe7-182">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0efe7-182">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="0efe7-183">Hiyerarşik yapılandırma değerleri belirtilen ortam değişkenleri, bir iki nokta üst üste (`:`) tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-183">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="0efe7-184">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-184">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="0efe7-185">Yapılandırma API'si, bir iki nokta üst üste ile etkileşim kurulurken (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-185">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="0efe7-186">Bellek içi sağlayıcısı ve bağlama için bir POCO sınıfı</span><span class="sxs-lookup"><span data-stu-id="0efe7-186">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="0efe7-187">Aşağıdaki örnek, bellek içi sağlayıcısı kullanın ve bir sınıfa Bağla gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0efe7-187">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="0efe7-188">Yapılandırma değerleri dize olarak döndürülür, ancak nesnelerin yapımı bağlamayı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-188">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="0efe7-189">Bağlamanın POCO nesneleri veya hatta tüm nesne grafiklerini alınmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="0efe7-189">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="0efe7-190">GetValue</span><span class="sxs-lookup"><span data-stu-id="0efe7-190">GetValue</span></span>

<span data-ttu-id="0efe7-191">Aşağıdaki örnekte [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0efe7-191">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="0efe7-192">ConfigurationBinder'ın `GetValue<T>` yöntemi (örnekte 80) varsayılan değer belirtimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="0efe7-192">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="0efe7-193">`GetValue<T>` Basit senaryolar için ve tüm bölümleri için bağlamayan.</span><span class="sxs-lookup"><span data-stu-id="0efe7-193">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="0efe7-194">`GetValue<T>` skaler değerden alır `GetSection(key).Value` belirli bir türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="0efe7-194">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="0efe7-195">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="0efe7-195">Bind to an object graph</span></span>

<span data-ttu-id="0efe7-196">Her nesne bir sınıftaki yinelemeli olarak bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-196">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="0efe7-197">Aşağıdakileri göz önünde bulundurun `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0efe7-197">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="0efe7-198">Aşağıdaki örnek bağlar `AppSettings` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0efe7-198">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="0efe7-199">**ASP.NET Core 1.1** ve daha yüksek kullanabilirsiniz `Get<T>`, tüm bölümleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-199">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="0efe7-200">`Get<T>` kullanmaktan daha kullanışlı olabilir `Bind`.</span><span class="sxs-lookup"><span data-stu-id="0efe7-200">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="0efe7-201">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle:</span><span class="sxs-lookup"><span data-stu-id="0efe7-201">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="0efe7-202">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0efe7-202">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="0efe7-203">Program görüntüler `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="0efe7-203">The program displays `Height 11`.</span></span>

<span data-ttu-id="0efe7-204">Aşağıdaki kod, birim için kullanılabilir test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="0efe7-204">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="0efe7-205">Entity Framework ile özel bir sağlayıcı oluşturma</span><span class="sxs-lookup"><span data-stu-id="0efe7-205">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="0efe7-206">Bu bölümde, ad-değer çiftlerini okur EF kullanarak bir veritabanından bir temel yapılandırma sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0efe7-206">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="0efe7-207">Tanımlayan bir `ConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık:</span><span class="sxs-lookup"><span data-stu-id="0efe7-207">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="0efe7-208">Ekleme bir `ConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için:</span><span class="sxs-lookup"><span data-stu-id="0efe7-208">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="0efe7-209">Uygulayan bir sınıf oluşturma [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="0efe7-209">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="0efe7-210">Özel yapılandırma sağlayıcısını devralarak oluşturma [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="0efe7-210">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="0efe7-211">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-211">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="0efe7-212">Örneği çalıştırdığınızda bir veritabanından ("value_from_ef_1" ve "value_from_ef_2") vurgulanan değerler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-212">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="0efe7-213">Bir `EFConfigSource` yapılandırma kaynağı eklemek için uzantı yöntemi kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="0efe7-213">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="0efe7-214">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="0efe7-214">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="0efe7-215">Örnek özel ekler Not `EFConfigProvider` JSON sağlayıcısı sonra bu nedenle veritabanından herhangi bir ayarı geçersiz kılar ayarlarından *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="0efe7-215">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="0efe7-216">Aşağıdakileri kullanarak *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0efe7-216">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="0efe7-217">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0efe7-217">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="0efe7-218">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="0efe7-218">CommandLine configuration provider</span></span>

<span data-ttu-id="0efe7-219">[CommandLine yapılandırma sağlayıcısı](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) komut satırı bağımsız değişkeni anahtar-değer çiftleri için çalışma zamanı yapılandırmasını alır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-219">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="0efe7-220">Görüntüleme veya komut satırı yapılandırma örneği indirme</span><span class="sxs-lookup"><span data-stu-id="0efe7-220">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="0efe7-221">Kurulum ve komut satırı yapılandırma sağlayıcısı kullanın</span><span class="sxs-lookup"><span data-stu-id="0efe7-221">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="0efe7-222">Temel yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0efe7-222">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="0efe7-223">Komut satırı yapılandırmasını etkinleştirmek için çağrı `AddCommandLine` örneği genişletme yöntemini [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="0efe7-223">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="0efe7-224">Kod çalıştırmak, aşağıdaki çıktıyı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0efe7-224">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="0efe7-225">Komut satırında anahtar-değer çiftleri bağımsız değişken geçirme değerlerini değiştirir `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="0efe7-225">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="0efe7-226">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0efe7-226">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="0efe7-227">Komut satırı yapılandırmasıyla diğer yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için çağrı `AddCommandLine` son `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0efe7-227">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0efe7-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0efe7-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="0efe7-229">Tipik bir ASP.NET Core 2.x uygulamaları kullanmak statik kolaylık yöntemi `CreateDefaultBuilder` ana bilgisayar oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="0efe7-229">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="0efe7-230">`CreateDefaultBuilder` İsteğe bağlı yapılandırmasından yükler *appsettings.json*, *appsettings. { Ortam} .json*, [kullanıcı parolalarını](xref:security/app-secrets) (içinde `Development` ortamı), ortam değişkenleri ve komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="0efe7-230">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="0efe7-231">Komut satırı yapılandırma sağlayıcısı en son çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-231">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="0efe7-232">Sağlayıcıya son çağrı çalışma zamanında yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenlerini daha önce çağırılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="0efe7-232">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="0efe7-233">İçin *appsettings* nerede dosyaları:</span><span class="sxs-lookup"><span data-stu-id="0efe7-233">For *appsettings* files where:</span></span>

* <span data-ttu-id="0efe7-234">`reloadOnChange` etkin.</span><span class="sxs-lookup"><span data-stu-id="0efe7-234">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="0efe7-235">Komut satırı bağımsız değişkenleri aynı ayarda içerir ve bir *appsettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="0efe7-235">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="0efe7-236">*Appsettings* eşleşen komut satırı bağımsız değişkenini içeren bir dosya uygulama başladıktan sonra değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="0efe7-236">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="0efe7-237">Yukarıdaki koşulların tümü doğruysa, komut satırı bağımsız değişkenleri geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-237">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="0efe7-238">ASP.NET Core 2.x uygulaması kullanabileceğiniz [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) yerine `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0efe7-238">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0efe7-239">Kullanırken `WebHostBuilder`, el ile set yapılandırma [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="0efe7-239">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="0efe7-240">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="0efe7-240">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0efe7-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0efe7-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="0efe7-242">Oluşturma bir [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ve çağrı `AddCommandLine` CommandLine yapılandırma sağlayıcısı kullanmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0efe7-242">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="0efe7-243">Sağlayıcıya son çağrı çalışma zamanında yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenlerini daha önce çağırılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="0efe7-243">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="0efe7-244">Yapılandırmasını uygulamak [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0efe7-244">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="0efe7-245">Arguments</span><span class="sxs-lookup"><span data-stu-id="0efe7-245">Arguments</span></span>

<span data-ttu-id="0efe7-246">Komut satırında geçirilen bağımsız değişkenler, aşağıdaki tabloda gösterilen iki biçim birine uymalıdır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-246">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="0efe7-247">Bağımsız değişken biçimi</span><span class="sxs-lookup"><span data-stu-id="0efe7-247">Argument format</span></span>                                                     | <span data-ttu-id="0efe7-248">Örnek</span><span class="sxs-lookup"><span data-stu-id="0efe7-248">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="0efe7-249">Tek bir bağımsız değişken: bir anahtar-değer çifti ayrılmış bir eşittir işareti (`=`)</span><span class="sxs-lookup"><span data-stu-id="0efe7-249">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="0efe7-250">İki bağımsız değişken dizisi: bir anahtar-değer çifti boşlukla ayrılmış</span><span class="sxs-lookup"><span data-stu-id="0efe7-250">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="0efe7-251">**Tek bir bağımsız değişken**</span><span class="sxs-lookup"><span data-stu-id="0efe7-251">**Single argument**</span></span>

<span data-ttu-id="0efe7-252">Değer bir eşittir işareti gelmelidir (`=`).</span><span class="sxs-lookup"><span data-stu-id="0efe7-252">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="0efe7-253">Değer null olabilir (örneğin, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="0efe7-253">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="0efe7-254">Anahtarı bir önek olabilir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-254">The key may have a prefix.</span></span>

| <span data-ttu-id="0efe7-255">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="0efe7-255">Key prefix</span></span>               | <span data-ttu-id="0efe7-256">Örnek</span><span class="sxs-lookup"><span data-stu-id="0efe7-256">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="0efe7-257">Önek yok</span><span class="sxs-lookup"><span data-stu-id="0efe7-257">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="0efe7-258">Tek bir tire (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="0efe7-258">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="0efe7-259">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="0efe7-259">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="0efe7-260">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="0efe7-260">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="0efe7-261">&#8224;Tek bir ön ekine sahip bir anahtar (`-`) belirtilmelidir [geçiş eşlemeleri](#switch-mappings)aşağıda açıklandığı gibi.</span><span class="sxs-lookup"><span data-stu-id="0efe7-261">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="0efe7-262">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="0efe7-262">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="0efe7-263">Not: Varsa `-key2` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı, verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0efe7-263">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="0efe7-264">**İki bağımsız değişken dizisi**</span><span class="sxs-lookup"><span data-stu-id="0efe7-264">**Sequence of two arguments**</span></span>

<span data-ttu-id="0efe7-265">Değer null olamaz ve bir boşluk ile ayrılan anahtarı izlemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="0efe7-265">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="0efe7-266">Anahtarı bir ön eki olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-266">The key must have a prefix.</span></span>

| <span data-ttu-id="0efe7-267">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="0efe7-267">Key prefix</span></span>               | <span data-ttu-id="0efe7-268">Örnek</span><span class="sxs-lookup"><span data-stu-id="0efe7-268">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="0efe7-269">Tek bir tire (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="0efe7-269">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="0efe7-270">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="0efe7-270">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="0efe7-271">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="0efe7-271">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="0efe7-272">&#8224;Tek bir ön ekine sahip bir anahtar (`-`) belirtilmelidir [geçiş eşlemeleri](#switch-mappings)aşağıda açıklandığı gibi.</span><span class="sxs-lookup"><span data-stu-id="0efe7-272">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="0efe7-273">Örnek komut:</span><span class="sxs-lookup"><span data-stu-id="0efe7-273">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="0efe7-274">Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı, verilen bir `FormatException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0efe7-274">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="0efe7-275">Yinelenen anahtarlar</span><span class="sxs-lookup"><span data-stu-id="0efe7-275">Duplicate keys</span></span>

<span data-ttu-id="0efe7-276">Yinelenen anahtarlar sağlanırsa, son anahtar-değer çifti kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-276">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="0efe7-277">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="0efe7-277">Switch mappings</span></span>

<span data-ttu-id="0efe7-278">El ile yapılandırma ile derleme yaparken `ConfigurationBuilder`, bir anahtar eşlemeleri sözlük eklenebilir `AddCommandLine` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0efe7-278">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="0efe7-279">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="0efe7-279">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="0efe7-280">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-280">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="0efe7-281">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) yapılandırma döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-281">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="0efe7-282">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="0efe7-282">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="0efe7-283">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="0efe7-283">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="0efe7-284">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="0efe7-284">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="0efe7-285">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-285">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="0efe7-286">Aşağıdaki örnekte, `GetSwitchMappings` yöntemi tek bir çizgi kullanılacak komut satırı bağımsız değişkenleri sağlar (`-`) anahtar öneki ve önde gelen alt anahtar önekleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="0efe7-286">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="0efe7-287">Komut satırı bağımsız değişkenleri sağlamadan sözlük için sağlanan `AddInMemoryCollection` yapılandırma değerlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0efe7-287">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="0efe7-288">Şu komutla uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0efe7-288">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="0efe7-289">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0efe7-289">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="0efe7-290">Yapılandırma ayarlarını geçirmek için aşağıdakileri kullanın:</span><span class="sxs-lookup"><span data-stu-id="0efe7-290">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="0efe7-291">Konsol penceresinde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0efe7-291">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="0efe7-292">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir:</span><span class="sxs-lookup"><span data-stu-id="0efe7-292">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="0efe7-293">Anahtar</span><span class="sxs-lookup"><span data-stu-id="0efe7-293">Key</span></span>            | <span data-ttu-id="0efe7-294">Değer</span><span class="sxs-lookup"><span data-stu-id="0efe7-294">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="0efe7-295">Anahtar geçişi sözlüğünü kullanarak göstermek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0efe7-295">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="0efe7-296">Komut satırı anahtarları değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-296">The command-line keys are swapped.</span></span> <span data-ttu-id="0efe7-297">Konsol penceresinde görüntüler için yapılandırma değerlerini `Profile:MachineName` ve `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="0efe7-297">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="0efe7-298">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="0efe7-298">web.config file</span></span>

<span data-ttu-id="0efe7-299">A *web.config* dosya, uygulama IIS veya IIS Express'te barındırma sırasında gereklidir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-299">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="0efe7-300">Ayarlarında *web.config* etkinleştirme [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) uygulamayı başlatın ve diğer IIS ayarlarını ve modülleri yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="0efe7-300">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="0efe7-301">Varsa *web.config* dosyası mevcut değil ve proje dosyasını içeren `<Project Sdk="Microsoft.NET.Sdk.Web">`, proje yayımlama oluşturur bir *web.config* yayımlanan çıkış dosyasında ( *Yayımlama* klasörü).</span><span class="sxs-lookup"><span data-stu-id="0efe7-301">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="0efe7-302">Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="0efe7-302">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="0efe7-303">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0efe7-303">Access configuration during startup</span></span>

<span data-ttu-id="0efe7-304">Erişim Yapılandırması içinde `ConfigureServices` veya `Configure` başlatma sırasında örneklere bakın [uygulama başlatma](xref:fundamentals/startup) konu.</span><span class="sxs-lookup"><span data-stu-id="0efe7-304">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="0efe7-305">Dış bütünleştirilmiş koddan ekleme yapılandırması</span><span class="sxs-lookup"><span data-stu-id="0efe7-305">Adding configuration from an external assembly</span></span>

<span data-ttu-id="0efe7-306">Bir [Ihostingstartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="0efe7-306">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="0efe7-307">Daha fazla bilgi için [dış bütünleştirilmiş koddan uygulama geliştiren](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="0efe7-307">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="0efe7-308">Erişim yapılandırmasında bir Razor sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="0efe7-308">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="0efe7-309">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](/dotnet/api/microsoft.extensions.configuration) ve ekleme [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="0efe7-309">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="0efe7-310">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="0efe7-310">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="0efe7-311">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="0efe7-311">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="0efe7-312">Ek Notlar</span><span class="sxs-lookup"><span data-stu-id="0efe7-312">Additional notes</span></span>

* <span data-ttu-id="0efe7-313">Bağımlılık ekleme (dı) ayarlanmamış kadar yukarı sonra `ConfigureServices` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-313">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="0efe7-314">Yapılandırma sistemi DI farkında değildir.</span><span class="sxs-lookup"><span data-stu-id="0efe7-314">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="0efe7-315">`IConfiguration` iki uzmanlıklar vardır:</span><span class="sxs-lookup"><span data-stu-id="0efe7-315">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="0efe7-316">`IConfigurationRoot` Kök düğümü için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-316">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="0efe7-317">Bir yeniden tetikleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0efe7-317">Can trigger a reload.</span></span>
  * <span data-ttu-id="0efe7-318">`IConfigurationSection` Yapılandırma değerleri bir bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0efe7-318">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="0efe7-319">`GetSection` Ve `GetChildren` yöntemleri döndürür bir `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="0efe7-319">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="0efe7-320">Kullanım [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) yapılandırma yeniden yükleme ya da her sağlayıcısına erişim için.</span><span class="sxs-lookup"><span data-stu-id="0efe7-320">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="0efe7-321">Bunlardan hiçbirine yaygındır.</span><span class="sxs-lookup"><span data-stu-id="0efe7-321">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0efe7-322">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0efe7-322">Additional resources</span></span>

* [<span data-ttu-id="0efe7-323">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="0efe7-323">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="0efe7-324">Birden çok ortam kullanma</span><span class="sxs-lookup"><span data-stu-id="0efe7-324">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="0efe7-325">Geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması</span><span class="sxs-lookup"><span data-stu-id="0efe7-325">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="0efe7-326">ASP.NET core'da konak</span><span class="sxs-lookup"><span data-stu-id="0efe7-326">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="0efe7-327">Bağımlılık Ekleme</span><span class="sxs-lookup"><span data-stu-id="0efe7-327">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="0efe7-328">Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="0efe7-328">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
