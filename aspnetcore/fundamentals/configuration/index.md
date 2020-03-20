---
title: ASP.NET Core yapılandırma
author: rick-anderson
description: ASP.NET Core uygulamasını yapılandırmak için yapılandırma API 'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: e1237db2625a127bfa5c31ac29b4394be6941b2f
ms.sourcegitcommit: 9e2b3aaccc9a41291eb23bf4561159e79cf6bc9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2020
ms.locfileid: "79546347"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="56fa4-103">ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="56fa4-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="56fa4-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Kirk larkabağı](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="56fa4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="56fa4-105">ASP.NET Core yapılandırma bir veya daha fazla [yapılandırma sağlayıcısı](#cp)kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-105">Configuration in ASP.NET Core is performed using one or more [configuration providers](#cp).</span></span> <span data-ttu-id="56fa4-106">Yapılandırma sağlayıcıları çeşitli yapılandırma kaynakları kullanarak anahtar-değer çiftlerinden yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-106">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

* <span data-ttu-id="56fa4-107">*AppSettings. JSON* gibi ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="56fa4-107">Settings files, such as *appsettings.json*</span></span>
* <span data-ttu-id="56fa4-108">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-108">Environment variables</span></span>
* <span data-ttu-id="56fa4-109">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="56fa4-109">Azure Key Vault</span></span>
* <span data-ttu-id="56fa4-110">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-110">Azure App Configuration</span></span>
* <span data-ttu-id="56fa4-111">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-111">Command-line arguments</span></span>
* <span data-ttu-id="56fa4-112">Özel sağlayıcılar, yüklendi veya oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="56fa4-112">Custom providers, installed or created</span></span>
* <span data-ttu-id="56fa4-113">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="56fa4-113">Directory files</span></span>
* <span data-ttu-id="56fa4-114">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-114">In-memory .NET objects</span></span>

<span data-ttu-id="56fa4-115">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56fa4-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="default"></a>

## <a name="default-configuration"></a><span data-ttu-id="56fa4-116">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="56fa4-116">Default configuration</span></span>

<span data-ttu-id="56fa4-117">[DotNet New](/dotnet/core/tools/dotnet-new) veya Visual Studio ile oluşturulan web Apps ASP.NET Core aşağıdaki kodu oluşturur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-117">ASP.NET Core web apps created with [dotnet new](/dotnet/core/tools/dotnet-new) or Visual Studio generate the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <span data-ttu-id="56fa4-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="56fa4-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="56fa4-119">[Chainedconfigurationprovider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) : mevcut bir `IConfiguration` kaynak olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Adds an existing `IConfiguration` as a source.</span></span> <span data-ttu-id="56fa4-120">Varsayılan yapılandırma durumunda, [ana bilgisayar](#hvac) yapılandırmasını ekler ve _uygulama_ yapılandırması için ilk kaynak olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-120">In the default configuration case, adds the [host](#hvac) configuration and setting it as the first source for the _app_ configuration.</span></span>
1. <span data-ttu-id="56fa4-121">[JSON yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak [appSettings. JSON](#appsettingsjson) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-121">[appsettings.json](#appsettingsjson) using the [JSON configuration provider](#file-configuration-provider).</span></span>
1. <span data-ttu-id="56fa4-122">*appSettings.* [JSON yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak`Environment` *. JSON* .</span><span class="sxs-lookup"><span data-stu-id="56fa4-122">*appsettings.*`Environment`*.json* using the [JSON configuration provider](#file-configuration-provider).</span></span> <span data-ttu-id="56fa4-123">Örneğin, *appSettings*. ***Üretim***. *JSON* ve *appSettings*. ***Geliştirme***. *JSON*.</span><span class="sxs-lookup"><span data-stu-id="56fa4-123">For example, *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json*.</span></span>
1. <span data-ttu-id="56fa4-124">Uygulama `Development` ortamda çalıştığında [uygulama gizli](xref:security/app-secrets) dizileri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-124">[App secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="56fa4-125">Ortam [değişkenleri yapılandırma sağlayıcısını](#evcp)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-125">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="56fa4-126">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-126">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="56fa4-127">Daha sonra eklenen yapılandırma sağlayıcıları önceki anahtar ayarlarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-127">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="56fa4-128">Örneğin, `MyKey` hem *appSettings. JSON* hem de ortamda ayarlandıysa, ortam değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-128">For example, if `MyKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="56fa4-129">Varsayılan yapılandırma sağlayıcılarını kullanarak, [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider) diğer tüm sağlayıcıları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-129">Using the default configuration providers, the  [Command-line configuration provider](#command-line-configuration-provider) overrides all other providers.</span></span>

<span data-ttu-id="56fa4-130">`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. [Varsayılan Oluşturucu ayarları](xref:fundamentals/host/generic-host#default-builder-settings).</span><span class="sxs-lookup"><span data-stu-id="56fa4-130">For more information on `CreateDefaultBuilder`, see [Default builder settings](xref:fundamentals/host/generic-host#default-builder-settings).</span></span>

<span data-ttu-id="56fa4-131">Aşağıdaki kod, etkin yapılandırma sağlayıcılarını eklendiği sırayla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-131">The following code displays the enabled configuration providers in the order they were added:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a><span data-ttu-id="56fa4-132">appSettings. JSON</span><span class="sxs-lookup"><span data-stu-id="56fa4-132">appsettings.json</span></span>

<span data-ttu-id="56fa4-133">Aşağıdaki *appSettings. JSON* dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="56fa4-133">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="56fa4-134">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-134">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-135">Varsayılan <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> yapılandırmayı aşağıdaki sırayla yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-135">The default <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration in the following order:</span></span>

1. <span data-ttu-id="56fa4-136">*appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="56fa4-136">*appsettings.json*</span></span>
1. <span data-ttu-id="56fa4-137">*appSettings.* `Environment` *. JSON* : Örneğin, *appSettings*. ***Üretim***. *JSON* ve *appSettings*. ***Geliştirme***. *JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="56fa4-137">*appsettings.*`Environment`*.json* : For example, the *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json* files.</span></span> <span data-ttu-id="56fa4-138">Dosyanın ortam sürümü, [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-138">The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="56fa4-139">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-139">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="56fa4-140">*appSettings*.`Environment`. *JSON* değerleri *appSettings. JSON*içindeki anahtarları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-140">*appsettings*.`Environment`.*json* values override keys in *appsettings.json*.</span></span> <span data-ttu-id="56fa4-141">Örneğin, varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="56fa4-141">For example, by default:</span></span>

* <span data-ttu-id="56fa4-142">Geliştirme sürümünde *appSettings*. ***Geliştirme***. *JSON* yapılandırması *appSettings. JSON*içinde bulunan değerlerin üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-142">In development, *appsettings*.***Development***.*json* configuration overwrites values found in *appsettings.json*.</span></span>
* <span data-ttu-id="56fa4-143">Üretimde, *appSettings*. ***Üretim***. *JSON* yapılandırması *appSettings. JSON*içinde bulunan değerlerin üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-143">In production, *appsettings*.***Production***.*json* configuration overwrites values found in *appsettings.json*.</span></span> <span data-ttu-id="56fa4-144">Örneğin, uygulamayı Azure 'a dağıtma.</span><span class="sxs-lookup"><span data-stu-id="56fa4-144">For example, when deploying the app to Azure.</span></span>

<a name="optpat"></a>

#### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a><span data-ttu-id="56fa4-145">Seçenek modelini kullanarak hiyerarşik yapılandırma verilerini bağlama</span><span class="sxs-lookup"><span data-stu-id="56fa4-145">Bind hierarchical configuration data using the options pattern</span></span>

<span data-ttu-id="56fa4-146">İlgili yapılandırma değerlerini okumak için tercih edilen yol, [Seçenekler modelini](xref:fundamentals/configuration/options)kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-146">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="56fa4-147">Örneğin, aşağıdaki yapılandırma değerlerini okumak için:</span><span class="sxs-lookup"><span data-stu-id="56fa4-147">For example, to read the following configuration values:</span></span>

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

<span data-ttu-id="56fa4-148">Aşağıdaki `PositionOptions` sınıfını oluşturun:</span><span class="sxs-lookup"><span data-stu-id="56fa4-148">Create the following `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

<span data-ttu-id="56fa4-149">Türün tüm genel okuma-yazma özellikleri bağlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-149">All the public read-write properties of the type are bound.</span></span> <span data-ttu-id="56fa4-150">Alanlar bağlanmadı ***.***</span><span class="sxs-lookup"><span data-stu-id="56fa4-150">Fields are ***not*** bound.</span></span>

<span data-ttu-id="56fa4-151">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="56fa4-151">The following code:</span></span>

* <span data-ttu-id="56fa4-152">`PositionOptions` sınıfını `Position` bölümüne bağlamak için [Configurationciltçi. Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) ' i çağırır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-152">Calls [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) to bind the `PositionOptions` class to the `Position` section.</span></span>
* <span data-ttu-id="56fa4-153">`Position` yapılandırma verilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-153">Displays the `Position` configuration data.</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlanır ve belirtilen türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-154">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="56fa4-155">`ConfigurationBinder.Get<T>` `ConfigurationBinder.Bind`kullanmaktan daha kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-155">`ConfigurationBinder.Get<T>` may be more convenient than using `ConfigurationBinder.Bind`.</span></span> <span data-ttu-id="56fa4-156">Aşağıdaki kod, `ConfigurationBinder.Get<T>` `PositionOptions` sınıfıyla nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-156">The following code shows how to use `ConfigurationBinder.Get<T>` with the `PositionOptions` class:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-157">***Seçenekler deseninin*** kullanılması, `Position` bölümünü bağlamak ve [bağımlılık ekleme hizmeti kapsayıcısına](xref:fundamentals/dependency-injection)eklemektir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-157">An alternative approach when using the ***options pattern*** is to bind the `Position` section and add it to the [dependency injection service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="56fa4-158">Aşağıdaki kodda, `PositionOptions` hizmet kapsayıcısına <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmaya bağlandığına eklenir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-158">In the following code, `PositionOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

<span data-ttu-id="56fa4-159">Önceki kodu kullanarak, aşağıdaki kod konum seçeneklerini okur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-159">Using the preceding code, the following code reads the position options:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-160">[Varsayılan](#default) yapılandırmayı kullanarak, *appSettings. JSON* ve *appSettings.* `Environment` *. JSON* dosyaları [reloadonchange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75)ile etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-160">Using the [default](#default) configuration, the *appsettings.json* and *appsettings.*`Environment`*.json* files are enabled with [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span></span> <span data-ttu-id="56fa4-161">Uygulama başladıktan ***sonra*** *appSettings. JSON* ve *appSettings.* `Environment` *. JSON* dosyasında yapılan değişiklikler [JSON yapılandırma sağlayıcısı](#jcp)tarafından okundu.</span><span class="sxs-lookup"><span data-stu-id="56fa4-161">Changes made to the *appsettings.json* and *appsettings.*`Environment`*.json* file ***after*** the app starts are read by the [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="56fa4-162">Ek JSON yapılandırma dosyaları ekleme hakkında bilgi için bu belgede [JSON yapılandırma sağlayıcısına](#jcp) bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-162">See [JSON configuration provider](#jcp) in this document for information on adding additional JSON configuration files.</span></span>

<a name="security"></a>

## <a name="security-and-secret-manager"></a><span data-ttu-id="56fa4-163">Güvenlik ve gizli dizi Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="56fa4-163">Security and secret manager</span></span>

<span data-ttu-id="56fa4-164">Yapılandırma verileri yönergeleri:</span><span class="sxs-lookup"><span data-stu-id="56fa4-164">Configuration data guidelines:</span></span>

* <span data-ttu-id="56fa4-165">Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-165">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="56fa4-166">[Gizli](xref:security/app-secrets) dizi, geliştirmelerde gizli dizileri depolamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-166">The [Secret manager](xref:security/app-secrets) can be used to store secrets in development.</span></span>
* <span data-ttu-id="56fa4-167">Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-167">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="56fa4-168">Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-168">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="56fa4-169">[Varsayılan](#default)olarak, [gizli yönetici](xref:security/app-secrets) *appSettings. JSON* ve *appSettings* .`Environment` *. JSON*sonrasında yapılandırma ayarlarını okur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-169">By [default](#default), [Secret manager](xref:security/app-secrets) reads configuration settings after *appsettings.json* and *appsettings.*`Environment`*.json*.</span></span>

<span data-ttu-id="56fa4-170">Parolaları veya diğer hassas verileri depolama hakkında daha fazla bilgi için:</span><span class="sxs-lookup"><span data-stu-id="56fa4-170">For more information on storing passwords or other sensitive data:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="56fa4-171"><xref:security/app-secrets>: hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler Içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-171"><xref:security/app-secrets>:  Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="56fa4-172">Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için [dosya yapılandırma sağlayıcısını](#fcp) kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-172">The Secret Manager uses the [File configuration provider](#fcp) to store user secrets in a JSON file on the local system.</span></span>

<span data-ttu-id="56fa4-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-173">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="56fa4-174">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-174">For more information, see <xref:security/key-vault-configuration>.</span></span>

<a name="evcp"></a>

## <a name="environment-variables"></a><span data-ttu-id="56fa4-175">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-175">Environment variables</span></span>

<span data-ttu-id="56fa4-176"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, [varsayılan](#default) yapılandırmayı kullanarak, *appSettings. JSON*, *appSettings.* `Environment` *. JSON*ve [gizli yönetici](xref:security/app-secrets)okumadan sonra anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-176">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and [Secret manager](xref:security/app-secrets).</span></span> <span data-ttu-id="56fa4-177">Bu nedenle, ortamdan okunan anahtar değerleri *appSettings. JSON*, *appSettings.* `Environment` *. JSON*ve gizli yöneticisinden okunan değerleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-177">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="56fa4-178">Aşağıdaki `set` komutları:</span><span class="sxs-lookup"><span data-stu-id="56fa4-178">The following `set` commands:</span></span>

* <span data-ttu-id="56fa4-179">Windows üzerinde [önceki örneğin](#appsettingsjson) ortam anahtarlarını ve değerlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-179">Set the environment keys and values of the [preceding example](#appsettingsjson) on Windows.</span></span>
* <span data-ttu-id="56fa4-180">[Örnek indirmeyi](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)kullanırken ayarları test edin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-180">Test the settings when using the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span></span> <span data-ttu-id="56fa4-181">`dotnet run` komutunun proje dizininde çalıştırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-181">The `dotnet run` command must be run in the project directory.</span></span>

```cmd
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

<span data-ttu-id="56fa4-182">Önceki ortam ayarları:</span><span class="sxs-lookup"><span data-stu-id="56fa4-182">The preceding environment settings:</span></span>

* <span data-ttu-id="56fa4-183">Yalnızca ' de ayarlanan komut penceresinden başlatılan işlemlerde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-183">Are only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="56fa4-184">Visual Studio ile başlatılan tarayıcılar tarafından okunmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-184">Won't be read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="56fa4-185">Aşağıdaki [Setx](/windows-server/administration/windows-commands/setx) komutları Windows üzerinde ortam anahtarlarını ve değerlerini ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-185">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="56fa4-186">`set`farklı olarak, `setx` ayarları kalıcı hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-186">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="56fa4-187">`/M`, Sistem ortamındaki değişkeni ayarlar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-187">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="56fa4-188">`/M` anahtarı kullanılmazsa, bir kullanıcı ortam değişkeni ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-188">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

<span data-ttu-id="56fa4-189">Önceki komutların *apsettings. JSON* ve *appSettings.* `Environment` *. JSON*' i geçersiz kılmasını test etmek için:</span><span class="sxs-lookup"><span data-stu-id="56fa4-189">To test that the preceding commands override *apsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

* <span data-ttu-id="56fa4-190">Visual Studio ile: Exit ve Visual Studio 'Yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-190">With Visual Studio: Exit and restart Visual Studio.</span></span>
* <span data-ttu-id="56fa4-191">CLı ile: yeni bir komut penceresi başlatın ve `dotnet run`girin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-191">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="56fa4-192">Ortam değişkenlerinin önekini belirtmek için bir dizeyle <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-192">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> with a string to specify a prefix for environment variables:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

<span data-ttu-id="56fa4-193">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="56fa4-193">In the preceding code:</span></span>

* <span data-ttu-id="56fa4-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")`, [varsayılan yapılandırma sağlayıcılarından](#default)sonra eklenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-194">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="56fa4-195">Yapılandırma sağlayıcılarını sipariş eden bir örnek için bkz. [JSON yapılandırma sağlayıcısı](#jcp).</span><span class="sxs-lookup"><span data-stu-id="56fa4-195">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>
* <span data-ttu-id="56fa4-196">`MyCustomPrefix_` önekiyle ayarlanan ortam değişkenleri [varsayılan yapılandırma sağlayıcılarını](#default)geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-196">Environment variables set with the `MyCustomPrefix_` prefix override the [default configuration providers](#default).</span></span> <span data-ttu-id="56fa4-197">Bu, öneki olmayan ortam değişkenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-197">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="56fa4-198">Yapılandırma anahtar-değer çiftleri okunduktan sonra önek çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-198">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="56fa4-199">Aşağıdaki komutlar özel öneki test et:</span><span class="sxs-lookup"><span data-stu-id="56fa4-199">The following commands test the custom prefix:</span></span>

```cmd
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

<span data-ttu-id="56fa4-200">[Varsayılan yapılandırma](#default) , `DOTNET_` ve `ASPNETCORE_`önekli ortam değişkenlerini ve komut satırı bağımsız değişkenlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-200">The [default configuration](#default) loads environment variables and command line arguments prefixed with `DOTNET_` and `ASPNETCORE_`.</span></span> <span data-ttu-id="56fa4-201">`DOTNET_` ve `ASPNETCORE_` önekleri [konak ve uygulama yapılandırması](xref:fundamentals/host/generic-host#host-configuration)için ASP.NET Core tarafından kullanılır, ancak kullanıcı yapılandırması için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-201">The `DOTNET_` and `ASPNETCORE_` prefixes are used by ASP.NET Core for [host and app configuration](xref:fundamentals/host/generic-host#host-configuration), but not for user configuration.</span></span> <span data-ttu-id="56fa4-202">Konak ve uygulama yapılandırması hakkında daha fazla bilgi için bkz. [.NET genel ana bilgisayar](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="56fa4-202">For more information on host and app configuration, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

<span data-ttu-id="56fa4-203">[Azure App Service](https://azure.microsoft.com/services/app-service/), **Ayarlar > yapılandırma** sayfasında **Yeni uygulama ayarı** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-203">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="56fa4-204">Azure App Service uygulama ayarları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-204">Azure App Service application settings are:</span></span>

* <span data-ttu-id="56fa4-205">Rest 'de şifrelenir ve şifreli bir kanal üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-205">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="56fa4-206">Ortam değişkenleri olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-206">Exposed as environment variables.</span></span>

<span data-ttu-id="56fa4-207">Daha fazla bilgi için bkz. [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="56fa4-207">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="56fa4-208">Azure veritabanı bağlantı dizeleri hakkında bilgi için bkz. [bağlantı dizesi önekleri](#constr) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-208">See [Connection string prefixes](#constr) for information on Azure database connection strings.</span></span>

<a name="clcp"></a>

## <a name="command-line"></a><span data-ttu-id="56fa4-209">Komut Satırı</span><span class="sxs-lookup"><span data-stu-id="56fa4-209">Command-line</span></span>

<span data-ttu-id="56fa4-210"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> [varsayılan](#default) yapılandırmayı kullanarak, aşağıdaki yapılandırma kaynaklarından sonra komut satırı bağımsız değişkeni anahtar-değer çiftleriyle yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-210">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

* <span data-ttu-id="56fa4-211">*appSettings. JSON* ve *appSettings*.`Environment`. *JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="56fa4-211">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="56fa4-212">Geliştirme ortamında [uygulama gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-212">[App secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="56fa4-213">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-213">Environment variables.</span></span>

<span data-ttu-id="56fa4-214">[Varsayılan](#default)olarak, komut satırı geçersiz kılma yapılandırma değerleri, diğer tüm yapılandırma sağlayıcılarıyla ayarlanan yapılandırma değerleri olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-214">By [default](#default), configuration values set on the command-line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="56fa4-215">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-215">Command-line arguments</span></span>

<span data-ttu-id="56fa4-216">Aşağıdaki komut `=`kullanarak anahtarları ve değerleri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="56fa4-216">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

<span data-ttu-id="56fa4-217">Aşağıdaki komut `/`kullanarak anahtarları ve değerleri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="56fa4-217">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

<span data-ttu-id="56fa4-218">Aşağıdaki komut `--`kullanarak anahtarları ve değerleri ayarlar:</span><span class="sxs-lookup"><span data-stu-id="56fa4-218">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

<span data-ttu-id="56fa4-219">Anahtar değeri:</span><span class="sxs-lookup"><span data-stu-id="56fa4-219">The key value:</span></span>

* <span data-ttu-id="56fa4-220">`=`izlemelidir ya da anahtarın bir boşluk izleyen bir `--` veya `/` ön ekine sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-220">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
* <span data-ttu-id="56fa4-221">`=` kullanılıyorsa gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-221">Isn't required if `=` is used.</span></span> <span data-ttu-id="56fa4-222">Örneğin, `MySetting=`.</span><span class="sxs-lookup"><span data-stu-id="56fa4-222">For example, `MySetting=`.</span></span>

<span data-ttu-id="56fa4-223">Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile `=` kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-223">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="56fa4-224">Eşleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="56fa4-224">Switch mappings</span></span>

<span data-ttu-id="56fa4-225">Anahtar eşlemeleri **anahtar** adı değiştirme mantığına izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-225">Switch mappings allow **key** name replacement logic.</span></span> <span data-ttu-id="56fa4-226"><xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoduna anahtar değiştirmeler sözlüğü sağlayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-226">Provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="56fa4-227">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-227">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="56fa4-228">Komut satırı anahtarı sözlükte bulunursa, sözlük değeri, anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-228">If the command-line key is found in the dictionary, the dictionary value is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="56fa4-229">Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-229">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="56fa4-230">Anahtar eşlemeleri sözlük anahtarı kuralları:</span><span class="sxs-lookup"><span data-stu-id="56fa4-230">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="56fa4-231">Anahtarlar `-` veya `--`başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-231">Switches must start with `-` or `--`.</span></span>
* <span data-ttu-id="56fa4-232">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-232">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="56fa4-233">Anahtar eşlemeleri sözlüğünü kullanmak için, `AddCommandLine`çağrıya geçirin:</span><span class="sxs-lookup"><span data-stu-id="56fa4-233">To use a switch mappings dictionary, pass it into the call to `AddCommandLine`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

<span data-ttu-id="56fa4-234">Aşağıdaki kod, değiştirilmiş anahtarların anahtar değerlerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-234">The following code shows the key values for the replaced keys:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-235">Anahtar değişimini test etmek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-235">Run the following command to test the key replacement:</span></span>

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="56fa4-236">Note: Şu anda `=`, anahtar değiştirme değerlerini tek bir tire `-`ayarlamak için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-236">Note: Currently, `=` cannot be used to set key-replacement values with a single dash `-`.</span></span> <span data-ttu-id="56fa4-237">[Bu GitHub sorununa](https://github.com/dotnet/extensions/issues/3059)bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-237">See [this GitHub issue](https://github.com/dotnet/extensions/issues/3059).</span></span>

<span data-ttu-id="56fa4-238">Aşağıdaki komut, anahtar değişimini test etmek için işe yarar:</span><span class="sxs-lookup"><span data-stu-id="56fa4-238">The following command works to test key replacement:</span></span>

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="56fa4-239">Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-239">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="56fa4-240">`CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`geçirmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-240">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch-mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="56fa4-241">Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="56fa4-241">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch-mapping dictionary.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="56fa4-242">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="56fa4-242">Hierarchical configuration data</span></span>

<span data-ttu-id="56fa4-243">Yapılandırma API 'SI, hiyerarşik verileri, yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini okur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-243">The Configuration API is reads hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="56fa4-244">[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki *appSettings. JSON* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-244">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="56fa4-245">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, yapılandırma ayarlarından birkaçını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-245">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-246">Hiyerarşik yapılandırma verilerini okumak için tercih edilen yol, Seçenekler modelini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-246">The preferred way to read hierarchical configuration data is using the options pattern.</span></span> <span data-ttu-id="56fa4-247">Daha fazla bilgi için, bkz. bu belgedeki [Hiyerarşik yapılandırma verilerini bağlama](#optpat) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-247">For more information, see [Bind hierarchical configuration data](#optpat) in this document.</span></span>

<span data-ttu-id="56fa4-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-248"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="56fa4-249">Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-249">These methods are described later in [GetSection, GetChildren, and Exists](#getsection).</span></span>

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a><span data-ttu-id="56fa4-250">Yapılandırma anahtarları ve değerleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-250">Configuration keys and values</span></span>

<span data-ttu-id="56fa4-251">Yapılandırma anahtarları:</span><span class="sxs-lookup"><span data-stu-id="56fa4-251">Configuration keys:</span></span>

* <span data-ttu-id="56fa4-252">Büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-252">Are case-insensitive.</span></span> <span data-ttu-id="56fa4-253">Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-253">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="56fa4-254">Bir anahtar ve değer birden fazla yapılandırma sağlayıcısından ayarlandıysa, eklenen son sağlayıcıdan alınan değer kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-254">If a key and value is set in more than one configuration providers, the value from the last provider added is used.</span></span> <span data-ttu-id="56fa4-255">Daha fazla bilgi için bkz. [varsayılan yapılandırma](#default).</span><span class="sxs-lookup"><span data-stu-id="56fa4-255">For more information, see [Default configuration](#default).</span></span>
* <span data-ttu-id="56fa4-256">Hiyerarşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="56fa4-256">Hierarchical keys</span></span>
  * <span data-ttu-id="56fa4-257">Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-257">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="56fa4-258">Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-258">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="56fa4-259">Çift alt çizgi, `__`tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta üst üste `:`dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-259">A double underscore, `__`, is supported by all platforms and is automatically converted into a colon `:`.</span></span>
  * <span data-ttu-id="56fa4-260">Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-260">In Azure Key Vault, hierarchical keys use `--` as a separator.</span></span> <span data-ttu-id="56fa4-261">Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde `:` `--` değiştirmek için kod yazın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-261">Write code to replace the `--` with a `:` when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="56fa4-262"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-262">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="56fa4-263">Dizi bağlama, [diziyi bir sınıfa bağlama](#boa) bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-263">Array binding is described in the [Bind an array to a class](#boa) section.</span></span>

<span data-ttu-id="56fa4-264">Yapılandırma değerleri:</span><span class="sxs-lookup"><span data-stu-id="56fa4-264">Configuration values:</span></span>

* <span data-ttu-id="56fa4-265">Dizeler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-265">Are strings.</span></span>
* <span data-ttu-id="56fa4-266">Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-266">Null values can't be stored in configuration or bound to objects.</span></span>

<a name="cp"></a>

## <a name="configuration-providers"></a><span data-ttu-id="56fa4-267">Yapılandırma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="56fa4-267">Configuration providers</span></span>

<span data-ttu-id="56fa4-268">Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-268">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="56fa4-269">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="56fa4-269">Provider</span></span> | <span data-ttu-id="56fa4-270">Şuradan yapılandırma sağlar</span><span class="sxs-lookup"><span data-stu-id="56fa4-270">Provides configuration from</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="56fa4-271">Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-271">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration) | <span data-ttu-id="56fa4-272">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="56fa4-272">Azure Key Vault</span></span> |
| [<span data-ttu-id="56fa4-273">Azure uygulama yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-273">Azure App configuration provider</span></span>](/azure/azure-app-configuration/quickstart-aspnet-core-app) | <span data-ttu-id="56fa4-274">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-274">Azure App Configuration</span></span> |
| [<span data-ttu-id="56fa4-275">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-275">Command-line configuration provider</span></span>](#clcp) | <span data-ttu-id="56fa4-276">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-276">Command-line parameters</span></span> |
| [<span data-ttu-id="56fa4-277">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-277">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="56fa4-278">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="56fa4-278">Custom source</span></span> |
| [<span data-ttu-id="56fa4-279">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-279">Environment Variables configuration provider</span></span>](#evcp) | <span data-ttu-id="56fa4-280">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-280">Environment variables</span></span> |
| [<span data-ttu-id="56fa4-281">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-281">File configuration provider</span></span>](#file-configuration-provider) | <span data-ttu-id="56fa4-282">INı, JSON ve XML dosyaları</span><span class="sxs-lookup"><span data-stu-id="56fa4-282">INI, JSON, and XML files</span></span> |
| [<span data-ttu-id="56fa4-283">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-283">Key-per-file configuration provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="56fa4-284">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="56fa4-284">Directory files</span></span> |
| [<span data-ttu-id="56fa4-285">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-285">Memory configuration provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="56fa4-286">Bellek içi Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="56fa4-286">In-memory collections</span></span> |
| [<span data-ttu-id="56fa4-287">Gizli dizi Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="56fa4-287">Secret Manager</span></span>](xref:security/app-secrets)  | <span data-ttu-id="56fa4-288">Kullanıcı profili dizinindeki dosya</span><span class="sxs-lookup"><span data-stu-id="56fa4-288">File in the user profile directory</span></span> |

<span data-ttu-id="56fa4-289">Yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="56fa4-289">Configuration sources are read in the order that their configuration providers are specified.</span></span> <span data-ttu-id="56fa4-290">Koddaki yapılandırma sağlayıcılarını, uygulamanın gerektirdiği temel yapılandırma kaynakları için önceliklere uyacak şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-290">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="56fa4-291">Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-291">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="56fa4-292">*appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="56fa4-292">*appsettings.json*</span></span>
1. <span data-ttu-id="56fa4-293">*appSettings*.`Environment`. *JSON*</span><span class="sxs-lookup"><span data-stu-id="56fa4-293">*appsettings*.`Environment`.*json*</span></span>
1. [<span data-ttu-id="56fa4-294">Gizli dizi Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="56fa4-294">Secret Manager</span></span>](xref:security/app-secrets)
1. <span data-ttu-id="56fa4-295">Ortam [değişkenleri yapılandırma sağlayıcısını](#evcp)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-295">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="56fa4-296">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-296">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="56fa4-297">Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasına izin vermek için komut satırı yapılandırma sağlayıcısını en son bir sağlayıcı dizisine eklemektir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-297">A common practice is to add the Command-line configuration provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="56fa4-298">Önceki sağlayıcı dizisi [Varsayılan yapılandırmada](#default)kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-298">The preceding sequence of providers is used in the [default configuration](#default).</span></span>

<a name="constr"></a>

### <a name="connection-string-prefixes"></a><span data-ttu-id="56fa4-299">Bağlantı dizesi önekleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-299">Connection string prefixes</span></span>

<span data-ttu-id="56fa4-300">Yapılandırma API 'sinin dört bağlantı dizesi ortam değişkeni için özel işleme kuralları vardır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-300">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="56fa4-301">Bu bağlantı dizeleri, uygulama ortamı için Azure bağlantı dizelerini yapılandırmaya dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-301">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="56fa4-302">Tabloda gösterilen öneklere sahip ortam değişkenleri [varsayılan yapılandırmayla](#default) uygulamaya yüklenir veya `AddEnvironmentVariables`için hiç önek sağlanmaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-302">Environment variables with the prefixes shown in the table are loaded into the app with the [default configuration](#default) or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="56fa4-303">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="56fa4-303">Connection string prefix</span></span> | <span data-ttu-id="56fa4-304">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="56fa4-304">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="56fa4-305">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="56fa4-305">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="56fa4-306">MySQL</span><span class="sxs-lookup"><span data-stu-id="56fa4-306">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="56fa4-307">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="56fa4-307">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="56fa4-308">SQL Server</span><span class="sxs-lookup"><span data-stu-id="56fa4-308">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="56fa4-309">Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:</span><span class="sxs-lookup"><span data-stu-id="56fa4-309">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="56fa4-310">Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-310">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="56fa4-311">Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).</span><span class="sxs-lookup"><span data-stu-id="56fa4-311">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="56fa4-312">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="56fa4-312">Environment variable key</span></span> | <span data-ttu-id="56fa4-313">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="56fa4-313">Converted configuration key</span></span> | <span data-ttu-id="56fa4-314">Sağlayıcı yapılandırma girişi</span><span class="sxs-lookup"><span data-stu-id="56fa4-314">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-315">Yapılandırma girişi oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="56fa4-315">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-316">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="56fa4-316">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="56fa4-317">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="56fa4-317">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-318">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="56fa4-318">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="56fa4-319">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="56fa4-319">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-320">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="56fa4-320">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="56fa4-321">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="56fa4-321">Value: `System.Data.SqlClient`</span></span>  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a><span data-ttu-id="56fa4-322">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-322">JSON configuration provider</span></span>

<span data-ttu-id="56fa4-323"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, yapılandırmayı JSON dosya anahtar-değer çiftlerinden yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-323">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs.</span></span>

<span data-ttu-id="56fa4-324">Aşırı yüklemeler şunları belirtebilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-324">Overloads can specify:</span></span>

* <span data-ttu-id="56fa4-325">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="56fa4-325">Whether the file is optional.</span></span>
* <span data-ttu-id="56fa4-326">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-326">Whether the configuration is reloaded if the file changes.</span></span>

<span data-ttu-id="56fa4-327">Aşağıdaki kodu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="56fa4-327">Consider the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

<span data-ttu-id="56fa4-328">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="56fa4-328">The preceding code:</span></span>

* <span data-ttu-id="56fa4-329">JSON yapılandırma sağlayıcısını, *MyConfig. JSON* dosyasını yüklemek için aşağıdaki seçeneklerle yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-329">Configures the JSON configuration provider to load the *MyConfig.json* file with the following options:</span></span>
  * <span data-ttu-id="56fa4-330">`optional: true`: dosya isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-330">`optional: true`: The file is optional.</span></span>
  * <span data-ttu-id="56fa4-331">`reloadOnChange: true`: değişiklikler kaydedildiğinde dosya yeniden yüklenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-331">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
* <span data-ttu-id="56fa4-332">*MyConfig. JSON* dosyasından önce [varsayılan yapılandırma sağlayıcılarını](#default) okur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-332">Reads the [default configuration providers](#default) before the *MyConfig.json* file.</span></span> <span data-ttu-id="56fa4-333">[Ortam değişkenleri yapılandırma sağlayıcısı](#evcp) ve [komut satırı yapılandırma sağlayıcısı](#clcp)da dahil olmak üzere varsayılan yapılandırma sağlayıcılarındaki *MyConfig. JSON* dosyası geçersiz kılma ayarındaki ayarlar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-333">Settings in the *MyConfig.json* file override setting in the default configuration providers, including the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="56fa4-334">Genellikle, [ortam değişkenleri Yapılandırma sağlayıcısında](#evcp) ve [komut satırı yapılandırma sağlayıcısında](#clcp)ayarlanmış özel bir JSON dosyası değerlerini geçersiz ***kılmayı istemezsiniz.***</span><span class="sxs-lookup"><span data-stu-id="56fa4-334">You typically ***don't*** want a custom JSON file overriding values set in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="56fa4-335">Aşağıdaki kod tüm yapılandırma sağlayıcılarını temizler ve çeşitli yapılandırma sağlayıcıları ekler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-335">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

<span data-ttu-id="56fa4-336">Yukarıdaki kodda, *MyConfig. JSON* ve *MyConfig*içindeki ayarlar.`Environment`. *JSON* dosyaları:</span><span class="sxs-lookup"><span data-stu-id="56fa4-336">In the preceding code, settings in the *MyConfig.json* and  *MyConfig*.`Environment`.*json* files:</span></span>

* <span data-ttu-id="56fa4-337">*AppSettings. JSON* ve *appSettings*içindeki ayarları geçersiz kılın.`Environment`. *JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="56fa4-337">Override settings in the *appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="56fa4-338">, [Ortam değişkenleri yapılandırma sağlayıcısı](#evcp) ve [komut satırı yapılandırma sağlayıcısı](#clcp)ayarları tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-338">Are overridden by settings in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="56fa4-339">[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) , şu *MyConfig. JSON* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-339">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *MyConfig.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

<span data-ttu-id="56fa4-340">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-340">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a><span data-ttu-id="56fa4-341">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-341">File configuration provider</span></span>

<span data-ttu-id="56fa4-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-342"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="56fa4-343">Aşağıdaki yapılandırma sağlayıcıları `FileConfigurationProvider`türetilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-343">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

* [<span data-ttu-id="56fa4-344">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-344">INI configuration provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="56fa4-345">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-345">JSON configuration provider</span></span>](#jcp)
* [<span data-ttu-id="56fa4-346">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-346">XML configuration provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="56fa4-347">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-347">INI configuration provider</span></span>

<span data-ttu-id="56fa4-348"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-348">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="56fa4-349">Aşağıdaki kod tüm yapılandırma sağlayıcılarını temizler ve çeşitli yapılandırma sağlayıcıları ekler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-349">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

<span data-ttu-id="56fa4-350">Yukarıdaki kodda, *myıniconfig. ini* ve *myıniconfig*içindeki ayarlar.`Environment`. *ını* dosyaları, içindeki ayarlar tarafından geçersiz kılınır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-350">In the preceding code, settings in the *MyIniConfig.ini* and  *MyIniConfig*.`Environment`.*ini* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="56fa4-351">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-351">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="56fa4-352">[Komut satırı yapılandırma sağlayıcısı](#clcp).</span><span class="sxs-lookup"><span data-stu-id="56fa4-352">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="56fa4-353">[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) , aşağıdaki *Myıniconfig. ini* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-353">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyIniConfig.ini* file:</span></span>

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

<span data-ttu-id="56fa4-354">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-354">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a><span data-ttu-id="56fa4-355">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-355">XML configuration provider</span></span>

<span data-ttu-id="56fa4-356"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-356">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="56fa4-357">Aşağıdaki kod tüm yapılandırma sağlayıcılarını temizler ve çeşitli yapılandırma sağlayıcıları ekler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-357">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

<span data-ttu-id="56fa4-358">Yukarıdaki kodda, *myXMLfile. xml* ve *myXMLfile*içindeki ayarlar.`Environment`. *XML* dosyaları, içindeki ayarlar tarafından geçersiz kılınır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-358">In the preceding code, settings in the *MyXMLFile.xml* and  *MyXMLFile*.`Environment`.*xml* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="56fa4-359">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-359">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="56fa4-360">[Komut satırı yapılandırma sağlayıcısı](#clcp).</span><span class="sxs-lookup"><span data-stu-id="56fa4-360">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="56fa4-361">[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) , şu *myXMLfile. xml* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-361">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyXMLFile.xml* file:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

<span data-ttu-id="56fa4-362">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-362">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-363">Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-363">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

<span data-ttu-id="56fa4-364">Aşağıdaki kod, önceki yapılandırma dosyasını okur ve anahtarları ve değerleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-364">The following code reads the previous configuration file and displays the keys and values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-365">Öznitelikler, değerler sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-365">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="56fa4-366">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-366">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="56fa4-367">anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="56fa4-367">key:attribute</span></span>
* <span data-ttu-id="56fa4-368">Section: Key: özniteliği</span><span class="sxs-lookup"><span data-stu-id="56fa4-368">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="56fa4-369">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-369">Key-per-file configuration provider</span></span>

<span data-ttu-id="56fa4-370"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-370">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="56fa4-371">Anahtar, dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-371">The key is the file name.</span></span> <span data-ttu-id="56fa4-372">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-372">The value contains the file's contents.</span></span> <span data-ttu-id="56fa4-373">Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-373">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="56fa4-374">Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-374">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="56fa4-375">Dosyaların `directoryPath` mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-375">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="56fa4-376">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="56fa4-376">Overloads permit specifying:</span></span>

* <span data-ttu-id="56fa4-377">Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="56fa4-377">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="56fa4-378">Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-378">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="56fa4-379">Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-379">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="56fa4-380">Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-380">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="56fa4-381">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a><span data-ttu-id="56fa4-382">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-382">Memory configuration provider</span></span>

<span data-ttu-id="56fa4-383"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-383">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="56fa4-384">Aşağıdaki kod, yapılandırma sistemine bir bellek koleksiyonu ekler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-384">The following code adds a memory collection to the configuration system:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

<span data-ttu-id="56fa4-385">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-385">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-386">Yukarıdaki kodda, `config.AddInMemoryCollection(Dict)` [varsayılan yapılandırma sağlayıcılarından](#default)sonra eklenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-386">In the preceding code, `config.AddInMemoryCollection(Dict)` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="56fa4-387">Yapılandırma sağlayıcılarını sipariş eden bir örnek için bkz. [JSON yapılandırma sağlayıcısı](#jcp).</span><span class="sxs-lookup"><span data-stu-id="56fa4-387">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="56fa4-388">Yapılandırma sağlayıcılarını sipariş eden bir örnek için bkz. [JSON yapılandırma sağlayıcısı](#jcp).</span><span class="sxs-lookup"><span data-stu-id="56fa4-388">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="56fa4-389">Bkz. `MemoryConfigurationProvider`kullanarak bir diziyi başka bir örnek için [bağlama](#boa) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-389">See [Bind an array](#boa) for another example using `MemoryConfigurationProvider`.</span></span>

## <a name="getvalue"></a><span data-ttu-id="56fa4-390">GetValue</span><span class="sxs-lookup"><span data-stu-id="56fa4-390">GetValue</span></span>

<span data-ttu-id="56fa4-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen türe dönüştürür:</span><span class="sxs-lookup"><span data-stu-id="56fa4-391">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified type:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-392">Yukarıdaki kodda, yapılandırmada `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-392">In the preceding code,  if `NumberKey` isn't found in the configuration, the default value of `99` is used.</span></span>

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="56fa4-393">GetSection, GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="56fa4-393">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="56fa4-394">İzleyen örnekler için aşağıdaki *Myalt bölüm. JSON* dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="56fa4-394">For the examples that follow, consider the following *MySubsection.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

<span data-ttu-id="56fa4-395">Aşağıdaki kod, *Myalt bölüm. JSON* öğesini yapılandırma sağlayıcılarına ekler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-395">The following code adds *MySubsection.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a><span data-ttu-id="56fa4-396">GetSection</span><span class="sxs-lookup"><span data-stu-id="56fa4-396">GetSection</span></span>

<span data-ttu-id="56fa4-397">[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="56fa4-397">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) returns a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="56fa4-398">Aşağıdaki kod `section1`için değerler döndürür:</span><span class="sxs-lookup"><span data-stu-id="56fa4-398">The following code returns values for `section1`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-399">Aşağıdaki kod `section2:subsection0`için değerler döndürür:</span><span class="sxs-lookup"><span data-stu-id="56fa4-399">The following code returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-400">`GetSection` hiçbir şekilde `null`döndürmez.</span><span class="sxs-lookup"><span data-stu-id="56fa4-400">`GetSection` never returns `null`.</span></span> <span data-ttu-id="56fa4-401">Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-401">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="56fa4-402">`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor.</span><span class="sxs-lookup"><span data-stu-id="56fa4-402">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="56fa4-403">Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-403">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren-and-exists"></a><span data-ttu-id="56fa4-404">GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="56fa4-404">GetChildren and Exists</span></span>

<span data-ttu-id="56fa4-405">Aşağıdaki kod, [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 'ı çağırır ve `section2:subsection0`için değerleri döndürür:</span><span class="sxs-lookup"><span data-stu-id="56fa4-405">The following code calls [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) and returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-406">Yukarıdaki kod, bir bölümü doğrulamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) öğesini çağırır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-406">The preceding code calls [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to verify the  section exists:</span></span>

 <a name="boa"></a>

## <a name="bind-an-array"></a><span data-ttu-id="56fa4-407">Bir diziyi bağlama</span><span class="sxs-lookup"><span data-stu-id="56fa4-407">Bind an array</span></span>

<span data-ttu-id="56fa4-408">[Configurationciltçi. Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) , yapılandırma anahtarlarındaki dizi dizinlerini kullanarak dizilere dizi nesneleri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-408">The [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="56fa4-409">Sayısal anahtar segmentini ortaya çıkaran herhangi bir dizi biçimi, [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) sınıf dizisine dizi bağlama özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-409">Any array format that exposes a numeric key segment is capable of array binding to a [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) class array.</span></span>

<span data-ttu-id="56fa4-410">[Örnek indirmenizde](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) *myArray. JSON* ' i göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="56fa4-410">Consider *MyArray.json* from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

<span data-ttu-id="56fa4-411">Aşağıdaki kod, yapılandırma sağlayıcılarına *myArray. JSON* ekler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-411">The following code adds *MyArray.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

<span data-ttu-id="56fa4-412">Aşağıdaki kod yapılandırmayı okur ve değerleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-412">The following code reads the configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-413">Yukarıdaki kod aşağıdaki çıktıyı döndürür:</span><span class="sxs-lookup"><span data-stu-id="56fa4-413">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

<span data-ttu-id="56fa4-414">Önceki çıktıda, Dizin 3 ' te, *myArray. JSON*içinde `"4": "value40",` karşılık gelen `value40`değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-414">In the preceding output, Index 3 has value `value40`, corresponding to `"4": "value40",` in *MyArray.json*.</span></span> <span data-ttu-id="56fa4-415">Bağlantılı dizi dizinleri sürekli ve yapılandırma anahtarı diziniyle bağlantılı değildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-415">The bound array indices are continuous and not bound to the configuration key index.</span></span> <span data-ttu-id="56fa4-416">Yapılandırma Bağlayıcısı, bağlantılı nesnelerde null değerleri bağlama veya null girişler oluşturma yeteneğine sahip değil</span><span class="sxs-lookup"><span data-stu-id="56fa4-416">The configuration binder isn't capable of binding null values or creating null entries in bound objects</span></span>

<span data-ttu-id="56fa4-417">Aşağıdaki kod, <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı yöntemiyle `array:entries` yapılandırmasını yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-417">The  following code loads the `array:entries` configuration with the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

<span data-ttu-id="56fa4-418">Aşağıdaki kod `arrayDict` `Dictionary` yapılandırmayı okur ve değerleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-418">The following code reads the configuration in the `arrayDict` `Dictionary` and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-419">Yukarıdaki kod aşağıdaki çıktıyı döndürür:</span><span class="sxs-lookup"><span data-stu-id="56fa4-419">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

<span data-ttu-id="56fa4-420">İlişkili nesnede &num;3 dizini, `array:4` yapılandırma anahtarı ve `value4`değeri için yapılandırma verilerini tutar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-420">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="56fa4-421">Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri, nesne oluştururken yapılandırma verilerini yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-421">When configuration data containing an array is bound, the array indices in the configuration keys are used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="56fa4-422">Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-422">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="56fa4-423">&num;3 dizini için eksik yapılandırma öğesi, Dizin &num;3 anahtar/değer çiftini okuyan herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-423">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that reads the index &num;3 key/value pair.</span></span> <span data-ttu-id="56fa4-424">Örnek indirmenin aşağıdaki *Value3. JSON* dosyasını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="56fa4-424">Consider the following *Value3.json* file from the sample download:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

<span data-ttu-id="56fa4-425">Aşağıdaki kod *Value3. JSON* yapılandırmasını ve `arrayDict` `Dictionary`içerir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-425">The following code includes configuration for *Value3.json* and the `arrayDict` `Dictionary`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

<span data-ttu-id="56fa4-426">Aşağıdaki kod önceki yapılandırmayı okur ve değerleri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-426">The following code reads the preceding configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="56fa4-427">Yukarıdaki kod aşağıdaki çıktıyı döndürür:</span><span class="sxs-lookup"><span data-stu-id="56fa4-427">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

<span data-ttu-id="56fa4-428">Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-428">Custom configuration providers aren't required to implement array binding.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="56fa4-429">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-429">Custom configuration provider</span></span>

<span data-ttu-id="56fa4-430">Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-430">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="56fa4-431">Sağlayıcı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-431">The provider has the following characteristics:</span></span>

* <span data-ttu-id="56fa4-432">EF bellek içi veritabanı, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-432">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="56fa4-433">Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-433">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="56fa4-434">Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-434">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="56fa4-435">Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-435">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="56fa4-436">Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-436">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="56fa4-437">Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-437">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="56fa4-438">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-438">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="56fa4-439">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-439">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="56fa4-440">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-440">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="56fa4-441"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="56fa4-441">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="56fa4-442">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-442">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="56fa4-443"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="56fa4-443">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="56fa4-444">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-444">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="56fa4-445">[Yapılandırma anahtarları büyük/küçük harfe duyarsız](#keys)olduğundan, veritabanını başlatmak için kullanılan sözlük, büyük/küçük harf duyarsız karşılaştırıcı ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)) ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-445">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="56fa4-446">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-446">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="56fa4-447">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-447">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="56fa4-448">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-448">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="56fa4-449">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-449">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a><span data-ttu-id="56fa4-450">Başlangıçta erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-450">Access configuration in Startup</span></span>

<span data-ttu-id="56fa4-451">Aşağıdaki kod `Startup` yöntemlerde yapılandırma verilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-451">The following code displays configuration data in `Startup` methods:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

<span data-ttu-id="56fa4-452">Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. [uygulama başlatma: kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="56fa4-452">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-razor-pages"></a><span data-ttu-id="56fa4-453">Razor Pages 'de erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-453">Access configuration in Razor Pages</span></span>

<span data-ttu-id="56fa4-454">Aşağıdaki kod, bir Razor sayfasında yapılandırma verilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-454">The following code displays configuration data in a Razor Page:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a><span data-ttu-id="56fa4-455">MVC görünüm dosyasındaki erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-455">Access configuration in a MVC view file</span></span>

<span data-ttu-id="56fa4-456">Aşağıdaki kod, yapılandırma verilerini bir MVC görünümünde görüntüler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-456">The following code displays configuration data in a MVC view:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="56fa4-457">Uygulama yapılandırmasına karşı konak</span><span class="sxs-lookup"><span data-stu-id="56fa4-457">Host versus app configuration</span></span>

<span data-ttu-id="56fa4-458">Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-458">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="56fa4-459">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-459">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="56fa4-460">Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-460">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="56fa4-461">Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-461">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="56fa4-462">Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-462">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

<a name="dhc"></a>

## <a name="default-host-configuration"></a><span data-ttu-id="56fa4-463">Varsayılan konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-463">Default host configuration</span></span>

<span data-ttu-id="56fa4-464">[Web konağını](xref:fundamentals/host/web-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [bu konunun ASP.NET Core 2,2 sürümüne](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-464">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="56fa4-465">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-465">Host configuration is provided from:</span></span>
  * <span data-ttu-id="56fa4-466">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `DOTNET_` (örneğin, `DOTNET_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-466">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables configuration provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="56fa4-467">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`DOTNET_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-467">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="56fa4-468">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-468">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="56fa4-469">Web ana bilgisayar varsayılan yapılandırması oluşturuldu (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="56fa4-469">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="56fa4-470">Kestrel, Web sunucusu olarak kullanılır ve uygulamanın yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-470">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="56fa4-471">Konak filtreleme ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-471">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="56fa4-472">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` ortam değişkeni `true`olarak ayarlandıysa, Iletilen üstbilgiler ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-472">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="56fa4-473">IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-473">Enable IIS integration.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="56fa4-474">Diğer yapılandırma</span><span class="sxs-lookup"><span data-stu-id="56fa4-474">Other configuration</span></span>

<span data-ttu-id="56fa4-475">Bu konu yalnızca *uygulama yapılandırması*ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-475">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="56fa4-476">ASP.NET Core uygulamalarını çalıştırmanın diğer yönleri, bu konuda kapsanmayan yapılandırma dosyaları kullanılarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-476">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="56fa4-477">*Launch. json*/*launchsettings. JSON* , geliştirme ortamı için yapılandırma dosyalarını araçlar, açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-477">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="56fa4-478"><xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-478">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="56fa4-479">Dosyaların geliştirme senaryolarında ASP.NET Core Uygulamaları yapılandırmak için kullanıldığı belge kümesi boyunca.</span><span class="sxs-lookup"><span data-stu-id="56fa4-479">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="56fa4-480">*Web. config* aşağıdaki konularda açıklanan bir sunucu yapılandırma dosyasıdır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-480">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="56fa4-481">ASP.NET 'in önceki sürümlerinden uygulama yapılandırmasını geçirme hakkında daha fazla bilgi için bkz. <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-481">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="56fa4-482">Bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="56fa4-482">Add configuration from an external assembly</span></span>

<span data-ttu-id="56fa4-483"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-483">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="56fa4-484">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-484">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56fa4-485">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="56fa4-485">Additional resources</span></span>

* [<span data-ttu-id="56fa4-486">Yapılandırma kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="56fa4-486">Configuration source code</span></span>](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="56fa4-487">ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-487">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="56fa4-488">Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-488">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="56fa4-489">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="56fa4-489">Azure Key Vault</span></span>
* <span data-ttu-id="56fa4-490">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-490">Azure App Configuration</span></span>
* <span data-ttu-id="56fa4-491">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-491">Command-line arguments</span></span>
* <span data-ttu-id="56fa4-492">Özel sağlayıcılar (yüklü veya oluşturulmuş)</span><span class="sxs-lookup"><span data-stu-id="56fa4-492">Custom providers (installed or created)</span></span>
* <span data-ttu-id="56fa4-493">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="56fa4-493">Directory files</span></span>
* <span data-ttu-id="56fa4-494">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-494">Environment variables</span></span>
* <span data-ttu-id="56fa4-495">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-495">In-memory .NET objects</span></span>
* <span data-ttu-id="56fa4-496">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="56fa4-496">Settings files</span></span>

<span data-ttu-id="56fa4-497">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-497">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="56fa4-498">Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-498">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="56fa4-499">*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-499">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="56fa4-500">Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-500">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="56fa4-501">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-501">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="56fa4-502">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56fa4-502">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="56fa4-503">Uygulama yapılandırmasına karşı konak</span><span class="sxs-lookup"><span data-stu-id="56fa4-503">Host versus app configuration</span></span>

<span data-ttu-id="56fa4-504">Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-504">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="56fa4-505">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-505">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="56fa4-506">Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-506">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="56fa4-507">Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-507">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="56fa4-508">Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-508">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="56fa4-509">Diğer yapılandırma</span><span class="sxs-lookup"><span data-stu-id="56fa4-509">Other configuration</span></span>

<span data-ttu-id="56fa4-510">Bu konu yalnızca *uygulama yapılandırması*ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-510">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="56fa4-511">ASP.NET Core uygulamalarını çalıştırmanın diğer yönleri, bu konuda kapsanmayan yapılandırma dosyaları kullanılarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-511">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="56fa4-512">*Launch. json*/*launchsettings. JSON* , geliştirme ortamı için yapılandırma dosyalarını araçlar, açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-512">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="56fa4-513"><xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-513">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="56fa4-514">Dosyaların geliştirme senaryolarında ASP.NET Core Uygulamaları yapılandırmak için kullanıldığı belge kümesi boyunca.</span><span class="sxs-lookup"><span data-stu-id="56fa4-514">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="56fa4-515">*Web. config* aşağıdaki konularda açıklanan bir sunucu yapılandırma dosyasıdır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-515">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="56fa4-516">ASP.NET 'in önceki sürümlerinden uygulama yapılandırmasını geçirme hakkında daha fazla bilgi için bkz. <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-516">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="56fa4-517">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="56fa4-517">Default configuration</span></span>

<span data-ttu-id="56fa4-518">ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-518">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="56fa4-519">`CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="56fa4-519">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="56fa4-520">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-520">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="56fa4-521">[Genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [Bu konunun en son sürümüne](xref:fundamentals/configuration/index)bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-521">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="56fa4-522">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-522">Host configuration is provided from:</span></span>
  * <span data-ttu-id="56fa4-523">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-523">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="56fa4-524">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`ASPNETCORE_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-524">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="56fa4-525">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-525">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="56fa4-526">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-526">App configuration is provided from:</span></span>
  * <span data-ttu-id="56fa4-527">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="56fa4-527">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="56fa4-528">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="56fa4-528">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="56fa4-529">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-529">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="56fa4-530">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-530">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="56fa4-531">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-531">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="56fa4-532">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="56fa4-532">Security</span></span>

<span data-ttu-id="56fa4-533">Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="56fa4-533">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="56fa4-534">Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-534">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="56fa4-535">Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-535">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="56fa4-536">Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-536">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="56fa4-537">Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-537">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="56fa4-538"><xref:security/app-secrets> &ndash;, hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler Içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-538"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="56fa4-539">Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-539">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="56fa4-540">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-540">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="56fa4-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-541">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="56fa4-542">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-542">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="56fa4-543">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="56fa4-543">Hierarchical configuration data</span></span>

<span data-ttu-id="56fa4-544">Yapılandırma API 'SI, hiyerarşik verileri yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini muhafaza ediyor.</span><span class="sxs-lookup"><span data-stu-id="56fa4-544">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="56fa4-545">Aşağıdaki JSON dosyasında, iki bölümün yapılandırılmış hiyerarşisinde dört anahtar mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-545">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="56fa4-546">Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-546">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="56fa4-547">Bölüm ve anahtarlar, özgün yapıyı sürdürmek için iki nokta üst üste (`:`) kullanımıyla düzleştirilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-547">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="56fa4-548">section0:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-548">section0:key0</span></span>
* <span data-ttu-id="56fa4-549">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-549">section0:key1</span></span>
* <span data-ttu-id="56fa4-550">section1:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-550">section1:key0</span></span>
* <span data-ttu-id="56fa4-551">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-551">section1:key1</span></span>

<span data-ttu-id="56fa4-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-552"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="56fa4-553">Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-553">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="56fa4-554">Kurallar</span><span class="sxs-lookup"><span data-stu-id="56fa4-554">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="56fa4-555">Kaynaklar ve sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="56fa4-555">Sources and providers</span></span>

<span data-ttu-id="56fa4-556">Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="56fa4-556">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="56fa4-557">Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-557">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="56fa4-558">Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.</span><span class="sxs-lookup"><span data-stu-id="56fa4-558">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="56fa4-559"><xref:Microsoft.Extensions.Configuration.IConfiguration>, uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-559"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="56fa4-560"><xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfın yapılandırmasını elde etmek için bir Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> veya MVC <xref:Microsoft.AspNetCore.Mvc.Controller> eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-560"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="56fa4-561">Aşağıdaki örneklerde `_config` alanı yapılandırma değerlerine erişmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-561">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="56fa4-562">Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-562">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="56fa4-563">Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="56fa4-563">Keys</span></span>

<span data-ttu-id="56fa4-564">Yapılandırma anahtarları aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="56fa4-564">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="56fa4-565">Anahtarlar büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-565">Keys are case-insensitive.</span></span> <span data-ttu-id="56fa4-566">Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-566">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="56fa4-567">Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-567">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="56fa4-568">Hiyerarşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="56fa4-568">Hierarchical keys</span></span>
  * <span data-ttu-id="56fa4-569">Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-569">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="56fa4-570">Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-570">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="56fa4-571">Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-571">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="56fa4-572">Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-572">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="56fa4-573">Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tireleri bir iki nokta ile değiştirmek için kod yazın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-573">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="56fa4-574"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-574">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="56fa4-575">Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-575">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="56fa4-576">Değerler</span><span class="sxs-lookup"><span data-stu-id="56fa4-576">Values</span></span>

<span data-ttu-id="56fa4-577">Yapılandırma değerleri aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="56fa4-577">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="56fa4-578">Değerler dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-578">Values are strings.</span></span>
* <span data-ttu-id="56fa4-579">Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-579">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="56fa4-580">Sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="56fa4-580">Providers</span></span>

<span data-ttu-id="56fa4-581">Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-581">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="56fa4-582">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="56fa4-582">Provider</span></span> | <span data-ttu-id="56fa4-583">&hellip; yapılandırma sağlar</span><span class="sxs-lookup"><span data-stu-id="56fa4-583">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="56fa4-584">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="56fa4-584">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="56fa4-585">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="56fa4-585">Azure Key Vault</span></span> |
| <span data-ttu-id="56fa4-586">[Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri)</span><span class="sxs-lookup"><span data-stu-id="56fa4-586">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="56fa4-587">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-587">Azure App Configuration</span></span> |
| [<span data-ttu-id="56fa4-588">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-588">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="56fa4-589">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-589">Command-line parameters</span></span> |
| [<span data-ttu-id="56fa4-590">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-590">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="56fa4-591">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="56fa4-591">Custom source</span></span> |
| [<span data-ttu-id="56fa4-592">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-592">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="56fa4-593">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-593">Environment variables</span></span> |
| [<span data-ttu-id="56fa4-594">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-594">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="56fa4-595">Dosyalar (ıNı, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="56fa4-595">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="56fa4-596">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-596">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="56fa4-597">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="56fa4-597">Directory files</span></span> |
| [<span data-ttu-id="56fa4-598">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-598">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="56fa4-599">Bellek içi Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="56fa4-599">In-memory collections</span></span> |
| <span data-ttu-id="56fa4-600">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="56fa4-600">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="56fa4-601">Kullanıcı profili dizinindeki dosya</span><span class="sxs-lookup"><span data-stu-id="56fa4-601">File in the user profile directory</span></span> |

<span data-ttu-id="56fa4-602">Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="56fa4-602">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="56fa4-603">Bu konu başlığı altında açıklanan yapılandırma sağlayıcıları, kodun onları düzenler sırasına göre değil alfabetik sırayla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-603">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="56fa4-604">Koddaki yapılandırma sağlayıcılarını, uygulamanın gerektirdiği temel yapılandırma kaynakları için önceliklere uyacak şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-604">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="56fa4-605">Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-605">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="56fa4-606">Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-606">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="56fa4-607">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="56fa4-607">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="56fa4-608">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (yalnızca geliştirme ortamı)</span><span class="sxs-lookup"><span data-stu-id="56fa4-608">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="56fa4-609">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-609">Environment variables</span></span>
1. <span data-ttu-id="56fa4-610">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="56fa4-610">Command-line arguments</span></span>

<span data-ttu-id="56fa4-611">Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-611">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="56fa4-612">Yeni bir ana bilgisayar Oluşturucusu `CreateDefaultBuilder`ile başlatıldığında yukarıdaki sağlayıcılar dizisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-612">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="56fa4-613">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-613">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="56fa4-614">Konak oluşturucuyu UseConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="56fa4-614">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="56fa4-615">Konak oluşturucuyu yapılandırmak için, yapılandırma ile konak Oluşturucu üzerinde <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-615">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a><span data-ttu-id="56fa4-616">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="56fa4-616">ConfigureAppConfiguration</span></span>

<span data-ttu-id="56fa4-617">`CreateDefaultBuilder`tarafından otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirlemek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-617">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="56fa4-618">Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="56fa4-618">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="56fa4-619">Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için `AddCommandLine` son ' u çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-619">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="56fa4-620">CreateDefaultBuilder tarafından eklenen sağlayıcıları kaldır</span><span class="sxs-lookup"><span data-stu-id="56fa4-620">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="56fa4-621">`CreateDefaultBuilder`tarafından eklenen sağlayıcıları kaldırmak için, önce [ılisteationbuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) üzerinde [clear](/dotnet/api/system.collections.generic.icollection-1.clear) öğesini çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-621">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="56fa4-622">Uygulama başlatma sırasında yapılandırmayı kullan</span><span class="sxs-lookup"><span data-stu-id="56fa4-622">Consume configuration during app startup</span></span>

<span data-ttu-id="56fa4-623">`ConfigureAppConfiguration` içinde uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında `Startup.ConfigureServices`dahil olmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-623">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="56fa4-624">Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-624">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="56fa4-625">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-625">Command-line Configuration Provider</span></span>

<span data-ttu-id="56fa4-626"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, çalışma zamanında komut satırı bağımsız değişkeni anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-626">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="56fa4-627">Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> uzantısı yöntemi bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-627">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="56fa4-628">`AddCommandLine`, `CreateDefaultBuilder(string [])` çağrıldığında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-628">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="56fa4-629">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-629">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="56fa4-630">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-630">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="56fa4-631">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="56fa4-631">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="56fa4-632">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-632">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="56fa4-633">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-633">Environment variables.</span></span>

<span data-ttu-id="56fa4-634">`CreateDefaultBuilder`, komut satırı yapılandırma sağlayıcısını en son ekler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-634">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="56fa4-635">Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.</span><span class="sxs-lookup"><span data-stu-id="56fa4-635">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="56fa4-636">`CreateDefaultBuilder` ana bilgisayar oluşturulduğunda davranır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-636">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="56fa4-637">Bu nedenle, `CreateDefaultBuilder` tarafından etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-637">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="56fa4-638">ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` `CreateDefaultBuilder`tarafından zaten çağırılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-638">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="56fa4-639">Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için, `ConfigureAppConfiguration` 'de uygulamanın ek sağlayıcılarını çağırın ve `AddCommandLine` son ' u çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-639">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="56fa4-640">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="56fa4-640">**Example**</span></span>

<span data-ttu-id="56fa4-641">Örnek uygulama, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-641">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="56fa4-642">Projenin dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-642">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="56fa4-643">`dotnet run` komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="56fa4-643">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="56fa4-644">Uygulama çalıştıktan sonra, `http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-644">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="56fa4-645">Çıktının `dotnet run`için belirtilen yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-645">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="56fa4-646">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="56fa4-646">Arguments</span></span>

<span data-ttu-id="56fa4-647">Değer bir eşittir işareti (`=`) izlemelidir veya değer bir boşluk izleyen anahtarın bir ön eki (`--` veya `/`) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-647">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="56fa4-648">Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="56fa4-648">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="56fa4-649">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="56fa4-649">Key prefix</span></span>               | <span data-ttu-id="56fa4-650">Örnek</span><span class="sxs-lookup"><span data-stu-id="56fa4-650">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="56fa4-651">Ön ek yok</span><span class="sxs-lookup"><span data-stu-id="56fa4-651">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="56fa4-652">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="56fa4-652">Two dashes (`--`)</span></span>        | <span data-ttu-id="56fa4-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="56fa4-653">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="56fa4-654">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="56fa4-654">Forward slash (`/`)</span></span>      | <span data-ttu-id="56fa4-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="56fa4-655">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="56fa4-656">Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-656">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="56fa4-657">Örnek komutlar:</span><span class="sxs-lookup"><span data-stu-id="56fa4-657">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="56fa4-658">Eşleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="56fa4-658">Switch mappings</span></span>

<span data-ttu-id="56fa4-659">Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-659">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="56fa4-660">Bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>el ile yapılandırma oluştururken, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoduna geçiş değiştirme sözlüğü sağlar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-660">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="56fa4-661">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-661">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="56fa4-662">Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-662">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="56fa4-663">Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-663">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="56fa4-664">Anahtar eşlemeleri sözlük anahtarı kuralları:</span><span class="sxs-lookup"><span data-stu-id="56fa4-664">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="56fa4-665">Anahtarlar tireyle (`-`) veya çift tireyle başlamalıdır (`--`).</span><span class="sxs-lookup"><span data-stu-id="56fa4-665">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="56fa4-666">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-666">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="56fa4-667">Anahtar eşlemeleri sözlüğü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="56fa4-667">Create a switch mappings dictionary.</span></span> <span data-ttu-id="56fa4-668">Aşağıdaki örnekte, iki anahtar eşlemesi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-668">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="56fa4-669">Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne `AddCommandLine` çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-669">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="56fa4-670">Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-670">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="56fa4-671">`CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`'e iletmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-671">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="56fa4-672">Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="56fa4-672">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="56fa4-673">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra, aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-673">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="56fa4-674">Anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-674">Key</span></span>       | <span data-ttu-id="56fa4-675">Değer</span><span class="sxs-lookup"><span data-stu-id="56fa4-675">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="56fa4-676">Uygulama başlatılırken anahtar eşlenmiş anahtarlar kullanılıyorsa, yapılandırma sözlük tarafından sağlanan anahtardaki yapılandırma değerini alır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-676">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="56fa4-677">Önceki komutu çalıştırdıktan sonra, yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-677">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="56fa4-678">Anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-678">Key</span></span>               | <span data-ttu-id="56fa4-679">Değer</span><span class="sxs-lookup"><span data-stu-id="56fa4-679">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="56fa4-680">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-680">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="56fa4-681"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-681">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="56fa4-682">Ortam değişkenleri yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-682">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="56fa4-683">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceği ortam değişkenlerini ayarlamaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-683">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="56fa4-684">Daha fazla bilgi için bkz. [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="56fa4-684">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="56fa4-685">`AddEnvironmentVariables`, [Web ana](xref:fundamentals/host/web-host) bilgisayarıyla yeni bir ana bilgisayar Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `ASPNETCORE_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-685">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="56fa4-686">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-686">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="56fa4-687">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-687">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="56fa4-688">Önek olmadan `AddEnvironmentVariables` çağırarak, ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="56fa4-688">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="56fa4-689">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="56fa4-689">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="56fa4-690">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-690">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="56fa4-691">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-691">Command-line arguments.</span></span>

<span data-ttu-id="56fa4-692">Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-692">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="56fa4-693">Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-693">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="56fa4-694">Ek ortam değişkenlerinden uygulama yapılandırması sağlamak için, uygulamanın `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle birlikte `AddEnvironmentVariables` çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-694">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="56fa4-695">Diğer sağlayıcılardan gelen değerleri geçersiz kılmak için verilen öneke sahip ortam değişkenlerine izin vermek üzere en son `AddEnvironmentVariables` çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-695">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="56fa4-696">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="56fa4-696">**Example**</span></span>

<span data-ttu-id="56fa4-697">Örnek uygulama, `AddEnvironmentVariables`bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-697">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="56fa4-698">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-698">Run the sample app.</span></span> <span data-ttu-id="56fa4-699">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-699">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="56fa4-700">Çıktının `ENVIRONMENT`ortam değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-700">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="56fa4-701">Değer, uygulamanın çalıştığı ortamı yansıtır, genellikle yerel olarak çalışırken `Development`.</span><span class="sxs-lookup"><span data-stu-id="56fa4-701">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="56fa4-702">Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-702">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="56fa4-703">Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-703">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="56fa4-704">Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek için, *Pages/Index. cshtml. cs* ' deki `FilteredConfiguration` aşağıdaki gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="56fa4-704">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="56fa4-705">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="56fa4-705">Prefixes</span></span>

<span data-ttu-id="56fa4-706">Uygulamanın yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağlanırken filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-706">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="56fa4-707">Örneğin, önek `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-707">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="56fa4-708">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-708">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="56fa4-709">Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-709">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="56fa4-710">Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-710">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="56fa4-711">**Bağlantı dizesi önekleri**</span><span class="sxs-lookup"><span data-stu-id="56fa4-711">**Connection string prefixes**</span></span>

<span data-ttu-id="56fa4-712">Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-712">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="56fa4-713">`AddEnvironmentVariables`için bir önek sağlanmazsa, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-713">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="56fa4-714">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="56fa4-714">Connection string prefix</span></span> | <span data-ttu-id="56fa4-715">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="56fa4-715">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="56fa4-716">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="56fa4-716">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="56fa4-717">MySQL</span><span class="sxs-lookup"><span data-stu-id="56fa4-717">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="56fa4-718">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="56fa4-718">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="56fa4-719">SQL Server</span><span class="sxs-lookup"><span data-stu-id="56fa4-719">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="56fa4-720">Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:</span><span class="sxs-lookup"><span data-stu-id="56fa4-720">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="56fa4-721">Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-721">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="56fa4-722">Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).</span><span class="sxs-lookup"><span data-stu-id="56fa4-722">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="56fa4-723">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="56fa4-723">Environment variable key</span></span> | <span data-ttu-id="56fa4-724">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="56fa4-724">Converted configuration key</span></span> | <span data-ttu-id="56fa4-725">Sağlayıcı yapılandırma girişi</span><span class="sxs-lookup"><span data-stu-id="56fa4-725">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-726">Yapılandırma girişi oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="56fa4-726">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-727">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="56fa4-727">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="56fa4-728">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="56fa4-728">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-729">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="56fa4-729">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="56fa4-730">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="56fa4-730">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="56fa4-731">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="56fa4-731">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="56fa4-732">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="56fa4-732">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="56fa4-733">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="56fa4-733">**Example**</span></span>

<span data-ttu-id="56fa4-734">Sunucuda özel bir bağlantı dizesi ortam değişkeni oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-734">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="56fa4-735">Ad &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="56fa4-735">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="56fa4-736">Değer &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="56fa4-736">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="56fa4-737">`IConfiguration` eklenen ve `_config`adlı bir alana atanmışsa, şu değeri okuyun:</span><span class="sxs-lookup"><span data-stu-id="56fa4-737">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="56fa4-738">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-738">File Configuration Provider</span></span>

<span data-ttu-id="56fa4-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-739"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="56fa4-740">Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-740">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="56fa4-741">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-741">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="56fa4-742">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-742">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="56fa4-743">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-743">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="56fa4-744">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-744">INI Configuration Provider</span></span>

<span data-ttu-id="56fa4-745"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-745">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="56fa4-746">INI dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-746">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="56fa4-747">İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-747">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="56fa4-748">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="56fa4-748">Overloads permit specifying:</span></span>

* <span data-ttu-id="56fa4-749">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="56fa4-749">Whether the file is optional.</span></span>
* <span data-ttu-id="56fa4-750">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-750">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="56fa4-751">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-751">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="56fa4-752">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-752">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="56fa4-753">Bir ıNı yapılandırma dosyasına genel bir örnek:</span><span class="sxs-lookup"><span data-stu-id="56fa4-753">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="56fa4-754">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-754">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="56fa4-755">section0:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-755">section0:key0</span></span>
* <span data-ttu-id="56fa4-756">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-756">section0:key1</span></span>
* <span data-ttu-id="56fa4-757">Section1: alt bölüm: anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-757">section1:subsection:key</span></span>
* <span data-ttu-id="56fa4-758">section2: subsection0: anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-758">section2:subsection0:key</span></span>
* <span data-ttu-id="56fa4-759">section2: subsection1: anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-759">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="56fa4-760">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-760">JSON Configuration Provider</span></span>

<span data-ttu-id="56fa4-761"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-761">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="56fa4-762">JSON dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-762">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="56fa4-763">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="56fa4-763">Overloads permit specifying:</span></span>

* <span data-ttu-id="56fa4-764">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="56fa4-764">Whether the file is optional.</span></span>
* <span data-ttu-id="56fa4-765">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-765">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="56fa4-766">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-766">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="56fa4-767">`CreateDefaultBuilder`ile yeni bir ana bilgisayar Oluşturucu başlatıldığında `AddJsonFile` otomatik olarak iki kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-767">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="56fa4-768">Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-768">The method is called to load configuration from:</span></span>

* <span data-ttu-id="56fa4-769">*appSettings. json* &ndash; bu dosya ilk kez okundu.</span><span class="sxs-lookup"><span data-stu-id="56fa4-769">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="56fa4-770">Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-770">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="56fa4-771">*appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-771">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="56fa4-772">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-772">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="56fa4-773">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-773">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="56fa4-774">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-774">Environment variables.</span></span>
* <span data-ttu-id="56fa4-775">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="56fa4-775">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="56fa4-776">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="56fa4-776">Command-line arguments.</span></span>

<span data-ttu-id="56fa4-777">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-777">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="56fa4-778">Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-778">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="56fa4-779">Ana bilgisayarı derlerken, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtecek `ConfigureAppConfiguration` çağrısı yapın *. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-779">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="56fa4-780">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="56fa4-780">**Example**</span></span>

<span data-ttu-id="56fa4-781">Örnek uygulama, `AddJsonFile`iki çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-781">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="56fa4-782">`AddJsonFile` ilk çağrısı *appSettings. JSON*' dan yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-782">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="56fa4-783">`AddJsonFile` ikinci çağrısı, appSettings 'ten yapılandırma yükler *. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="56fa4-783">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="56fa4-784">*AppSettings için. Geliştirme. JSON* örnek uygulamada aşağıdaki dosya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-784">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="56fa4-785">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-785">Run the sample app.</span></span> <span data-ttu-id="56fa4-786">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-786">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="56fa4-787">Çıktı, uygulamanın ortamına göre yapılandırma için anahtar-değer çiftleri içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-787">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="56fa4-788">Uygulama geliştirme ortamında çalıştırılırken anahtar `Logging:LogLevel:Default` günlük düzeyi `Debug`.</span><span class="sxs-lookup"><span data-stu-id="56fa4-788">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="56fa4-789">Örnek uygulamayı üretim ortamında yeniden çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-789">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="56fa4-790">*Properties/launchSettings. JSON* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-790">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="56fa4-791">`ConfigurationSample` profilinde, `ASPNETCORE_ENVIRONMENT` ortam değişkeninin değerini `Production`olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-791">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="56fa4-792">Dosyayı kaydedin ve bir komut kabuğunda `dotnet run` uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-792">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="56fa4-793">AppSettings içindeki ayarlar *. Development. JSON* artık *appSettings. JSON*içindeki ayarları geçersiz kılmaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-793">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="56fa4-794">Anahtar `Logging:LogLevel:Default` için günlük düzeyi `Warning`.</span><span class="sxs-lookup"><span data-stu-id="56fa4-794">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="56fa4-795">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-795">XML Configuration Provider</span></span>

<span data-ttu-id="56fa4-796"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-796">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="56fa4-797">XML dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-797">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="56fa4-798">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="56fa4-798">Overloads permit specifying:</span></span>

* <span data-ttu-id="56fa4-799">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="56fa4-799">Whether the file is optional.</span></span>
* <span data-ttu-id="56fa4-800">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-800">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="56fa4-801">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-801">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="56fa4-802">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-802">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="56fa4-803">Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-803">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="56fa4-804">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-804">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="56fa4-805">XML yapılandırma dosyaları, yinelenen bölümler için farklı öğe adları kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-805">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="56fa4-806">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-806">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="56fa4-807">section0:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-807">section0:key0</span></span>
* <span data-ttu-id="56fa4-808">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-808">section0:key1</span></span>
* <span data-ttu-id="56fa4-809">section1:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-809">section1:key0</span></span>
* <span data-ttu-id="56fa4-810">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-810">section1:key1</span></span>

<span data-ttu-id="56fa4-811">Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-811">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="56fa4-812">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-812">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="56fa4-813">Bölüm: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-813">section:section0:key:key0</span></span>
* <span data-ttu-id="56fa4-814">Bölüm: section0: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-814">section:section0:key:key1</span></span>
* <span data-ttu-id="56fa4-815">Bölüm: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-815">section:section1:key:key0</span></span>
* <span data-ttu-id="56fa4-816">Bölüm: Section1: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-816">section:section1:key:key1</span></span>

<span data-ttu-id="56fa4-817">Öznitelikler, değerler sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-817">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="56fa4-818">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="56fa4-818">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="56fa4-819">anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="56fa4-819">key:attribute</span></span>
* <span data-ttu-id="56fa4-820">Section: Key: özniteliği</span><span class="sxs-lookup"><span data-stu-id="56fa4-820">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="56fa4-821">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-821">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="56fa4-822"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-822">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="56fa4-823">Anahtar, dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-823">The key is the file name.</span></span> <span data-ttu-id="56fa4-824">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-824">The value contains the file's contents.</span></span> <span data-ttu-id="56fa4-825">Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-825">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="56fa4-826">Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-826">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="56fa4-827">Dosyaların `directoryPath` mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-827">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="56fa4-828">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="56fa4-828">Overloads permit specifying:</span></span>

* <span data-ttu-id="56fa4-829">Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="56fa4-829">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="56fa4-830">Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-830">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="56fa4-831">Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-831">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="56fa4-832">Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-832">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="56fa4-833">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-833">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="56fa4-834">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-834">Memory Configuration Provider</span></span>

<span data-ttu-id="56fa4-835"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-835">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="56fa4-836">Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-836">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="56fa4-837">Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>`başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-837">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="56fa4-838">Uygulamanın yapılandırmasını belirtmek için Konağı derlerken `ConfigureAppConfiguration` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-838">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="56fa4-839">Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-839">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="56fa4-840">Sözlük, yapılandırmayı sağlamak için bir `AddInMemoryCollection` çağrısıyla kullanılır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-840">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="56fa4-841">GetValue</span><span class="sxs-lookup"><span data-stu-id="56fa4-841">GetValue</span></span>

<span data-ttu-id="56fa4-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen koleksiyon olmayan türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-842">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="56fa4-843">Aşırı yükleme varsayılan bir değeri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="56fa4-843">An overload accepts a default value.</span></span>

<span data-ttu-id="56fa4-844">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="56fa4-844">The following example:</span></span>

* <span data-ttu-id="56fa4-845">Anahtar `NumberKey`, yapılandırmadan dize değerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-845">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="56fa4-846">Yapılandırma anahtarlarında `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-846">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="56fa4-847">Değeri bir `int`olarak türler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-847">Types the value as an `int`.</span></span>
* <span data-ttu-id="56fa4-848">Değeri, sayfanın kullanımı için `NumberConfig` özelliği içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-848">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="56fa4-849">GetSection, GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="56fa4-849">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="56fa4-850">İzleyen örnekler için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="56fa4-850">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="56fa4-851">İki bölüm arasında dört anahtar bulunur ve bunlardan biri alt bölümleri çifti içerir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-851">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="56fa4-852">Dosya yapılandırmaya okunduğu zaman yapılandırma değerlerini tutmak için aşağıdaki benzersiz hiyerarşik anahtarlar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-852">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="56fa4-853">section0:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-853">section0:key0</span></span>
* <span data-ttu-id="56fa4-854">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-854">section0:key1</span></span>
* <span data-ttu-id="56fa4-855">section1:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-855">section1:key0</span></span>
* <span data-ttu-id="56fa4-856">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-856">section1:key1</span></span>
* <span data-ttu-id="56fa4-857">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-857">section2:subsection0:key0</span></span>
* <span data-ttu-id="56fa4-858">section2: subsection0: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-858">section2:subsection0:key1</span></span>
* <span data-ttu-id="56fa4-859">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="56fa4-859">section2:subsection1:key0</span></span>
* <span data-ttu-id="56fa4-860">section2: subsection1: KEY1</span><span class="sxs-lookup"><span data-stu-id="56fa4-860">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="56fa4-861">GetSection</span><span class="sxs-lookup"><span data-stu-id="56fa4-861">GetSection</span></span>

<span data-ttu-id="56fa4-862">[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü ayıklar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-862">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="56fa4-863">`section1`yalnızca anahtar-değer çiftlerini içeren bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> döndürmek için `GetSection` çağırın ve bölüm adını sağlayın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-863">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="56fa4-864">`configSection` bir değer, yalnızca bir anahtar ve yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-864">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="56fa4-865">Benzer şekilde, `section2:subsection0`anahtarlar için değerleri almak için, `GetSection` çağırın ve Bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-865">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="56fa4-866">`GetSection` hiçbir şekilde `null`döndürmez.</span><span class="sxs-lookup"><span data-stu-id="56fa4-866">`GetSection` never returns `null`.</span></span> <span data-ttu-id="56fa4-867">Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-867">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="56fa4-868">`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor.</span><span class="sxs-lookup"><span data-stu-id="56fa4-868">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="56fa4-869">Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-869">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="56fa4-870">GetChildren</span><span class="sxs-lookup"><span data-stu-id="56fa4-870">GetChildren</span></span>

<span data-ttu-id="56fa4-871">`section2` üzerinde [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı, şunları içeren bir `IEnumerable<IConfigurationSection>` edinir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-871">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="56fa4-872">Var</span><span class="sxs-lookup"><span data-stu-id="56fa4-872">Exists</span></span>

<span data-ttu-id="56fa4-873">Bir yapılandırma bölümünün mevcut olup olmadığını anlamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) kullanın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-873">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="56fa4-874">Örnek veriler verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölümü olmadığından `sectionExists` `false`.</span><span class="sxs-lookup"><span data-stu-id="56fa4-874">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="56fa4-875">Bir nesne grafiğine bağlama</span><span class="sxs-lookup"><span data-stu-id="56fa4-875">Bind to an object graph</span></span>

<span data-ttu-id="56fa4-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, tüm POCO nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-876"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="56fa4-877">Basit bir nesne bağlamakla birlikte yalnızca genel okuma/yazma özellikleri bağlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-877">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="56fa4-878">Örnek, nesne grafı `Metadata` ve `Actors` sınıfları (*modeller/TvShow. cs*) içeren bir `TvShow` modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-878">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="56fa4-879">Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-879">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="56fa4-880">Yapılandırma, `Bind` yöntemi ile `TvShow` nesne grafiğinin tamamına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-880">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="56fa4-881">Bağlantılı örnek, işleme için bir özelliğe atandı:</span><span class="sxs-lookup"><span data-stu-id="56fa4-881">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="56fa4-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlanır ve belirtilen türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="56fa4-882">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="56fa4-883">`Get<T>`, `Bind`kullanmaktan daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-883">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="56fa4-884">Aşağıdaki kod, `Get<T>` önceki örnekle nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-884">The following code shows how to use `Get<T>` with the preceding example:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="56fa4-885">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="56fa4-885">Bind an array to a class</span></span>

<span data-ttu-id="56fa4-886">*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*</span><span class="sxs-lookup"><span data-stu-id="56fa4-886">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="56fa4-887"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="56fa4-887">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="56fa4-888">Sayısal anahtar segmentini (`:0:`, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir POCO sınıf dizisine dizi bağlama özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-888">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="56fa4-889">Bağlama, kural tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-889">Binding is provided by convention.</span></span> <span data-ttu-id="56fa4-890">Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-890">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="56fa4-891">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="56fa4-891">**In-memory array processing**</span></span>

<span data-ttu-id="56fa4-892">Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="56fa4-892">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="56fa4-893">Anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-893">Key</span></span>             | <span data-ttu-id="56fa4-894">Değer</span><span class="sxs-lookup"><span data-stu-id="56fa4-894">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="56fa4-895">dizi: girdiler: 0</span><span class="sxs-lookup"><span data-stu-id="56fa4-895">array:entries:0</span></span> | <span data-ttu-id="56fa4-896">value0</span><span class="sxs-lookup"><span data-stu-id="56fa4-896">value0</span></span> |
| <span data-ttu-id="56fa4-897">dizi: girdiler: 1</span><span class="sxs-lookup"><span data-stu-id="56fa4-897">array:entries:1</span></span> | <span data-ttu-id="56fa4-898">Value1</span><span class="sxs-lookup"><span data-stu-id="56fa4-898">value1</span></span> |
| <span data-ttu-id="56fa4-899">dizi: girdiler: 2</span><span class="sxs-lookup"><span data-stu-id="56fa4-899">array:entries:2</span></span> | <span data-ttu-id="56fa4-900">Value2</span><span class="sxs-lookup"><span data-stu-id="56fa4-900">value2</span></span> |
| <span data-ttu-id="56fa4-901">dizi: girdiler: 4</span><span class="sxs-lookup"><span data-stu-id="56fa4-901">array:entries:4</span></span> | <span data-ttu-id="56fa4-902">value4</span><span class="sxs-lookup"><span data-stu-id="56fa4-902">value4</span></span> |
| <span data-ttu-id="56fa4-903">dizi: girdiler: 5</span><span class="sxs-lookup"><span data-stu-id="56fa4-903">array:entries:5</span></span> | <span data-ttu-id="56fa4-904">value5</span><span class="sxs-lookup"><span data-stu-id="56fa4-904">value5</span></span> |

<span data-ttu-id="56fa4-905">Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-905">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="56fa4-906">Dizi, Dizin &num;3 için bir değer atlıyor.</span><span class="sxs-lookup"><span data-stu-id="56fa4-906">The array skips a value for index &num;3.</span></span> <span data-ttu-id="56fa4-907">Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-907">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="56fa4-908">Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-908">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="56fa4-909">Yapılandırma verileri nesnesine bağlanır:</span><span class="sxs-lookup"><span data-stu-id="56fa4-909">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="56fa4-910">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-910">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="56fa4-911">`ArrayExample`bir örneği olan bağlantılı nesne, yapılandırmadan dizi verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-911">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="56fa4-912">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="56fa4-912">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="56fa4-913">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="56fa4-913">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="56fa4-914">0</span><span class="sxs-lookup"><span data-stu-id="56fa4-914">0</span></span>                            | <span data-ttu-id="56fa4-915">value0</span><span class="sxs-lookup"><span data-stu-id="56fa4-915">value0</span></span>                       |
| <span data-ttu-id="56fa4-916">1</span><span class="sxs-lookup"><span data-stu-id="56fa4-916">1</span></span>                            | <span data-ttu-id="56fa4-917">Value1</span><span class="sxs-lookup"><span data-stu-id="56fa4-917">value1</span></span>                       |
| <span data-ttu-id="56fa4-918">2</span><span class="sxs-lookup"><span data-stu-id="56fa4-918">2</span></span>                            | <span data-ttu-id="56fa4-919">Value2</span><span class="sxs-lookup"><span data-stu-id="56fa4-919">value2</span></span>                       |
| <span data-ttu-id="56fa4-920">3</span><span class="sxs-lookup"><span data-stu-id="56fa4-920">3</span></span>                            | <span data-ttu-id="56fa4-921">value4</span><span class="sxs-lookup"><span data-stu-id="56fa4-921">value4</span></span>                       |
| <span data-ttu-id="56fa4-922">4</span><span class="sxs-lookup"><span data-stu-id="56fa4-922">4</span></span>                            | <span data-ttu-id="56fa4-923">value5</span><span class="sxs-lookup"><span data-stu-id="56fa4-923">value5</span></span>                       |

<span data-ttu-id="56fa4-924">İlişkili nesnede &num;3 dizini, `array:4` yapılandırma anahtarı ve `value4`değeri için yapılandırma verilerini tutar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-924">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="56fa4-925">Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-925">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="56fa4-926">Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-926">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="56fa4-927">Yapılandırmada doğru anahtar-değer çiftini üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce, &num;3 dizini için eksik yapılandırma öğesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-927">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="56fa4-928">Örnek, eksik anahtar-değer çiftine sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, `ArrayExample.Entries` tam yapılandırma dizisiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-928">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="56fa4-929">*missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-929">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="56fa4-930">`ConfigureAppConfiguration` içinde:</span><span class="sxs-lookup"><span data-stu-id="56fa4-930">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="56fa4-931">Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.</span><span class="sxs-lookup"><span data-stu-id="56fa4-931">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="56fa4-932">Anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-932">Key</span></span>             | <span data-ttu-id="56fa4-933">Değer</span><span class="sxs-lookup"><span data-stu-id="56fa4-933">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="56fa4-934">dizi: girdiler: 3</span><span class="sxs-lookup"><span data-stu-id="56fa4-934">array:entries:3</span></span> | <span data-ttu-id="56fa4-935">value3</span><span class="sxs-lookup"><span data-stu-id="56fa4-935">value3</span></span> |

<span data-ttu-id="56fa4-936">`ArrayExample` sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ün girdisini içeriyorsa, `ArrayExample.Entries` dizisi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-936">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="56fa4-937">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="56fa4-937">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="56fa4-938">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="56fa4-938">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="56fa4-939">0</span><span class="sxs-lookup"><span data-stu-id="56fa4-939">0</span></span>                            | <span data-ttu-id="56fa4-940">value0</span><span class="sxs-lookup"><span data-stu-id="56fa4-940">value0</span></span>                       |
| <span data-ttu-id="56fa4-941">1</span><span class="sxs-lookup"><span data-stu-id="56fa4-941">1</span></span>                            | <span data-ttu-id="56fa4-942">Value1</span><span class="sxs-lookup"><span data-stu-id="56fa4-942">value1</span></span>                       |
| <span data-ttu-id="56fa4-943">2</span><span class="sxs-lookup"><span data-stu-id="56fa4-943">2</span></span>                            | <span data-ttu-id="56fa4-944">Value2</span><span class="sxs-lookup"><span data-stu-id="56fa4-944">value2</span></span>                       |
| <span data-ttu-id="56fa4-945">3</span><span class="sxs-lookup"><span data-stu-id="56fa4-945">3</span></span>                            | <span data-ttu-id="56fa4-946">value3</span><span class="sxs-lookup"><span data-stu-id="56fa4-946">value3</span></span>                       |
| <span data-ttu-id="56fa4-947">4</span><span class="sxs-lookup"><span data-stu-id="56fa4-947">4</span></span>                            | <span data-ttu-id="56fa4-948">value4</span><span class="sxs-lookup"><span data-stu-id="56fa4-948">value4</span></span>                       |
| <span data-ttu-id="56fa4-949">5</span><span class="sxs-lookup"><span data-stu-id="56fa4-949">5</span></span>                            | <span data-ttu-id="56fa4-950">value5</span><span class="sxs-lookup"><span data-stu-id="56fa4-950">value5</span></span>                       |

<span data-ttu-id="56fa4-951">**JSON dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="56fa4-951">**JSON array processing**</span></span>

<span data-ttu-id="56fa4-952">JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-952">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="56fa4-953">Aşağıdaki yapılandırma dosyasında, `subsection` bir dizidir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-953">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="56fa4-954">JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="56fa4-954">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="56fa4-955">Anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-955">Key</span></span>                     | <span data-ttu-id="56fa4-956">Değer</span><span class="sxs-lookup"><span data-stu-id="56fa4-956">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="56fa4-957">json_array: anahtar</span><span class="sxs-lookup"><span data-stu-id="56fa4-957">json_array:key</span></span>          | <span data-ttu-id="56fa4-958">değer EA</span><span class="sxs-lookup"><span data-stu-id="56fa4-958">valueA</span></span> |
| <span data-ttu-id="56fa4-959">json_array: alt bölüm: 0</span><span class="sxs-lookup"><span data-stu-id="56fa4-959">json_array:subsection:0</span></span> | <span data-ttu-id="56fa4-960">valueB</span><span class="sxs-lookup"><span data-stu-id="56fa4-960">valueB</span></span> |
| <span data-ttu-id="56fa4-961">json_array: alt bölüm: 1</span><span class="sxs-lookup"><span data-stu-id="56fa4-961">json_array:subsection:1</span></span> | <span data-ttu-id="56fa4-962">değer EC</span><span class="sxs-lookup"><span data-stu-id="56fa4-962">valueC</span></span> |
| <span data-ttu-id="56fa4-963">json_array: alt bölüm: 2</span><span class="sxs-lookup"><span data-stu-id="56fa4-963">json_array:subsection:2</span></span> | <span data-ttu-id="56fa4-964">Değerler</span><span class="sxs-lookup"><span data-stu-id="56fa4-964">valueD</span></span> |

<span data-ttu-id="56fa4-965">Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-965">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="56fa4-966">Bağlamadan sonra, `JsonArrayExample.Key` `valueA`değerini tutar.</span><span class="sxs-lookup"><span data-stu-id="56fa4-966">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="56fa4-967">Alt bölüm değerleri, `Subsection`POCO dizisi özelliğinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-967">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="56fa4-968">`JsonArrayExample.Subsection` dizini</span><span class="sxs-lookup"><span data-stu-id="56fa4-968">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="56fa4-969">`JsonArrayExample.Subsection` değeri</span><span class="sxs-lookup"><span data-stu-id="56fa4-969">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="56fa4-970">0</span><span class="sxs-lookup"><span data-stu-id="56fa4-970">0</span></span>                                   | <span data-ttu-id="56fa4-971">valueB</span><span class="sxs-lookup"><span data-stu-id="56fa4-971">valueB</span></span>                              |
| <span data-ttu-id="56fa4-972">1</span><span class="sxs-lookup"><span data-stu-id="56fa4-972">1</span></span>                                   | <span data-ttu-id="56fa4-973">değer EC</span><span class="sxs-lookup"><span data-stu-id="56fa4-973">valueC</span></span>                              |
| <span data-ttu-id="56fa4-974">2</span><span class="sxs-lookup"><span data-stu-id="56fa4-974">2</span></span>                                   | <span data-ttu-id="56fa4-975">Değerler</span><span class="sxs-lookup"><span data-stu-id="56fa4-975">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="56fa4-976">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="56fa4-976">Custom configuration provider</span></span>

<span data-ttu-id="56fa4-977">Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-977">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="56fa4-978">Sağlayıcı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-978">The provider has the following characteristics:</span></span>

* <span data-ttu-id="56fa4-979">EF bellek içi veritabanı, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-979">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="56fa4-980">Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-980">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="56fa4-981">Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur.</span><span class="sxs-lookup"><span data-stu-id="56fa4-981">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="56fa4-982">Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.</span><span class="sxs-lookup"><span data-stu-id="56fa4-982">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="56fa4-983">Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-983">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="56fa4-984">Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="56fa4-984">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="56fa4-985">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-985">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="56fa4-986">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-986">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="56fa4-987">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-987">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="56fa4-988"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="56fa4-988">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="56fa4-989">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-989">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="56fa4-990"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="56fa4-990">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="56fa4-991">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="56fa4-991">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="56fa4-992">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-992">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="56fa4-993">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-993">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="56fa4-994">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="56fa4-994">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="56fa4-995">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="56fa4-995">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="56fa4-996">Başlangıç sırasında yapılandırmaya erişim</span><span class="sxs-lookup"><span data-stu-id="56fa4-996">Access configuration during startup</span></span>

<span data-ttu-id="56fa4-997">`Startup.ConfigureServices`yapılandırma değerlerine erişmek için `Startup` oluşturucusuna `IConfiguration` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-997">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="56fa4-998">`Startup.Configure`yapılandırmaya erişmek için `IConfiguration` doğrudan yönteme ekleyin ya da oluşturucuyu kullanarak örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="56fa4-998">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="56fa4-999">Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. [uygulama başlatma: kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="56fa4-999">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="56fa4-1000">Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="56fa4-1000">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="56fa4-1001">Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfa ya da görünüme <xref:Microsoft.Extensions.Configuration.IConfiguration> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fa4-1001">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="56fa4-1002">Razor Pages sayfasında:</span><span class="sxs-lookup"><span data-stu-id="56fa4-1002">In a Razor Pages page:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="56fa4-1003">MVC görünümünde:</span><span class="sxs-lookup"><span data-stu-id="56fa4-1003">In an MVC view:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="56fa4-1004">Bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="56fa4-1004">Add configuration from an external assembly</span></span>

<span data-ttu-id="56fa4-1005"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="56fa4-1005">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="56fa4-1006">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="56fa4-1006">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56fa4-1007">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="56fa4-1007">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
