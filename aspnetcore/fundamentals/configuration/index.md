---
title: ASP.NET core'da yapılandırma
author: guardrex
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 6dd478770d4eae4d497da576c17fbe7d2c133b89
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021748"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="6c1aa-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6c1aa-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="6c1aa-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6c1aa-105">ASP.NET core'da uygulama yapılandırması tarafından kurulan anahtar-değer çiftleri temel *yapılandırma sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="6c1aa-106">Yapılandırma sağlayıcıları, yapılandırma kaynaklarını çeşitli anahtar-değer çiftlerine yapılandırma verilerini okuyun:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="6c1aa-107">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="6c1aa-107">Azure Key Vault</span></span>
* <span data-ttu-id="6c1aa-108">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-108">Command-line arguments</span></span>
* <span data-ttu-id="6c1aa-109">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="6c1aa-110">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-110">Directory files</span></span>
* <span data-ttu-id="6c1aa-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-111">Environment variables</span></span>
* <span data-ttu-id="6c1aa-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-112">In-memory .NET objects</span></span>
* <span data-ttu-id="6c1aa-113">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="6c1aa-114">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="6c1aa-114">Azure Key Vault</span></span>
* <span data-ttu-id="6c1aa-115">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-115">Command-line arguments</span></span>
* <span data-ttu-id="6c1aa-116">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="6c1aa-117">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-117">Environment variables</span></span>
* <span data-ttu-id="6c1aa-118">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-118">In-memory .NET objects</span></span>
* <span data-ttu-id="6c1aa-119">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="6c1aa-120">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-120">Command-line arguments</span></span>
* <span data-ttu-id="6c1aa-121">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="6c1aa-122">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-122">Environment variables</span></span>
* <span data-ttu-id="6c1aa-123">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-123">In-memory .NET objects</span></span>
* <span data-ttu-id="6c1aa-124">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="6c1aa-125">*Seçenekleri deseni* bu konuda açıklanan yapılandırma kavramları bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="6c1aa-126">Seçenekler, ilgili ayar gruplarını temsil etmek için sınıflar kullanır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="6c1aa-127">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="6c1aa-128">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c1aa-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6c1aa-129">Bu konudaki sağladığınız örnekleri temel kullanır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="6c1aa-130">Temel yol ile uygulama ayarı <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6c1aa-131">`SetBasePath` başvurarak bir uygulama için kullanılabilir hale getirileceğini [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="6c1aa-132">Yapılandırma dosyalarıyla bölümlerini çözümleme <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="6c1aa-133">`GetSection` başvurarak bir uygulama için kullanılabilir hale getirileceğini [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="6c1aa-134">.NET için bağlama yapılandırması sınıfları ile <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> ve [alma&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="6c1aa-135">`Bind` ve `Get<T>` başvurarak bir uygulama için kullanılabilir yapılır [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="6c1aa-136">`Get<T>` ASP.NET Core 1.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-137">Bu üç paketi içinde yer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c1aa-138">Bu üç paketi içinde yer [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="6c1aa-139">Uygulama yapılandırması barındırın</span><span class="sxs-lookup"><span data-stu-id="6c1aa-139">Host vs. app configuration</span></span>

<span data-ttu-id="6c1aa-140">Uygulama yapılandırılmış ve başlatıldı, önce bir *konak* başlatılan ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="6c1aa-141">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="6c1aa-142">Bu konuda açıklanan yapılandırma sağlayıcıları kullanarak, hem uygulama hem de konak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="6c1aa-143">Ana bilgisayar yapılandırma anahtar-değer çiftleri uygulamanın genel yapılandırmasının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="6c1aa-144">Yapılandırma sağlayıcıları konak oluşturulduğunda kullanılan yapılandırma ve yapılandırma kaynaklarını nasıl etkileyeceğini nasıl barındırmak daha fazla bilgi için bkz: <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="6c1aa-145">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="6c1aa-145">Security</span></span>

<span data-ttu-id="6c1aa-146">Aşağıdaki en iyi benimseme:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="6c1aa-147">Hiçbir zaman parolaları ve diğer hassas verileri yapılandırma sağlayıcısı kodda veya düz metin yapılandırma dosyalarında depolayın.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="6c1aa-148">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="6c1aa-149">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="6c1aa-150">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [gizli dizi Yöneticisi ile geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) (depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. hassas verileri).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="6c1aa-151">Gizli dizi Yöneticisi'ni dosya yapılandırma sağlayıcısı bir JSON dosyası yerel sistemdeki kullanıcı gizli dizileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="6c1aa-152">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="6c1aa-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) uygulama gizli anahtarlarının güvenli bir şekilde depolanması için bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="6c1aa-154">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="6c1aa-155">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-155">Hierarchical configuration data</span></span>

<span data-ttu-id="6c1aa-156">Yapılandırma API configuration anahtarlarında bir sınırlayıcı kullanarak hiyerarşik veri düzleştirme tarafından hiyerarşik yapılandırma verileri koruma özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="6c1aa-157">Aşağıdaki JSON dosyasında iki bölüm yapılandırılmış bir hiyerarşide dört anahtarları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="6c1aa-158">Dosya yapılandırma okuduğunuzda benzersiz anahtarlar özgün hiyerarşik veri yapısını yapılandırma kaynağı korumak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="6c1aa-159">Bölümler ve anahtarlar ile bir iki nokta üst üste kullanımını düzleştirilir (`:`) özgün yapıyı korumak için:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="6c1aa-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-160">section0:key0</span></span>
* <span data-ttu-id="6c1aa-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-161">section0:key1</span></span>
* <span data-ttu-id="6c1aa-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-162">section1:key0</span></span>
* <span data-ttu-id="6c1aa-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-163">section1:key1</span></span>

<span data-ttu-id="6c1aa-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemlerdir bölümler ve yapılandırma verilerini bir bölümde alt yalıtmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="6c1aa-165">Bu yöntem daha sonra açıklanmıştır [GetSection GetChildren ve Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="6c1aa-166">Kurallar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-166">Conventions</span></span>

<span data-ttu-id="6c1aa-167">Uygulama başlangıcında, yapılandırma kaynaklarını yapılandırma sağlayıcıları belirttiğiniz sırayla okunur.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="6c1aa-168">Dosya yapılandırma sağlayıcıları uygulama başlangıcından sonra bir temel alınan ayarları dosyası değiştirildiğinde yapılandırmayı yeniden yükle seçeneğine sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="6c1aa-169">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="6c1aa-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> uygulamanın kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6c1aa-171">Bunlar ana bilgisayar tarafından kurduktan olduğunda değil olarak yapılandırma sağlayıcıları DI, faydalanamaz.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="6c1aa-172">Yapılandırma anahtarları, aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="6c1aa-173">Anahtarlar büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-173">Keys are case-insensitive.</span></span> <span data-ttu-id="6c1aa-174">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="6c1aa-175">Aynı veya farklı yapılandırma sağlayıcıları tarafından aynı anahtar için bir değer ayarlarsanız, anahtarda ayarlanan son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="6c1aa-176">Hiyerarşik anahtarları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-176">Hierarchical keys</span></span>
  * <span data-ttu-id="6c1aa-177">Yapılandırma API'sinin, iki nokta üst üste ayırıcı (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="6c1aa-178">Ortam değişkenleri, iki nokta üst üste ayırıcı tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="6c1aa-179">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve bir iki nokta üst üste dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="6c1aa-180">Azure anahtar Kasası'nda hiyerarşik tuşlarını `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="6c1aa-181">Gizli dizileri uygulama yapılandırma yüklendiğinde tireler iki nokta üst üste ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="6c1aa-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="6c1aa-183">Dizi bağlama açıklanan [bir dizi bir sınıfa Bağla](#bind-an-array-to-a-class) bölümü.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="6c1aa-184">Yapılandırma değerleri aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="6c1aa-185">Dizeleri değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-185">Values are strings.</span></span>
* <span data-ttu-id="6c1aa-186">Null değerler yapılandırmasında depolanmış veya nesnelere bağlı olamaz.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="6c1aa-187">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-187">Providers</span></span>

<span data-ttu-id="6c1aa-188">Aşağıdaki tabloda, ASP.NET Core uygulamaları için kullanılabilir yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="6c1aa-189">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-189">Provider</span></span> | <span data-ttu-id="6c1aa-190">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="6c1aa-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="6c1aa-191">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="6c1aa-192">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="6c1aa-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="6c1aa-193">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="6c1aa-194">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-194">Command-line parameters</span></span> |
| [<span data-ttu-id="6c1aa-195">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="6c1aa-196">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="6c1aa-196">Custom source</span></span> |
| [<span data-ttu-id="6c1aa-197">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="6c1aa-198">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-198">Environment variables</span></span> |
| [<span data-ttu-id="6c1aa-199">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="6c1aa-200">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="6c1aa-201">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="6c1aa-202">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-202">Directory files</span></span> |
| [<span data-ttu-id="6c1aa-203">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="6c1aa-204">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-204">In-memory collections</span></span> |
| <span data-ttu-id="6c1aa-205">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="6c1aa-206">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="6c1aa-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="6c1aa-207">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-207">Provider</span></span> | <span data-ttu-id="6c1aa-208">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="6c1aa-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="6c1aa-209">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="6c1aa-210">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="6c1aa-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="6c1aa-211">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="6c1aa-212">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-212">Command-line parameters</span></span> |
| [<span data-ttu-id="6c1aa-213">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="6c1aa-214">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="6c1aa-214">Custom source</span></span> |
| [<span data-ttu-id="6c1aa-215">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="6c1aa-216">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-216">Environment variables</span></span> |
| [<span data-ttu-id="6c1aa-217">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="6c1aa-218">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="6c1aa-219">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="6c1aa-220">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-220">In-memory collections</span></span> |
| <span data-ttu-id="6c1aa-221">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="6c1aa-222">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="6c1aa-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="6c1aa-223">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-223">Provider</span></span> | <span data-ttu-id="6c1aa-224">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="6c1aa-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="6c1aa-225">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="6c1aa-226">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-226">Command-line parameters</span></span> |
| [<span data-ttu-id="6c1aa-227">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="6c1aa-228">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="6c1aa-228">Custom source</span></span> |
| [<span data-ttu-id="6c1aa-229">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="6c1aa-230">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-230">Environment variables</span></span> |
| [<span data-ttu-id="6c1aa-231">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="6c1aa-232">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="6c1aa-233">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="6c1aa-234">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="6c1aa-234">In-memory collections</span></span> |
| <span data-ttu-id="6c1aa-235">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="6c1aa-236">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="6c1aa-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="6c1aa-237">Yapılandırma sağlayıcıları başlatma sırasında belirttiğiniz sırayla yapılandırma kaynaklarını okunur.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="6c1aa-238">Bu konuda açıklanan yapılandırma sağlayıcıları açıklanan alfabetik sırada, bunları kodunuzu düzenleme sırasını değil.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="6c1aa-239">Temel yapılandırma kaynakları için önceliklerinizden uyacak şekilde yapılandırma sağlayıcıları kodunuzu sırası.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="6c1aa-240">Yapılandırma sağlayıcıları, tipik bir dizisidir:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="6c1aa-241">Dosyaları (*appsettings.json*, *appsettings. { Ortam} .json*burada `{Environment}` uygulamanın geçerli barındırma ortamı)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="6c1aa-242">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="6c1aa-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="6c1aa-243">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki yalnızca)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="6c1aa-244">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-244">Environment variables</span></span>
1. <span data-ttu-id="6c1aa-245">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-245">Command-line arguments</span></span>

<span data-ttu-id="6c1aa-246">Komut satırı yapılandırma sağlayıcısı yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılmak komut satırı bağımsız değişkenlerine izin vermek için sağlayıcıları serisinin son konumlandırmak için yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-247">Yeni bir başlattığınızda bu sağlayıcıları dizi yerine konur <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6c1aa-248">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-249">Sağlayıcıları bu dizi için (ana değil) uygulaması ile oluşturulabilir bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ve bir çağrı, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> yönteminde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="6c1aa-250">Yukarıdaki örnekte, ortam adı (`env.EnvironmentName`) ve uygulama derleme adı (`env.ApplicationName`) tarafından sağlanan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="6c1aa-251">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="6c1aa-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="6c1aa-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="6c1aa-253">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ne zaman bunlara ek olarak, uygulamanın yapılandırma sağlayıcıları belirtmek için Web ana bilgisayarını oluşturma eklenen tarafından otomatik olarak <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="6c1aa-254">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="6c1aa-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Yapılandırma komut satırı bağımsız değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="6c1aa-256">Komut satırı yapılandırmasını etkinleştirmek için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> genişletme yöntemi bir örneğinde çağrıldığında <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-257">`AddCommandLine` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6c1aa-258">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6c1aa-259">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6c1aa-260">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="6c1aa-261">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6c1aa-262">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-262">Environment variables.</span></span>

<span data-ttu-id="6c1aa-263">`CreateDefaultBuilder` Son komut satırı yapılandırma sağlayıcısı ekler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="6c1aa-264">Çalışma zamanında geçirilen komut satırı bağımsız değişkenleri yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="6c1aa-265">`CreateDefaultBuilder` konak oluşturulduğunda işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="6c1aa-266">Bu nedenle, komut satırı yapılandırma etkinleştirildi tarafından `CreateDefaultBuilder` konak nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-267">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="6c1aa-268">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6c1aa-269">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-270">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c1aa-271">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="6c1aa-272">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder` olduğunda `UseConfiguration` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="6c1aa-273">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar çağırmak bir `ConfigurationBuilder` ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6c1aa-274">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-275">Komut satırı yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6c1aa-276">Son çalışma zamanında yapılandırması diğer yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenleri izin vermek için sağlayıcı çağırın.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="6c1aa-277">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6c1aa-278">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="6c1aa-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-279">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-280">1.x örnek uygulama çağrıları <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> üzerinde bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="6c1aa-281">Proje dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="6c1aa-282">Bir komut satırı bağımsız değişken `dotnet run` komutu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="6c1aa-283">Uygulama çalıştıktan sonra bir uygulamaya tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6c1aa-284">Çıkış için sağlanan yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çifti içeren gözlemleyin `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="6c1aa-285">Arguments</span><span class="sxs-lookup"><span data-stu-id="6c1aa-285">Arguments</span></span>

<span data-ttu-id="6c1aa-286">Değeri bir eşittir işareti gelmelidir (`=`), veya bir önek anahtarı olmalıdır (`--` veya `/`) zaman değeri bir boşluk izler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="6c1aa-287">Değer bir eşittir işareti kullanılıyorsa, null olabilir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="6c1aa-288">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="6c1aa-288">Key prefix</span></span>               | <span data-ttu-id="6c1aa-289">Örnek</span><span class="sxs-lookup"><span data-stu-id="6c1aa-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="6c1aa-290">Önek yok</span><span class="sxs-lookup"><span data-stu-id="6c1aa-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="6c1aa-291">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="6c1aa-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="6c1aa-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="6c1aa-293">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="6c1aa-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="6c1aa-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="6c1aa-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="6c1aa-295">Aynı komut içinde komut satırı bağımsız değişkeni bir eşittir işareti ile bir alanı kullanan anahtar-değer çiftleri kullanan anahtar-değer çiftleri karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="6c1aa-296">Örnek komutları:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="6c1aa-297">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-297">Switch mappings</span></span>

<span data-ttu-id="6c1aa-298">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="6c1aa-299">El ile yapı kurarken yapılandırmayla bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, anahtar değişiklik için bir sözlük sağlayabilir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="6c1aa-300">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="6c1aa-301">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) anahtar-değer çifti uygulamanın yapılandırmasını döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="6c1aa-302">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="6c1aa-303">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="6c1aa-304">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="6c1aa-305">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-306">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-307">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="6c1aa-308">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6c1aa-309">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="6c1aa-310">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6c1aa-311">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="6c1aa-312">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6c1aa-313">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="6c1aa-314">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="6c1aa-315">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="6c1aa-316">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-316">Key</span></span>       | <span data-ttu-id="6c1aa-317">Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="6c1aa-318">Anahtar eşlemeli anahtarları uygulamayı başlatırken kullandıysanız, yapılandırma sözlüğü tarafından sağlanan anahtardaki yapılandırma değeri alır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="6c1aa-319">Önceki komutu çalıştırdıktan sonra yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="6c1aa-320">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-320">Key</span></span>               | <span data-ttu-id="6c1aa-321">Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="6c1aa-322">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="6c1aa-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Yapılandırma ortam değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="6c1aa-324">Ortam değişkenlerini yapılandırma etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6c1aa-325">Ortam değişkenleri, iki nokta üst üste ayırıcı hiyerarşik anahtarlarla çalışırken (`:`) tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="6c1aa-326">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve virgül ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="6c1aa-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) ortam değişkenlerini yapılandırma Sağlayıcısı'nı kullanarak uygulama yapılandırması geçersiz kılabilirsiniz Azure portalında ortam değişkenlerini ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="6c1aa-328">Daha fazla bilgi için [Azure uygulamaları: Geçersiz Uygulama yapılandırması Azure portalını kullanarak](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-329">`AddEnvironmentVariables` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-329">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6c1aa-330">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6c1aa-331">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6c1aa-332">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-332">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="6c1aa-333">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-333">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6c1aa-334">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-334">Command-line arguments.</span></span>

<span data-ttu-id="6c1aa-335">Yapılandırma kullanıcı parolalarının kurulduktan sonra ortam değişkenlerini yapılandırma sağlayıcısı denir ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-335">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="6c1aa-336">Bu konumda sağlayıcıya çağrı ortam değişkenlerini okuma yapılandırması tarafından kullanıcı parolalarını ayarlanmış geçersiz kılmak için çalışma zamanında sağlar ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-336">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-337">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-337">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="6c1aa-338">`AddEnvironmentVariables` ortam değişkenlerini ön eki için `ASPNETCORE_` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-338">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6c1aa-339">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-340">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c1aa-341">Çağrı `AddEnvironmentVariables` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="6c1aa-342">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="6c1aa-343">`AddEnvironmentVariables` ortam değişkenlerini ön eki için `ASPNETCORE_` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-343">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6c1aa-344">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-344">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6c1aa-345">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-345">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-346">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-346">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6c1aa-347">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="6c1aa-347">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-348">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-348">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-349">1.x örnek uygulama çağrıları `AddEnvironmentVariables` üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-349">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="6c1aa-350">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-350">Run the sample app.</span></span> <span data-ttu-id="6c1aa-351">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-351">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6c1aa-352">Çıkış ortam değişkeni için anahtar-değer çifti içeren gözlemleyin `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-352">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="6c1aa-353">Değeri, uygulamanın çalıştığı, genellikle ortamın yansıtır `Development` yerel olarak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-353">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="6c1aa-354">Ortam değişkenlerini kısa uygulama tarafından işlenen listesini tutmak için aşağıdaki ile başlayan bu ortam değişkenleri uygulama filtreleri:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-354">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="6c1aa-355">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="6c1aa-355">ASPNETCORE_</span></span>
* <span data-ttu-id="6c1aa-356">URL'leri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-356">urls</span></span>
* <span data-ttu-id="6c1aa-357">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="6c1aa-357">Logging</span></span>
* <span data-ttu-id="6c1aa-358">ORTAM</span><span class="sxs-lookup"><span data-stu-id="6c1aa-358">ENVIRONMENT</span></span>
* <span data-ttu-id="6c1aa-359">contentRoot</span><span class="sxs-lookup"><span data-stu-id="6c1aa-359">contentRoot</span></span>
* <span data-ttu-id="6c1aa-360">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="6c1aa-360">AllowedHosts</span></span>
* <span data-ttu-id="6c1aa-361">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="6c1aa-361">applicationName</span></span>
* <span data-ttu-id="6c1aa-362">komut satırı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-362">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-363">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Pages/Index.cshtml.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-364">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Controllers/HomeController.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-364">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="6c1aa-365">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="6c1aa-365">Prefixes</span></span>

<span data-ttu-id="6c1aa-366">Uygulamanın yapılandırma yüklendi ortam değişkenleri, bir ön ek sağladığında filtrelenir `AddEnvironmentVariables` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-366">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="6c1aa-367">Örneğin, ortam değişkenlerini önek filtresi için `CUSTOM_`, yapılandırma sağlayıcısı için önek sağlayın:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-367">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="6c1aa-368">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek yapılandırıldıktan devre dışı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-368">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-369">Statik kolaylık yöntemi `CreateDefaultBuilder` oluşturur bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> uygulamanın ana bilgisayar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-369">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="6c1aa-370">Zaman `WebHostBuilder` olan oluşturulan, kendi ana bilgisayar yapılandırması ön ekine sahip ortam değişkenleri içindeki bulduğu `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-370">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="6c1aa-371">**Bağlantı dizesi ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="6c1aa-371">**Connection string prefixes**</span></span>

<span data-ttu-id="6c1aa-372">Yapılandırma API'si, dört bağlantı dizesi ortam değişkenleri için app ortamı için Azure bağlantı dizelerini yapılandırılmasıyla ilgili özel işleme kurallarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-372">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="6c1aa-373">Önek yok belirtilirse tabloda gösterilen ön ekine sahip ortam değişkenleri uygulamaya yüklenir `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-373">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="6c1aa-374">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="6c1aa-374">Connection string prefix</span></span> | <span data-ttu-id="6c1aa-375">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-375">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="6c1aa-376">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-376">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="6c1aa-377">MySQL</span><span class="sxs-lookup"><span data-stu-id="6c1aa-377">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="6c1aa-378">Azure SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-378">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="6c1aa-379">SQL Server</span><span class="sxs-lookup"><span data-stu-id="6c1aa-379">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="6c1aa-380">Ne zaman bir ortam değişkeni bulunur ve herhangi bir tabloda gösterilen dört öneklerini yapılandırmasını yüklendi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-380">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="6c1aa-381">Yapılandırma anahtarı ortam değişkeni ön eki kaldırma ve yapılandırma anahtar bölümünü ekleyerek oluşturulur (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-381">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="6c1aa-382">Veritabanı bağlantı sağlayıcısı temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (dışında `CUSTOMCONNSTR_`, belirtilen sağlayıcı yok sahiptir).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-382">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="6c1aa-383">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-383">Environment variable key</span></span> | <span data-ttu-id="6c1aa-384">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-384">Converted configuration key</span></span> | <span data-ttu-id="6c1aa-385">Sağlayıcı Yapılandırması girdisi</span><span class="sxs-lookup"><span data-stu-id="6c1aa-385">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6c1aa-386">Yapılandırma girişi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-386">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6c1aa-387">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-387">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6c1aa-388">Değer: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="6c1aa-388">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6c1aa-389">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-389">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6c1aa-390">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="6c1aa-390">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6c1aa-391">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-391">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6c1aa-392">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="6c1aa-392">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="6c1aa-393">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-393">File Configuration Provider</span></span>

<span data-ttu-id="6c1aa-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> yapılandırma dosya sisteminden yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="6c1aa-395">Aşağıdaki yapılandırma sağlayıcıları için belirli dosya türleri ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-395">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="6c1aa-396">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-396">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="6c1aa-397">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-397">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="6c1aa-398">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-398">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="6c1aa-399">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-399">INI Configuration Provider</span></span>

<span data-ttu-id="6c1aa-400"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Yapılandırma ını dosyası anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-400">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="6c1aa-401">INI dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-401">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6c1aa-402">İki nokta üst üste INI dosya yapılandırması bölüm ayırıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-402">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="6c1aa-403">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-403">Overloads permit specifying:</span></span>

* <span data-ttu-id="6c1aa-404">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-404">Whether the file is optional.</span></span>
* <span data-ttu-id="6c1aa-405">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-405">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6c1aa-406"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-406">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-407">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-407">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-408">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-408">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c1aa-409">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-409">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6c1aa-410">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-410">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-411">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-411">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6c1aa-412">Genel bir INI yapılandırma dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-412">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="6c1aa-413">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-413">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6c1aa-414">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-414">section0:key0</span></span>
* <span data-ttu-id="6c1aa-415">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-415">section0:key1</span></span>
* <span data-ttu-id="6c1aa-416">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="6c1aa-416">section1:subsection:key</span></span>
* <span data-ttu-id="6c1aa-417">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="6c1aa-417">section2:subsection0:key</span></span>
* <span data-ttu-id="6c1aa-418">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="6c1aa-418">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="6c1aa-419">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-419">JSON Configuration Provider</span></span>

<span data-ttu-id="6c1aa-420"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Yapılandırma JSON dosyası anahtar-değer çiftlerinden çalışma zamanı sırasında yükler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-420">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="6c1aa-421">JSON dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-421">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6c1aa-422">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-422">Overloads permit specifying:</span></span>

* <span data-ttu-id="6c1aa-423">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-423">Whether the file is optional.</span></span>
* <span data-ttu-id="6c1aa-424">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-424">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6c1aa-425"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-425">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-426">`AddJsonFile` Yeni bir başlattığınızda iki kez otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-426">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6c1aa-427">Yöntem yapılandırmasından yüklenemedi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-427">The method is called to load configuration from:</span></span>

* <span data-ttu-id="6c1aa-428">*appSettings.JSON* &ndash; bu dosyayı ilk okuyun.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-428">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="6c1aa-429">Dosyanın ortam sürümü tarafından sağlanan değerleri geçersiz kılabilir *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-429">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="6c1aa-430">*appSettings. {Ortamı} .json* &ndash; dosyanın ortam sürümünü temel alınarak yüklenir [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-430">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="6c1aa-431">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-431">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6c1aa-432">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-432">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6c1aa-433">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-433">Environment variables.</span></span>
* <span data-ttu-id="6c1aa-434">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-434">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6c1aa-435">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-435">Command-line arguments.</span></span>

<span data-ttu-id="6c1aa-436">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-436">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="6c1aa-437">Bu nedenle, yapılandırma kümesi kullanıcı parolaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri geçersiz kılma *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-437">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-438">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırma dosyaları için dışında belirtmek için konak oluştururken *appsettings.json* ve *appsettings. { Ortam} .json*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-438">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-439">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-439">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c1aa-440">Ayrıca doğrudan çağırabilir miyim `AddJsonFile` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-440">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6c1aa-441">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-441">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6c1aa-442">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-442">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-443">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-443">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6c1aa-444">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="6c1aa-444">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-445">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` iki çağrıları içeren ana bilgisayar oluşturmak için `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-445">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="6c1aa-446">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-446">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-447">1.x örnek uygulama çağrıları `AddJsonFile` iki kez üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-447">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="6c1aa-448">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-448">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="6c1aa-449">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-449">Run the sample app.</span></span> <span data-ttu-id="6c1aa-450">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-450">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6c1aa-451">Çıkış ortamına bağlı olarak tabloda gösterilen yapılandırması için anahtar-değer çiftleri içeren gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-451">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="6c1aa-452">Günlük kaydı yapılandırması tuşlarını iki nokta üst üste (`:`) hiyerarşik ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-452">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="6c1aa-453">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-453">Key</span></span>                        | <span data-ttu-id="6c1aa-454">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-454">Development Value</span></span> | <span data-ttu-id="6c1aa-455">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-455">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="6c1aa-456">Günlüğe kaydetme: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="6c1aa-456">Logging:LogLevel:System</span></span>    | <span data-ttu-id="6c1aa-457">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="6c1aa-457">Information</span></span>       | <span data-ttu-id="6c1aa-458">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="6c1aa-458">Information</span></span>      |
| <span data-ttu-id="6c1aa-459">Günlüğe kaydetme: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="6c1aa-459">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="6c1aa-460">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="6c1aa-460">Information</span></span>       | <span data-ttu-id="6c1aa-461">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="6c1aa-461">Information</span></span>      |
| <span data-ttu-id="6c1aa-462">Günlüğe kaydetme: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="6c1aa-462">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="6c1aa-463">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="6c1aa-463">Debug</span></span>             | <span data-ttu-id="6c1aa-464">Hata</span><span class="sxs-lookup"><span data-stu-id="6c1aa-464">Error</span></span>            |
| <span data-ttu-id="6c1aa-465">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="6c1aa-465">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="6c1aa-466">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-466">XML Configuration Provider</span></span>

<span data-ttu-id="6c1aa-467"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Yapılandırma XML dosyası anahtar-değer çiftlerinin zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-467">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="6c1aa-468">XML dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-468">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6c1aa-469">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-469">Overloads permit specifying:</span></span>

* <span data-ttu-id="6c1aa-470">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-470">Whether the file is optional.</span></span>
* <span data-ttu-id="6c1aa-471">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-471">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6c1aa-472"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-472">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="6c1aa-473">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayıldı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-473">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="6c1aa-474">Bir belge türü tanımı (DTD'nin) veya ad alanı dosyasında belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-474">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-475">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-475">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-476">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-476">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c1aa-477">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-477">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6c1aa-478">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-478">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-479">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-479">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6c1aa-480">XML yapılandırma dosyalarını, yinelenen bölümler için ayrı bir öğe adları kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-480">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="6c1aa-481">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-481">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6c1aa-482">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-482">section0:key0</span></span>
* <span data-ttu-id="6c1aa-483">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-483">section0:key1</span></span>
* <span data-ttu-id="6c1aa-484">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-484">section1:key0</span></span>
* <span data-ttu-id="6c1aa-485">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-485">section1:key1</span></span>

<span data-ttu-id="6c1aa-486">İş öğesi adının aynısını kullanın öğeleri, yinelenen `name` özniteliği öğeleri ayırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-486">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="6c1aa-487">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-487">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6c1aa-488">Bölüm: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-488">section:section0:key:key0</span></span>
* <span data-ttu-id="6c1aa-489">Bölüm: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-489">section:section0:key:key1</span></span>
* <span data-ttu-id="6c1aa-490">Bölüm: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-490">section:section1:key:key0</span></span>
* <span data-ttu-id="6c1aa-491">Bölüm: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-491">section:section1:key:key1</span></span>

<span data-ttu-id="6c1aa-492">Öznitelik değerlerini sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-492">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="6c1aa-493">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-493">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6c1aa-494">Anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="6c1aa-494">key:attribute</span></span>
* <span data-ttu-id="6c1aa-495">Bölüm: anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="6c1aa-495">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="6c1aa-496">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-496">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="6c1aa-497"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bir dizin dosyalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-497">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="6c1aa-498">Anahtar dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-498">The key is the file name.</span></span> <span data-ttu-id="6c1aa-499">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-499">The value contains the file's contents.</span></span> <span data-ttu-id="6c1aa-500">Dosya başına anahtar yapılandırma sağlayıcısı Docker'da barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-500">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="6c1aa-501">Dosya başına anahtar yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-501">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="6c1aa-502">`directoryPath` Dosyalar için mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-502">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="6c1aa-503">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-503">Overloads permit specifying:</span></span>

* <span data-ttu-id="6c1aa-504">Bir `Action<KeyPerFileConfigurationSource>` kaynağını yapılandırır temsilci.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-504">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="6c1aa-505">Dizin isteğe bağlı olup olmadığını ve dizinin yolu.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-505">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="6c1aa-506">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-506">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-507">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-507">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="6c1aa-508">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-508">Memory Configuration Provider</span></span>

<span data-ttu-id="6c1aa-509"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Bir bellek içi koleksiyonu yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-509">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="6c1aa-510">Bellek içi toplama yapılandırması etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-510">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6c1aa-511">Yapılandırma sağlayıcısı ile başlatılabilir bir `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-511">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c1aa-512">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-512">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="6c1aa-513">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-513">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c1aa-514">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-514">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

<span data-ttu-id="6c1aa-515">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-515">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-516">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-516">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="6c1aa-517">GetValue</span><span class="sxs-lookup"><span data-stu-id="6c1aa-517">GetValue</span></span>

<span data-ttu-id="6c1aa-518">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) bir değeri belirtilen bir anahtarla yapılandırmasından ayıklar ve onu belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="6c1aa-519">Aşırı yükleme anahtarı bulunmazsa, varsayılan bir değer sağlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-519">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="6c1aa-520">Aşağıdaki örnek bir dize değeri yapılandırmasından anahtarıyla ayıklar `NumberKey`, değer olarak türleri bir `int`ve değeri değişkeninde depolar `intValue`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-520">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="6c1aa-521">Varsa `NumberKey` yapılandırma anahtarlarını bulunamadığında `intValue` varsayılan değerini alır `99`:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-521">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="6c1aa-522">GetSection, GetChildren ve var.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-522">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="6c1aa-523">Aşağıdaki örneklerde için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-523">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="6c1aa-524">Dört anahtarları içeren bir çift alt bölümleri içerir, iki bölümlerde bulunur:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-524">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="6c1aa-525">Dosya yapılandırma okuduğunuzda aşağıdaki benzersiz hiyerarşik anahtarları yapılandırma değerleri tutmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-525">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="6c1aa-526">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-526">section0:key0</span></span>
* <span data-ttu-id="6c1aa-527">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-527">section0:key1</span></span>
* <span data-ttu-id="6c1aa-528">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-528">section1:key0</span></span>
* <span data-ttu-id="6c1aa-529">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-529">section1:key1</span></span>
* <span data-ttu-id="6c1aa-530">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-530">section2:subsection0:key0</span></span>
* <span data-ttu-id="6c1aa-531">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-531">section2:subsection0:key1</span></span>
* <span data-ttu-id="6c1aa-532">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-532">section2:subsection1:key0</span></span>
* <span data-ttu-id="6c1aa-533">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-533">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="6c1aa-534">GetSection</span><span class="sxs-lookup"><span data-stu-id="6c1aa-534">GetSection</span></span>

<span data-ttu-id="6c1aa-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) yapılandırma bölümüne belirtilen alt anahtarını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="6c1aa-536">Döndürülecek bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> yalnızca anahtar-değer çiftlerini içeren `section1`, çağrı `GetSection` ve bölüm adı verin:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-536">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="6c1aa-537">Benzer şekilde, anahtarları değerlerini almak için `section2:subsection0`, çağrı `GetSection` ve bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-537">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="6c1aa-538">`GetSection` hiç dönmüyor `null`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-538">`GetSection` never returns `null`.</span></span> <span data-ttu-id="6c1aa-539">Eşleşen bir bölümü olmadığından bulunamazsa, boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-539">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="6c1aa-540">GetChildren</span><span class="sxs-lookup"><span data-stu-id="6c1aa-540">GetChildren</span></span>

<span data-ttu-id="6c1aa-541">Bir çağrı [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) üzerinde `section2` alır bir `IEnumerable<IConfigurationSection>` içeren:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-541">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="6c1aa-542">Var.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-542">Exists</span></span>

<span data-ttu-id="6c1aa-543">Kullanım [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) yapılandırma bölümü olup olmadığını belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-543">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="6c1aa-544">Belirtilen örnek veri `sectionExists` olduğu `false` olmadığından bir `section2:subsection2` yapılandırma verilerini bir bölümde.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-544">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="6c1aa-545">Bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="6c1aa-545">Bind to a class</span></span>

<span data-ttu-id="6c1aa-546">Yapılandırma kullanarak ilgili ayar gruplarını temsil eden sınıflar için bağlanabilir *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-546">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="6c1aa-547">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-547">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="6c1aa-548">Yapılandırma değerleri döndürülür dizeler olarak çağıran <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> oluşumu sağlayan [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesneleri.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-548">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="6c1aa-549">Örnek uygulamayı içeren bir `Starship` modeli (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="6c1aa-549">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-550">`starship` Bölümünü *starship.json* örnek uygulamayı yapılandırma yüklemek için JSON yapılandırma sağlayıcısı kullandığında yapılandırma dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-550">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="6c1aa-551">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-551">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="6c1aa-552">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-552">Key</span></span>                   | <span data-ttu-id="6c1aa-553">Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-553">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="6c1aa-554">starship: adı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-554">starship:name</span></span>         | <span data-ttu-id="6c1aa-555">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="6c1aa-555">USS Enterprise</span></span>                                    |
| <span data-ttu-id="6c1aa-556">starship: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="6c1aa-556">starship:registry</span></span>     | <span data-ttu-id="6c1aa-557">NCC 1701</span><span class="sxs-lookup"><span data-stu-id="6c1aa-557">NCC-1701</span></span>                                          |
| <span data-ttu-id="6c1aa-558">starship: sınıfı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-558">starship:class</span></span>        | <span data-ttu-id="6c1aa-559">Anayasa</span><span class="sxs-lookup"><span data-stu-id="6c1aa-559">Constitution</span></span>                                      |
| <span data-ttu-id="6c1aa-560">starship: uzunluğu</span><span class="sxs-lookup"><span data-stu-id="6c1aa-560">starship:length</span></span>       | <span data-ttu-id="6c1aa-561">304.8</span><span class="sxs-lookup"><span data-stu-id="6c1aa-561">304.8</span></span>                                             |
| <span data-ttu-id="6c1aa-562">starship: yetkilendirilen</span><span class="sxs-lookup"><span data-stu-id="6c1aa-562">starship:commissioned</span></span> | <span data-ttu-id="6c1aa-563">False</span><span class="sxs-lookup"><span data-stu-id="6c1aa-563">False</span></span>                                             |
| <span data-ttu-id="6c1aa-564">Ticari marka</span><span class="sxs-lookup"><span data-stu-id="6c1aa-564">trademark</span></span>             | <span data-ttu-id="6c1aa-565">Paramount resimleri Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="6c1aa-565">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="6c1aa-566">Örnek Uygulama çağrıları `GetSection` ile `starship` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-566">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="6c1aa-567">`starship` Anahtar-değer çiftleridir yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-567">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="6c1aa-568">`Bind` Bir örneğini geçirerek Altbölüm yöntemi çağrıldığında `Starship` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-568">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="6c1aa-569">Örnek değerleri bağlandıktan sonra işleme için bir özellik için örneği atanır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-569">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="6c1aa-570">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="6c1aa-570">Bind to an object graph</span></span>

<span data-ttu-id="6c1aa-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Tüm POCO Nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="6c1aa-572">Örnek içeren bir `TvShow` olan nesne grafiğini içeren model `Metadata` ve `Actors` sınıfları (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="6c1aa-572">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-573">Örnek uygulamanın bir *tvshow.xml* yapılandırma verilerini içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-573">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="6c1aa-574">Yapılandırma tüm bağlı `TvShow` Nesne grafiği ile `Bind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-574">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="6c1aa-575">İlişkili örneği, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-575">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="6c1aa-576">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="6c1aa-577">`Get<T>` kullanmaktan daha kullanışlı olan `Bind`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-577">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="6c1aa-578">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle birlikte işlemek için kullanılan özellik doğrudan atanan bağlı örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-578">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="6c1aa-579">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="6c1aa-579">Bind an array to a class</span></span>

<span data-ttu-id="6c1aa-580">*Örnek uygulama, bu bölümde açıklanan kavramları göstermektedir.*</span><span class="sxs-lookup"><span data-stu-id="6c1aa-580">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="6c1aa-581"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-581">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="6c1aa-582">Sayısal bir anahtar kesimi sunan herhangi bir dizi biçimi (`:0:`, `:1:`, &hellip; `:{n}:`) dizisi bağlama POCO sınıfı dizisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-582">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="6c1aa-583">Bağlama, kural olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-583">Binding is provided by convention.</span></span> <span data-ttu-id="6c1aa-584">Özel yapılandırma sağlayıcıları dizi bağlama uygulamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-584">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="6c1aa-585">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="6c1aa-585">**In-memory array processing**</span></span>

<span data-ttu-id="6c1aa-586">Yapılandırma anahtarları ve değerleri aşağıdaki tabloda gösterilen göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-586">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="6c1aa-587">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-587">Key</span></span>             | <span data-ttu-id="6c1aa-588">Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="6c1aa-589">dizi: girişler: 0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-589">array:entries:0</span></span> | <span data-ttu-id="6c1aa-590">value0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-590">value0</span></span> |
| <span data-ttu-id="6c1aa-591">dizi: girişler: 1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-591">array:entries:1</span></span> | <span data-ttu-id="6c1aa-592">Değer1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-592">value1</span></span> |
| <span data-ttu-id="6c1aa-593">dizi: girişler: 2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-593">array:entries:2</span></span> | <span data-ttu-id="6c1aa-594">Value2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-594">value2</span></span> |
| <span data-ttu-id="6c1aa-595">dizi: girişler: 4</span><span class="sxs-lookup"><span data-stu-id="6c1aa-595">array:entries:4</span></span> | <span data-ttu-id="6c1aa-596">Değer4</span><span class="sxs-lookup"><span data-stu-id="6c1aa-596">value4</span></span> |
| <span data-ttu-id="6c1aa-597">dizi: girişler: 5</span><span class="sxs-lookup"><span data-stu-id="6c1aa-597">array:entries:5</span></span> | <span data-ttu-id="6c1aa-598">Değeri5</span><span class="sxs-lookup"><span data-stu-id="6c1aa-598">value5</span></span> |

<span data-ttu-id="6c1aa-599">Bellek yapılandırma sağlayıcısı kullanan örnek uygulamasında, bu anahtarların ve değerlerin yüklenir:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-599">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="6c1aa-600">Dizi dizini için bir değer atlar &num;3.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-600">The array skips a value for index &num;3.</span></span> <span data-ttu-id="6c1aa-601">Yapılandırma bağlayıcı temizleyin, bu dizi için bir nesne bağlamanın sonucunu gösterilmiştir birazdan haline geldikten bağlama null değerler veya null girişler bağımlı nesneleri oluşturma yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-601">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="6c1aa-602">Örnek uygulamada, ilişkili yapılandırma verilerini tutmak bir POCO sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-602">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-603">Yapılandırma verilerini nesnesine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-603">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="6c1aa-604">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) söz dizimi de kullanılabilir, daha küçük kod sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-604">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="6c1aa-605">İlişkili nesne, örneği `ArrayExample`, yapılandırmasından dizisi verileri alır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-605">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="6c1aa-606">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="6c1aa-606">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="6c1aa-607">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-607">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="6c1aa-608">0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-608">0</span></span>                            | <span data-ttu-id="6c1aa-609">value0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-609">value0</span></span>                       |
| <span data-ttu-id="6c1aa-610">1.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-610">1</span></span>                            | <span data-ttu-id="6c1aa-611">Değer1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-611">value1</span></span>                       |
| <span data-ttu-id="6c1aa-612">2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-612">2</span></span>                            | <span data-ttu-id="6c1aa-613">Value2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-613">value2</span></span>                       |
| <span data-ttu-id="6c1aa-614">3</span><span class="sxs-lookup"><span data-stu-id="6c1aa-614">3</span></span>                            | <span data-ttu-id="6c1aa-615">Değer4</span><span class="sxs-lookup"><span data-stu-id="6c1aa-615">value4</span></span>                       |
| <span data-ttu-id="6c1aa-616">4</span><span class="sxs-lookup"><span data-stu-id="6c1aa-616">4</span></span>                            | <span data-ttu-id="6c1aa-617">Değeri5</span><span class="sxs-lookup"><span data-stu-id="6c1aa-617">value5</span></span>                       |

<span data-ttu-id="6c1aa-618">Dizin &num;3'te ilişkili nesne için yapılandırma verilerini tutan `array:4` yapılandırma anahtarı ve değeri `value4`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-618">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="6c1aa-619">Bir diziyi içeren yapılandırma verilerini bağlandığında, dizi dizinleri yapılandırma anahtarları yalnızca nesne oluşturma sırasında yapılandırma verileri yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-619">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="6c1aa-620">Yapılandırma verilerinde bir null değer tutulamıyor ve yapılandırma anahtarları bir dizide bir veya daha fazla dizinlerini atladığında null değerli bir girişi bir bağımlı nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-620">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="6c1aa-621">Eksik yapılandırma öğesi için dizin &num;bağlama önce 3 sağlanabilir `ArrayExample` örneği üretir yapılandırma doğru anahtar-değer çifti herhangi bir yapılandırma sağlayıcısı tarafından.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-621">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="6c1aa-622">Ek bir JSON yapılandırma sağlayıcısı eksik anahtar-değer çifti ile örneği varsa `ArrayExample.Entries` yapılandırmanın tamamı dizisi ile eşleşen:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-622">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="6c1aa-623">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-623">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c1aa-624">İçinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-624">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c1aa-625">İçinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-625">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="6c1aa-626">Tabloda belirtilen anahtar-değer çifti yapılandırma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-626">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="6c1aa-627">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-627">Key</span></span>             | <span data-ttu-id="6c1aa-628">Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-628">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="6c1aa-629">dizi: girişler: 3</span><span class="sxs-lookup"><span data-stu-id="6c1aa-629">array:entries:3</span></span> | <span data-ttu-id="6c1aa-630">Değeri3</span><span class="sxs-lookup"><span data-stu-id="6c1aa-630">value3</span></span> |

<span data-ttu-id="6c1aa-631">Varsa `ArrayExample` sınıf örneği bağlı dizin için giriş JSON yapılandırma sağlayıcısı içerir sonra &num;3 `ArrayExample.Entries` dizi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-631">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="6c1aa-632">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="6c1aa-632">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="6c1aa-633">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-633">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="6c1aa-634">0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-634">0</span></span>                            | <span data-ttu-id="6c1aa-635">value0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-635">value0</span></span>                       |
| <span data-ttu-id="6c1aa-636">1.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-636">1</span></span>                            | <span data-ttu-id="6c1aa-637">Değer1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-637">value1</span></span>                       |
| <span data-ttu-id="6c1aa-638">2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-638">2</span></span>                            | <span data-ttu-id="6c1aa-639">Value2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-639">value2</span></span>                       |
| <span data-ttu-id="6c1aa-640">3</span><span class="sxs-lookup"><span data-stu-id="6c1aa-640">3</span></span>                            | <span data-ttu-id="6c1aa-641">Değeri3</span><span class="sxs-lookup"><span data-stu-id="6c1aa-641">value3</span></span>                       |
| <span data-ttu-id="6c1aa-642">4</span><span class="sxs-lookup"><span data-stu-id="6c1aa-642">4</span></span>                            | <span data-ttu-id="6c1aa-643">Değer4</span><span class="sxs-lookup"><span data-stu-id="6c1aa-643">value4</span></span>                       |
| <span data-ttu-id="6c1aa-644">5</span><span class="sxs-lookup"><span data-stu-id="6c1aa-644">5</span></span>                            | <span data-ttu-id="6c1aa-645">Değeri5</span><span class="sxs-lookup"><span data-stu-id="6c1aa-645">value5</span></span>                       |

<span data-ttu-id="6c1aa-646">**JSON dizisi işleme**</span><span class="sxs-lookup"><span data-stu-id="6c1aa-646">**JSON array processing**</span></span>

<span data-ttu-id="6c1aa-647">Bir JSON dosyası bir dizi varsa, dizi öğeleri bölümünde sıfır tabanlı dizine sahip için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-647">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="6c1aa-648">Aşağıdaki yapılandırma dosyasında `subsection` dizisi:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-648">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="6c1aa-649">JSON yapılandırma sağlayıcısı, aşağıdaki anahtar-değer çiftlerine yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-649">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="6c1aa-650">Anahtar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-650">Key</span></span>                     | <span data-ttu-id="6c1aa-651">Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-651">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="6c1aa-652">json_array:Key</span><span class="sxs-lookup"><span data-stu-id="6c1aa-652">json_array:key</span></span>          | <span data-ttu-id="6c1aa-653">Değera</span><span class="sxs-lookup"><span data-stu-id="6c1aa-653">valueA</span></span> |
| <span data-ttu-id="6c1aa-654">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-654">json_array:subsection:0</span></span> | <span data-ttu-id="6c1aa-655">Değerb</span><span class="sxs-lookup"><span data-stu-id="6c1aa-655">valueB</span></span> |
| <span data-ttu-id="6c1aa-656">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="6c1aa-656">json_array:subsection:1</span></span> | <span data-ttu-id="6c1aa-657">valueC</span><span class="sxs-lookup"><span data-stu-id="6c1aa-657">valueC</span></span> |
| <span data-ttu-id="6c1aa-658">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-658">json_array:subsection:2</span></span> | <span data-ttu-id="6c1aa-659">Değerli</span><span class="sxs-lookup"><span data-stu-id="6c1aa-659">valueD</span></span> |

<span data-ttu-id="6c1aa-660">Örnek uygulamada, aşağıdaki POCO sınıfı yapılandırma anahtar-değer çiftleri bağlamak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-660">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-661">Bağlama sonra `JsonArrayExample.Key` değerine `valueA`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-661">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="6c1aa-662">Alt değerleri POCO dizi özelliğinde depolanır `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-662">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="6c1aa-663">`JsonArrayExample.Subsection` Dizin</span><span class="sxs-lookup"><span data-stu-id="6c1aa-663">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="6c1aa-664">`JsonArrayExample.Subsection` Değer</span><span class="sxs-lookup"><span data-stu-id="6c1aa-664">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="6c1aa-665">0</span><span class="sxs-lookup"><span data-stu-id="6c1aa-665">0</span></span>                                   | <span data-ttu-id="6c1aa-666">Değerb</span><span class="sxs-lookup"><span data-stu-id="6c1aa-666">valueB</span></span>                              |
| <span data-ttu-id="6c1aa-667">1.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-667">1</span></span>                                   | <span data-ttu-id="6c1aa-668">valueC</span><span class="sxs-lookup"><span data-stu-id="6c1aa-668">valueC</span></span>                              |
| <span data-ttu-id="6c1aa-669">2</span><span class="sxs-lookup"><span data-stu-id="6c1aa-669">2</span></span>                                   | <span data-ttu-id="6c1aa-670">Değerli</span><span class="sxs-lookup"><span data-stu-id="6c1aa-670">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="6c1aa-671">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6c1aa-671">Custom configuration provider</span></span>

<span data-ttu-id="6c1aa-672">Örnek uygulamayı yapılandırma anahtar-değer çiftleri kullanarak bir veritabanını okuyan bir temel yapılandırma sağlayıcısı oluşturma gösterilmektedir [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-672">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="6c1aa-673">Sağlayıcı, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-673">The provider has the following characteristics:</span></span>

* <span data-ttu-id="6c1aa-674">EF bellek içi veritabanına tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-674">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="6c1aa-675">Bir bağlantı dizesi gerektiren bir veritabanı kullanmak için ikincil uygulama `ConfigurationBuilder` başka bir yapılandırma sağlayıcısı bağlantı dizesinden sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-675">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="6c1aa-676">Sağlayıcı bir veritabanı tablosu, başlangıç yapılandırmasını içine okur.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-676">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="6c1aa-677">Sağlayıcı anahtarı başına temelinde veritabanını sorgulamak değil.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-677">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="6c1aa-678">Uygulama başlatılır sahip olduktan sonra uygulamanın yapılandırma üzerinde hiçbir etkisi kadar veritabanını güncelleme yeniden üzerinde değişiklik uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-678">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="6c1aa-679">Tanımlayan bir `EFConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-679">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="6c1aa-680">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-680">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-681">Ekleme bir `EFConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-681">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="6c1aa-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-683">Uygulayan bir sınıf oluşturma <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-683">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="6c1aa-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-685">Özel yapılandırma sağlayıcısını devralarak oluşturma <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-685">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="6c1aa-686">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-686">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="6c1aa-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-688">Bir `AddEFConfiguration` genişletme yöntemi izin veren yapılandırması kaynağına ekleme bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-688">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="6c1aa-689">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-689">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6c1aa-690">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigurationProvider` içinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-690">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="6c1aa-691">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6c1aa-691">Access configuration during startup</span></span>

<span data-ttu-id="6c1aa-692">Ekleme `IConfiguration` içine `Startup` oluşturucuya erişim yapılandırma değerlerini `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-692">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6c1aa-693">Erişim yapılandırmaya `Startup.Configure`, ya da ekleme `IConfiguration` doğrudan yöntem veya oluşturucu örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-693">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="6c1aa-694">Başlangıç kullanışlı yöntemler kullanarak yapılandırma erişme ilişkin bir örnek için bkz [uygulama başlatma: yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="6c1aa-694">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="6c1aa-695">Erişim yapılandırmasında bir Razor sayfaları sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="6c1aa-695">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="6c1aa-696">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](xref:Microsoft.Extensions.Configuration) ve ekleme <xref:Microsoft.Extensions.Configuration.IConfiguration> sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-696">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="6c1aa-697">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-697">In a Razor Pages page:</span></span>

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

<span data-ttu-id="6c1aa-698">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="6c1aa-698">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="6c1aa-699">Dış bütünleştirilmiş koddan Yapılandırması Ekle</span><span class="sxs-lookup"><span data-stu-id="6c1aa-699">Add configuration from an external assembly</span></span>

<span data-ttu-id="6c1aa-700">Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-700">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="6c1aa-701">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="6c1aa-701">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c1aa-702">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6c1aa-702">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="6c1aa-703">Microsoft yapılandırma hakkında ayrıntılı bir inceleme</span><span class="sxs-lookup"><span data-stu-id="6c1aa-703">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
