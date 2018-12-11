---
title: ASP.NET core'da yapılandırma
author: guardrex
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 6f0378ffc4f9a1efa95c8f70d70e7799abef130b
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216904"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="24cdb-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24cdb-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="24cdb-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="24cdb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="24cdb-105">ASP.NET core'da uygulama yapılandırması tarafından kurulan anahtar-değer çiftleri temel *yapılandırma sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="24cdb-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="24cdb-106">Yapılandırma sağlayıcıları, yapılandırma kaynaklarını çeşitli anahtar-değer çiftlerine yapılandırma verilerini okuyun:</span><span class="sxs-lookup"><span data-stu-id="24cdb-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="24cdb-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24cdb-107">Azure Key Vault</span></span>
* <span data-ttu-id="24cdb-108">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-108">Command-line arguments</span></span>
* <span data-ttu-id="24cdb-109">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="24cdb-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="24cdb-110">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="24cdb-110">Directory files</span></span>
* <span data-ttu-id="24cdb-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-111">Environment variables</span></span>
* <span data-ttu-id="24cdb-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-112">In-memory .NET objects</span></span>
* <span data-ttu-id="24cdb-113">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="24cdb-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="24cdb-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24cdb-114">Azure Key Vault</span></span>
* <span data-ttu-id="24cdb-115">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-115">Command-line arguments</span></span>
* <span data-ttu-id="24cdb-116">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="24cdb-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="24cdb-117">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-117">Environment variables</span></span>
* <span data-ttu-id="24cdb-118">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-118">In-memory .NET objects</span></span>
* <span data-ttu-id="24cdb-119">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="24cdb-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="24cdb-120">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-120">Command-line arguments</span></span>
* <span data-ttu-id="24cdb-121">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="24cdb-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="24cdb-122">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-122">Environment variables</span></span>
* <span data-ttu-id="24cdb-123">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-123">In-memory .NET objects</span></span>
* <span data-ttu-id="24cdb-124">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="24cdb-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="24cdb-125">*Seçenekleri deseni* bu konuda açıklanan yapılandırma kavramları bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="24cdb-126">Seçenekler, ilgili ayar gruplarını temsil etmek için sınıflar kullanır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="24cdb-127">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="24cdb-128">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="24cdb-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="24cdb-129">Bu konudaki sağladığınız örnekleri temel kullanır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="24cdb-130">Temel yol ile uygulama ayarı <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="24cdb-131">`SetBasePath` başvurarak bir uygulama için kullanılabilir hale getirileceğini [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.</span><span class="sxs-lookup"><span data-stu-id="24cdb-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="24cdb-132">Yapılandırma dosyalarıyla bölümlerini çözümleme <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="24cdb-133">`GetSection` başvurarak bir uygulama için kullanılabilir hale getirileceğini [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.</span><span class="sxs-lookup"><span data-stu-id="24cdb-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="24cdb-134">.NET için bağlama yapılandırması sınıfları ile <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> ve [alma&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="24cdb-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="24cdb-135">`Bind` ve `Get<T>` başvurarak bir uygulama için kullanılabilir yapılır [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.</span><span class="sxs-lookup"><span data-stu-id="24cdb-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="24cdb-136">`Get<T>` ASP.NET Core 1.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-137">Bu üç paketi içinde yer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="24cdb-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="24cdb-138">Bu üç paketi içinde yer [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="24cdb-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="24cdb-139">Uygulama yapılandırması barındırın</span><span class="sxs-lookup"><span data-stu-id="24cdb-139">Host vs. app configuration</span></span>

<span data-ttu-id="24cdb-140">Uygulama yapılandırılmış ve başlatıldı, önce bir *konak* başlatılan ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="24cdb-141">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="24cdb-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="24cdb-142">Bu konuda açıklanan yapılandırma sağlayıcıları kullanarak, hem uygulama hem de konak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="24cdb-143">Ana bilgisayar yapılandırma anahtar-değer çiftleri uygulamanın genel yapılandırmasının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="24cdb-144">Yapılandırma sağlayıcıları konak oluşturulduğunda kullanılan yapılandırma ve yapılandırma kaynaklarını nasıl etkileyeceğini nasıl barındırmak daha fazla bilgi için bkz: <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="24cdb-145">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="24cdb-145">Security</span></span>

<span data-ttu-id="24cdb-146">Aşağıdaki en iyi benimseme:</span><span class="sxs-lookup"><span data-stu-id="24cdb-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="24cdb-147">Hiçbir zaman parolaları ve diğer hassas verileri yapılandırma sağlayıcısı kodda veya düz metin yapılandırma dosyalarında depolayın.</span><span class="sxs-lookup"><span data-stu-id="24cdb-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="24cdb-148">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="24cdb-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="24cdb-149">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="24cdb-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="24cdb-150">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [gizli dizi Yöneticisi ile geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) (depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. hassas verileri).</span><span class="sxs-lookup"><span data-stu-id="24cdb-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="24cdb-151">Gizli dizi Yöneticisi'ni dosya yapılandırma sağlayıcısı bir JSON dosyası yerel sistemdeki kullanıcı gizli dizileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="24cdb-152">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="24cdb-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) uygulama gizli anahtarlarının güvenli bir şekilde depolanması için bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="24cdb-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="24cdb-154">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="24cdb-155">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="24cdb-155">Hierarchical configuration data</span></span>

<span data-ttu-id="24cdb-156">Yapılandırma API configuration anahtarlarında bir sınırlayıcı kullanarak hiyerarşik veri düzleştirme tarafından hiyerarşik yapılandırma verileri koruma özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="24cdb-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="24cdb-157">Aşağıdaki JSON dosyasında iki bölüm yapılandırılmış bir hiyerarşide dört anahtarları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="24cdb-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="24cdb-158">Dosya yapılandırma okuduğunuzda benzersiz anahtarlar özgün hiyerarşik veri yapısını yapılandırma kaynağı korumak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24cdb-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="24cdb-159">Bölümler ve anahtarlar ile bir iki nokta üst üste kullanımını düzleştirilir (`:`) özgün yapıyı korumak için:</span><span class="sxs-lookup"><span data-stu-id="24cdb-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="24cdb-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-160">section0:key0</span></span>
* <span data-ttu-id="24cdb-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-161">section0:key1</span></span>
* <span data-ttu-id="24cdb-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-162">section1:key0</span></span>
* <span data-ttu-id="24cdb-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-163">section1:key1</span></span>

<span data-ttu-id="24cdb-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemlerdir bölümler ve yapılandırma verilerini bir bölümde alt yalıtmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="24cdb-165">Bu yöntem daha sonra açıklanmıştır [GetSection GetChildren ve Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="24cdb-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="24cdb-166">Kurallar</span><span class="sxs-lookup"><span data-stu-id="24cdb-166">Conventions</span></span>

<span data-ttu-id="24cdb-167">Uygulama başlangıcında, yapılandırma kaynaklarını yapılandırma sağlayıcıları belirttiğiniz sırayla okunur.</span><span class="sxs-lookup"><span data-stu-id="24cdb-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="24cdb-168">Dosya yapılandırma sağlayıcıları uygulama başlangıcından sonra bir temel alınan ayarları dosyası değiştirildiğinde yapılandırmayı yeniden yükle seçeneğine sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="24cdb-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="24cdb-169">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="24cdb-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> uygulamanın kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="24cdb-171">Bunlar ana bilgisayar tarafından kurduktan olduğunda değil olarak yapılandırma sağlayıcıları DI, faydalanamaz.</span><span class="sxs-lookup"><span data-stu-id="24cdb-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="24cdb-172">Yapılandırma anahtarları, aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="24cdb-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="24cdb-173">Anahtarlar büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-173">Keys are case-insensitive.</span></span> <span data-ttu-id="24cdb-174">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="24cdb-175">Aynı veya farklı yapılandırma sağlayıcıları tarafından aynı anahtar için bir değer ayarlarsanız, anahtarda ayarlanan son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="24cdb-176">Hiyerarşik anahtarları</span><span class="sxs-lookup"><span data-stu-id="24cdb-176">Hierarchical keys</span></span>
  * <span data-ttu-id="24cdb-177">Yapılandırma API'sinin, iki nokta üst üste ayırıcı (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="24cdb-178">Ortam değişkenleri, iki nokta üst üste ayırıcı tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="24cdb-179">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve bir iki nokta üst üste dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="24cdb-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="24cdb-180">Azure anahtar Kasası'nda hiyerarşik tuşlarını `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="24cdb-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="24cdb-181">Gizli dizileri uygulama yapılandırma yüklendiğinde tireler iki nokta üst üste ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="24cdb-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="24cdb-183">Dizi bağlama açıklanan [bir dizi bir sınıfa Bağla](#bind-an-array-to-a-class) bölümü.</span><span class="sxs-lookup"><span data-stu-id="24cdb-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="24cdb-184">Yapılandırma değerleri aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="24cdb-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="24cdb-185">Dizeleri değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-185">Values are strings.</span></span>
* <span data-ttu-id="24cdb-186">Null değerler yapılandırmasında depolanmış veya nesnelere bağlı olamaz.</span><span class="sxs-lookup"><span data-stu-id="24cdb-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="24cdb-187">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="24cdb-187">Providers</span></span>

<span data-ttu-id="24cdb-188">Aşağıdaki tabloda, ASP.NET Core uygulamaları için kullanılabilir yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="24cdb-189">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24cdb-189">Provider</span></span> | <span data-ttu-id="24cdb-190">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="24cdb-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="24cdb-191">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="24cdb-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="24cdb-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24cdb-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="24cdb-193">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="24cdb-194">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-194">Command-line parameters</span></span> |
| [<span data-ttu-id="24cdb-195">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="24cdb-196">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="24cdb-196">Custom source</span></span> |
| [<span data-ttu-id="24cdb-197">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="24cdb-198">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-198">Environment variables</span></span> |
| [<span data-ttu-id="24cdb-199">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="24cdb-200">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="24cdb-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="24cdb-201">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="24cdb-202">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="24cdb-202">Directory files</span></span> |
| [<span data-ttu-id="24cdb-203">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="24cdb-204">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="24cdb-204">In-memory collections</span></span> |
| <span data-ttu-id="24cdb-205">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="24cdb-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="24cdb-206">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="24cdb-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="24cdb-207">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24cdb-207">Provider</span></span> | <span data-ttu-id="24cdb-208">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="24cdb-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="24cdb-209">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="24cdb-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="24cdb-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24cdb-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="24cdb-211">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="24cdb-212">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-212">Command-line parameters</span></span> |
| [<span data-ttu-id="24cdb-213">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="24cdb-214">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="24cdb-214">Custom source</span></span> |
| [<span data-ttu-id="24cdb-215">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="24cdb-216">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-216">Environment variables</span></span> |
| [<span data-ttu-id="24cdb-217">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="24cdb-218">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="24cdb-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="24cdb-219">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="24cdb-220">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="24cdb-220">In-memory collections</span></span> |
| <span data-ttu-id="24cdb-221">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="24cdb-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="24cdb-222">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="24cdb-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="24cdb-223">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24cdb-223">Provider</span></span> | <span data-ttu-id="24cdb-224">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="24cdb-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="24cdb-225">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="24cdb-226">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-226">Command-line parameters</span></span> |
| [<span data-ttu-id="24cdb-227">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="24cdb-228">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="24cdb-228">Custom source</span></span> |
| [<span data-ttu-id="24cdb-229">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="24cdb-230">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-230">Environment variables</span></span> |
| [<span data-ttu-id="24cdb-231">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="24cdb-232">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="24cdb-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="24cdb-233">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="24cdb-234">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="24cdb-234">In-memory collections</span></span> |
| <span data-ttu-id="24cdb-235">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="24cdb-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="24cdb-236">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="24cdb-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="24cdb-237">Yapılandırma sağlayıcıları başlatma sırasında belirttiğiniz sırayla yapılandırma kaynaklarını okunur.</span><span class="sxs-lookup"><span data-stu-id="24cdb-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="24cdb-238">Bu konuda açıklanan yapılandırma sağlayıcıları açıklanan alfabetik sırada, bunları kodunuzu düzenleme sırasını değil.</span><span class="sxs-lookup"><span data-stu-id="24cdb-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="24cdb-239">Temel yapılandırma kaynakları için önceliklerinizden uyacak şekilde yapılandırma sağlayıcıları kodunuzu sırası.</span><span class="sxs-lookup"><span data-stu-id="24cdb-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="24cdb-240">Yapılandırma sağlayıcıları, tipik bir dizisidir:</span><span class="sxs-lookup"><span data-stu-id="24cdb-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="24cdb-241">Dosyaları (*appsettings.json*, *appsettings. { Ortam} .json*burada `{Environment}` uygulamanın geçerli barındırma ortamı)</span><span class="sxs-lookup"><span data-stu-id="24cdb-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="24cdb-242">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="24cdb-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="24cdb-243">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki yalnızca)</span><span class="sxs-lookup"><span data-stu-id="24cdb-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="24cdb-244">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-244">Environment variables</span></span>
1. <span data-ttu-id="24cdb-245">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-245">Command-line arguments</span></span>

<span data-ttu-id="24cdb-246">Komut satırı yapılandırma sağlayıcısı yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılmak komut satırı bağımsız değişkenlerine izin vermek için sağlayıcıları serisinin son konumlandırmak için yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-247">Yeni bir başlattığınızda bu sağlayıcıları dizi yerine konur <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="24cdb-248">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="24cdb-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-249">Sağlayıcıları bu dizi için (ana değil) uygulaması ile oluşturulabilir bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ve bir çağrı, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> yönteminde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="24cdb-250">Yukarıdaki örnekte, ortam adı (`env.EnvironmentName`) ve uygulama derleme adı (`env.ApplicationName`) tarafından sağlanan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="24cdb-251">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="24cdb-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="24cdb-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="24cdb-253">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ne zaman bunlara ek olarak, uygulamanın yapılandırma sağlayıcıları belirtmek için Web ana bilgisayarını oluşturma eklenen tarafından otomatik olarak <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="24cdb-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="24cdb-254">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="24cdb-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Yapılandırma komut satırı bağımsız değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="24cdb-256">Komut satırı yapılandırmasını etkinleştirmek için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> genişletme yöntemi bir örneğinde çağrıldığında <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-257">`AddCommandLine` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="24cdb-258">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="24cdb-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="24cdb-259">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="24cdb-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="24cdb-260">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="24cdb-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="24cdb-261">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="24cdb-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="24cdb-262">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24cdb-262">Environment variables.</span></span>

<span data-ttu-id="24cdb-263">`CreateDefaultBuilder` Son komut satırı yapılandırma sağlayıcısı ekler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="24cdb-264">Çalışma zamanında geçirilen komut satırı bağımsız değişkenleri yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="24cdb-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="24cdb-265">`CreateDefaultBuilder` konak oluşturulduğunda işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="24cdb-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="24cdb-266">Bu nedenle, komut satırı yapılandırma etkinleştirildi tarafından `CreateDefaultBuilder` konak nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-267">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="24cdb-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="24cdb-268">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24cdb-269">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="24cdb-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="24cdb-270">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="24cdb-271">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="24cdb-272">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder` olduğunda `UseConfiguration` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="24cdb-273">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar çağırmak bir `ConfigurationBuilder` ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="24cdb-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="24cdb-274">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-275">Komut satırı yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24cdb-276">Son çalışma zamanında yapılandırması diğer yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenleri izin vermek için sağlayıcı çağırın.</span><span class="sxs-lookup"><span data-stu-id="24cdb-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="24cdb-277">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="24cdb-278">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="24cdb-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-279">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-280">1.x örnek uygulama çağrıları <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> üzerinde bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="24cdb-281">Proje dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="24cdb-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="24cdb-282">Bir komut satırı bağımsız değişken `dotnet run` komutu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="24cdb-283">Uygulama çalıştıktan sonra bir uygulamaya tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="24cdb-284">Çıkış için sağlanan yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çifti içeren gözlemleyin `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="24cdb-285">Arguments</span><span class="sxs-lookup"><span data-stu-id="24cdb-285">Arguments</span></span>

<span data-ttu-id="24cdb-286">Değeri bir eşittir işareti gelmelidir (`=`), veya bir önek anahtarı olmalıdır (`--` veya `/`) zaman değeri bir boşluk izler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="24cdb-287">Değer bir eşittir işareti kullanılıyorsa, null olabilir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="24cdb-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="24cdb-288">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="24cdb-288">Key prefix</span></span>               | <span data-ttu-id="24cdb-289">Örnek</span><span class="sxs-lookup"><span data-stu-id="24cdb-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="24cdb-290">Önek yok</span><span class="sxs-lookup"><span data-stu-id="24cdb-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="24cdb-291">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="24cdb-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="24cdb-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="24cdb-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="24cdb-293">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="24cdb-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="24cdb-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="24cdb-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="24cdb-295">Aynı komut içinde komut satırı bağımsız değişkeni bir eşittir işareti ile bir alanı kullanan anahtar-değer çiftleri kullanan anahtar-değer çiftleri karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="24cdb-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="24cdb-296">Örnek komutları:</span><span class="sxs-lookup"><span data-stu-id="24cdb-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="24cdb-297">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="24cdb-297">Switch mappings</span></span>

<span data-ttu-id="24cdb-298">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="24cdb-299">El ile yapı kurarken yapılandırmayla bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, anahtar değişiklik için bir sözlük sağlayabilir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="24cdb-300">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="24cdb-301">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) anahtar-değer çifti uygulamanın yapılandırmasını döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="24cdb-302">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="24cdb-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="24cdb-303">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="24cdb-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="24cdb-304">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="24cdb-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="24cdb-305">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-306">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="24cdb-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="24cdb-307">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="24cdb-308">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24cdb-309">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="24cdb-310">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="24cdb-311">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="24cdb-312">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24cdb-313">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="24cdb-314">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="24cdb-315">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="24cdb-316">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24cdb-316">Key</span></span>       | <span data-ttu-id="24cdb-317">Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="24cdb-318">Anahtar eşlemeli anahtarları uygulamayı başlatırken kullandıysanız, yapılandırma sözlüğü tarafından sağlanan anahtardaki yapılandırma değeri alır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="24cdb-319">Önceki komutu çalıştırdıktan sonra yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="24cdb-320">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24cdb-320">Key</span></span>               | <span data-ttu-id="24cdb-321">Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="24cdb-322">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="24cdb-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Yapılandırma ortam değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="24cdb-324">Ortam değişkenlerini yapılandırma etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24cdb-325">Ortam değişkenleri, iki nokta üst üste ayırıcı hiyerarşik anahtarlarla çalışırken (`:`) tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="24cdb-326">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve virgül ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="24cdb-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) ortam değişkenlerini yapılandırma Sağlayıcısı'nı kullanarak uygulama yapılandırması geçersiz kılabilirsiniz Azure portalında ortam değişkenlerini ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="24cdb-328">Daha fazla bilgi için [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="24cdb-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-329">`AddEnvironmentVariables` ortam değişkenlerini ön eki için otomatik olarak çağrılır `ASPNETCORE_` yeni başlatırken <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-329">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="24cdb-330">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="24cdb-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="24cdb-331">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="24cdb-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="24cdb-332">Uygulama yapılandırması çağırarak unprefixed ortam değişkenlerinden `AddEnvironmentVariables` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="24cdb-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="24cdb-333">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="24cdb-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="24cdb-334">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="24cdb-334">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="24cdb-335">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24cdb-335">Command-line arguments.</span></span>

<span data-ttu-id="24cdb-336">Yapılandırma kullanıcı parolalarının kurulduktan sonra ortam değişkenlerini yapılandırma sağlayıcısı denir ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="24cdb-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="24cdb-337">Bu konumda sağlayıcıya çağrı ortam değişkenlerini okuma yapılandırması tarafından kullanıcı parolalarını ayarlanmış geçersiz kılmak için çalışma zamanında sağlar ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="24cdb-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-338">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="24cdb-338">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="24cdb-339">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="24cdb-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="24cdb-340">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="24cdb-341">Çağrı `AddEnvironmentVariables` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="24cdb-342">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="24cdb-343">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="24cdb-343">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="24cdb-344">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-344">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-345">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-345">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="24cdb-346">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="24cdb-346">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-347">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-347">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-348">1.x örnek uygulama çağrıları `AddEnvironmentVariables` üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-348">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="24cdb-349">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24cdb-349">Run the sample app.</span></span> <span data-ttu-id="24cdb-350">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-350">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="24cdb-351">Çıkış ortam değişkeni için anahtar-değer çifti içeren gözlemleyin `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-351">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="24cdb-352">Değeri, uygulamanın çalıştığı, genellikle ortamın yansıtır `Development` yerel olarak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="24cdb-352">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="24cdb-353">Ortam değişkenlerini kısa uygulama tarafından işlenen listesini tutmak için aşağıdaki ile başlayan bu ortam değişkenleri uygulama filtreleri:</span><span class="sxs-lookup"><span data-stu-id="24cdb-353">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="24cdb-354">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="24cdb-354">ASPNETCORE_</span></span>
* <span data-ttu-id="24cdb-355">URL'leri</span><span class="sxs-lookup"><span data-stu-id="24cdb-355">urls</span></span>
* <span data-ttu-id="24cdb-356">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="24cdb-356">Logging</span></span>
* <span data-ttu-id="24cdb-357">ORTAM</span><span class="sxs-lookup"><span data-stu-id="24cdb-357">ENVIRONMENT</span></span>
* <span data-ttu-id="24cdb-358">contentRoot</span><span class="sxs-lookup"><span data-stu-id="24cdb-358">contentRoot</span></span>
* <span data-ttu-id="24cdb-359">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="24cdb-359">AllowedHosts</span></span>
* <span data-ttu-id="24cdb-360">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="24cdb-360">applicationName</span></span>
* <span data-ttu-id="24cdb-361">komut satırı</span><span class="sxs-lookup"><span data-stu-id="24cdb-361">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-362">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Pages/Index.cshtml.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="24cdb-362">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-363">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Controllers/HomeController.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="24cdb-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="24cdb-364">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="24cdb-364">Prefixes</span></span>

<span data-ttu-id="24cdb-365">Uygulamanın yapılandırma yüklendi ortam değişkenleri, bir ön ek sağladığında filtrelenir `AddEnvironmentVariables` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-365">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="24cdb-366">Örneğin, ortam değişkenlerini önek filtresi için `CUSTOM_`, yapılandırma sağlayıcısı için önek sağlayın:</span><span class="sxs-lookup"><span data-stu-id="24cdb-366">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="24cdb-367">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek yapılandırıldıktan devre dışı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-367">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-368">Statik kolaylık yöntemi `CreateDefaultBuilder` oluşturur bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> uygulamanın ana bilgisayar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="24cdb-368">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="24cdb-369">Zaman `WebHostBuilder` olan oluşturulan, kendi ana bilgisayar yapılandırması ön ekine sahip ortam değişkenleri içindeki bulduğu `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-369">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="24cdb-370">**Bağlantı dizesi ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="24cdb-370">**Connection string prefixes**</span></span>

<span data-ttu-id="24cdb-371">Yapılandırma API'si, dört bağlantı dizesi ortam değişkenleri için app ortamı için Azure bağlantı dizelerini yapılandırılmasıyla ilgili özel işleme kurallarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-371">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="24cdb-372">Önek yok belirtilirse tabloda gösterilen ön ekine sahip ortam değişkenleri uygulamaya yüklenir `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-372">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="24cdb-373">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="24cdb-373">Connection string prefix</span></span> | <span data-ttu-id="24cdb-374">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24cdb-374">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="24cdb-375">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24cdb-375">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="24cdb-376">MySQL</span><span class="sxs-lookup"><span data-stu-id="24cdb-376">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="24cdb-377">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="24cdb-377">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="24cdb-378">SQL Server</span><span class="sxs-lookup"><span data-stu-id="24cdb-378">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="24cdb-379">Ne zaman bir ortam değişkeni bulunur ve herhangi bir tabloda gösterilen dört öneklerini yapılandırmasını yüklendi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-379">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="24cdb-380">Yapılandırma anahtarı ortam değişkeni ön eki kaldırma ve yapılandırma anahtar bölümünü ekleyerek oluşturulur (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="24cdb-380">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="24cdb-381">Veritabanı bağlantı sağlayıcısı temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (dışında `CUSTOMCONNSTR_`, belirtilen sağlayıcı yok sahiptir).</span><span class="sxs-lookup"><span data-stu-id="24cdb-381">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="24cdb-382">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="24cdb-382">Environment variable key</span></span> | <span data-ttu-id="24cdb-383">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="24cdb-383">Converted configuration key</span></span> | <span data-ttu-id="24cdb-384">Sağlayıcı Yapılandırması girdisi</span><span class="sxs-lookup"><span data-stu-id="24cdb-384">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24cdb-385">Yapılandırma girişi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-385">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24cdb-386">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-386">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="24cdb-387">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="24cdb-387">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24cdb-388">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-388">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="24cdb-389">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="24cdb-389">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24cdb-390">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-390">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="24cdb-391">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="24cdb-391">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="24cdb-392">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-392">File Configuration Provider</span></span>

<span data-ttu-id="24cdb-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> yapılandırma dosya sisteminden yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="24cdb-394">Aşağıdaki yapılandırma sağlayıcıları için belirli dosya türleri ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-394">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="24cdb-395">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-395">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="24cdb-396">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-396">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="24cdb-397">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-397">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="24cdb-398">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-398">INI Configuration Provider</span></span>

<span data-ttu-id="24cdb-399"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Yapılandırma ını dosyası anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-399">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="24cdb-400">INI dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-400">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24cdb-401">İki nokta üst üste INI dosya yapılandırması bölüm ayırıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-401">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="24cdb-402">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="24cdb-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="24cdb-403">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-403">Whether the file is optional.</span></span>
* <span data-ttu-id="24cdb-404">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="24cdb-405"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-406">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="24cdb-406">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="24cdb-407">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-407">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="24cdb-408">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-408">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="24cdb-409">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-409">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-410">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-410">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="24cdb-411">Genel bir INI yapılandırma dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="24cdb-411">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="24cdb-412">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-412">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24cdb-413">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-413">section0:key0</span></span>
* <span data-ttu-id="24cdb-414">section0:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-414">section0:key1</span></span>
* <span data-ttu-id="24cdb-415">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="24cdb-415">section1:subsection:key</span></span>
* <span data-ttu-id="24cdb-416">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="24cdb-416">section2:subsection0:key</span></span>
* <span data-ttu-id="24cdb-417">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="24cdb-417">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="24cdb-418">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-418">JSON Configuration Provider</span></span>

<span data-ttu-id="24cdb-419"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Yapılandırma JSON dosyası anahtar-değer çiftlerinden çalışma zamanı sırasında yükler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-419">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="24cdb-420">JSON dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-420">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24cdb-421">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="24cdb-421">Overloads permit specifying:</span></span>

* <span data-ttu-id="24cdb-422">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-422">Whether the file is optional.</span></span>
* <span data-ttu-id="24cdb-423">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-423">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="24cdb-424"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-424">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-425">`AddJsonFile` Yeni bir başlattığınızda iki kez otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-425">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="24cdb-426">Yöntem yapılandırmasından yüklenemedi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-426">The method is called to load configuration from:</span></span>

* <span data-ttu-id="24cdb-427">*appSettings.JSON* &ndash; bu dosyayı ilk okuyun.</span><span class="sxs-lookup"><span data-stu-id="24cdb-427">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="24cdb-428">Dosyanın ortam sürümü tarafından sağlanan değerleri geçersiz kılabilir *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="24cdb-428">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="24cdb-429">*appSettings. {Ortamı} .json* &ndash; dosyanın ortam sürümünü temel alınarak yüklenir [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="24cdb-429">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="24cdb-430">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="24cdb-430">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="24cdb-431">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="24cdb-431">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="24cdb-432">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24cdb-432">Environment variables.</span></span>
* <span data-ttu-id="24cdb-433">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="24cdb-433">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="24cdb-434">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24cdb-434">Command-line arguments.</span></span>

<span data-ttu-id="24cdb-435">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24cdb-435">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="24cdb-436">Bu nedenle, yapılandırma kümesi kullanıcı parolaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri geçersiz kılma *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="24cdb-436">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-437">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırma dosyaları için dışında belirtmek için konak oluştururken *appsettings.json* ve *appsettings. { Ortam} .json*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-437">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="24cdb-438">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-438">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="24cdb-439">Ayrıca doğrudan çağırabilir miyim `AddJsonFile` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-439">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24cdb-440">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-440">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="24cdb-441">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-441">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-442">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-442">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="24cdb-443">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="24cdb-443">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-444">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` iki çağrıları içeren ana bilgisayar oluşturmak için `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-444">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="24cdb-445">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="24cdb-445">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-446">1.x örnek uygulama çağrıları `AddJsonFile` iki kez üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-446">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="24cdb-447">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="24cdb-447">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="24cdb-448">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24cdb-448">Run the sample app.</span></span> <span data-ttu-id="24cdb-449">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-449">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="24cdb-450">Çıkış ortamına bağlı olarak tabloda gösterilen yapılandırması için anahtar-değer çiftleri içeren gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="24cdb-450">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="24cdb-451">Günlük kaydı yapılandırması tuşlarını iki nokta üst üste (`:`) hiyerarşik ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="24cdb-451">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="24cdb-452">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24cdb-452">Key</span></span>                        | <span data-ttu-id="24cdb-453">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="24cdb-453">Development Value</span></span> | <span data-ttu-id="24cdb-454">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="24cdb-454">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="24cdb-455">Günlüğe kaydetme: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="24cdb-455">Logging:LogLevel:System</span></span>    | <span data-ttu-id="24cdb-456">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24cdb-456">Information</span></span>       | <span data-ttu-id="24cdb-457">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24cdb-457">Information</span></span>      |
| <span data-ttu-id="24cdb-458">Günlüğe kaydetme: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="24cdb-458">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="24cdb-459">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24cdb-459">Information</span></span>       | <span data-ttu-id="24cdb-460">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24cdb-460">Information</span></span>      |
| <span data-ttu-id="24cdb-461">Günlüğe kaydetme: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="24cdb-461">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="24cdb-462">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="24cdb-462">Debug</span></span>             | <span data-ttu-id="24cdb-463">Hata</span><span class="sxs-lookup"><span data-stu-id="24cdb-463">Error</span></span>            |
| <span data-ttu-id="24cdb-464">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="24cdb-464">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="24cdb-465">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-465">XML Configuration Provider</span></span>

<span data-ttu-id="24cdb-466"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Yapılandırma XML dosyası anahtar-değer çiftlerinin zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-466">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="24cdb-467">XML dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-467">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24cdb-468">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="24cdb-468">Overloads permit specifying:</span></span>

* <span data-ttu-id="24cdb-469">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-469">Whether the file is optional.</span></span>
* <span data-ttu-id="24cdb-470">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-470">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="24cdb-471"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-471">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="24cdb-472">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayıldı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-472">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="24cdb-473">Bir belge türü tanımı (DTD'nin) veya ad alanı dosyasında belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="24cdb-473">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-474">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="24cdb-474">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="24cdb-475">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-475">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="24cdb-476">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-476">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="24cdb-477">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-477">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-478">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-478">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="24cdb-479">XML yapılandırma dosyalarını, yinelenen bölümler için ayrı bir öğe adları kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="24cdb-479">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="24cdb-480">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-480">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24cdb-481">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-481">section0:key0</span></span>
* <span data-ttu-id="24cdb-482">section0:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-482">section0:key1</span></span>
* <span data-ttu-id="24cdb-483">section1:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-483">section1:key0</span></span>
* <span data-ttu-id="24cdb-484">section1:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-484">section1:key1</span></span>

<span data-ttu-id="24cdb-485">İş öğesi adının aynısını kullanın öğeleri, yinelenen `name` özniteliği öğeleri ayırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-485">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="24cdb-486">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-486">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24cdb-487">Bölüm: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-487">section:section0:key:key0</span></span>
* <span data-ttu-id="24cdb-488">Bölüm: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-488">section:section0:key:key1</span></span>
* <span data-ttu-id="24cdb-489">Bölüm: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-489">section:section1:key:key0</span></span>
* <span data-ttu-id="24cdb-490">Bölüm: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-490">section:section1:key:key1</span></span>

<span data-ttu-id="24cdb-491">Öznitelik değerlerini sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="24cdb-491">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="24cdb-492">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-492">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24cdb-493">Anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="24cdb-493">key:attribute</span></span>
* <span data-ttu-id="24cdb-494">Bölüm: anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="24cdb-494">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="24cdb-495">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-495">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="24cdb-496"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bir dizin dosyalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-496">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="24cdb-497">Anahtar dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-497">The key is the file name.</span></span> <span data-ttu-id="24cdb-498">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-498">The value contains the file's contents.</span></span> <span data-ttu-id="24cdb-499">Dosya başına anahtar yapılandırma sağlayıcısı Docker'da barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-499">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="24cdb-500">Dosya başına anahtar yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-500">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="24cdb-501">`directoryPath` Dosyalar için mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-501">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="24cdb-502">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="24cdb-502">Overloads permit specifying:</span></span>

* <span data-ttu-id="24cdb-503">Bir `Action<KeyPerFileConfigurationSource>` kaynağını yapılandırır temsilci.</span><span class="sxs-lookup"><span data-stu-id="24cdb-503">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="24cdb-504">Dizin isteğe bağlı olup olmadığını ve dizinin yolu.</span><span class="sxs-lookup"><span data-stu-id="24cdb-504">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="24cdb-505">Çift alt çizgi (`__`) dosya adları içinde yapılandırma anahtar sınırlayıcı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-505">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="24cdb-506">Örneğin, dosya adı `Logging__LogLevel__System` üretir yapılandırma anahtarı `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-506">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="24cdb-507">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="24cdb-507">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="24cdb-508">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-508">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="24cdb-509">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-509">Memory Configuration Provider</span></span>

<span data-ttu-id="24cdb-510"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Bir bellek içi koleksiyonu yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-510">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="24cdb-511">Bellek içi toplama yapılandırması etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-511">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24cdb-512">Yapılandırma sağlayıcısı ile başlatılabilir bir `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-512">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="24cdb-513">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="24cdb-513">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="24cdb-514">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-514">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="24cdb-515">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-515">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="24cdb-516">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="24cdb-516">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-517">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-517">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="24cdb-518">GetValue</span><span class="sxs-lookup"><span data-stu-id="24cdb-518">GetValue</span></span>

<span data-ttu-id="24cdb-519">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) bir değeri belirtilen bir anahtarla yapılandırmasından ayıklar ve onu belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="24cdb-519">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="24cdb-520">Aşırı yükleme anahtarı bulunmazsa, varsayılan bir değer sağlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-520">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="24cdb-521">Aşağıdaki örnek bir dize değeri yapılandırmasından anahtarıyla ayıklar `NumberKey`, değer olarak türleri bir `int`ve değeri değişkeninde depolar `intValue`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-521">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="24cdb-522">Varsa `NumberKey` yapılandırma anahtarlarını bulunamadığında `intValue` varsayılan değerini alır `99`:</span><span class="sxs-lookup"><span data-stu-id="24cdb-522">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="24cdb-523">GetSection, GetChildren ve var.</span><span class="sxs-lookup"><span data-stu-id="24cdb-523">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="24cdb-524">Aşağıdaki örneklerde için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="24cdb-524">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="24cdb-525">Dört anahtarları içeren bir çift alt bölümleri içerir, iki bölümlerde bulunur:</span><span class="sxs-lookup"><span data-stu-id="24cdb-525">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="24cdb-526">Dosya yapılandırma okuduğunuzda aşağıdaki benzersiz hiyerarşik anahtarları yapılandırma değerleri tutmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="24cdb-526">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="24cdb-527">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-527">section0:key0</span></span>
* <span data-ttu-id="24cdb-528">section0:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-528">section0:key1</span></span>
* <span data-ttu-id="24cdb-529">section1:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-529">section1:key0</span></span>
* <span data-ttu-id="24cdb-530">section1:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-530">section1:key1</span></span>
* <span data-ttu-id="24cdb-531">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-531">section2:subsection0:key0</span></span>
* <span data-ttu-id="24cdb-532">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-532">section2:subsection0:key1</span></span>
* <span data-ttu-id="24cdb-533">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="24cdb-533">section2:subsection1:key0</span></span>
* <span data-ttu-id="24cdb-534">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="24cdb-534">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="24cdb-535">GetSection</span><span class="sxs-lookup"><span data-stu-id="24cdb-535">GetSection</span></span>

<span data-ttu-id="24cdb-536">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) yapılandırma bölümüne belirtilen alt anahtarını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="24cdb-536">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="24cdb-537">Döndürülecek bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> yalnızca anahtar-değer çiftlerini içeren `section1`, çağrı `GetSection` ve bölüm adı verin:</span><span class="sxs-lookup"><span data-stu-id="24cdb-537">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="24cdb-538">Benzer şekilde, anahtarları değerlerini almak için `section2:subsection0`, çağrı `GetSection` ve bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="24cdb-538">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="24cdb-539">`GetSection` hiç dönmüyor `null`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-539">`GetSection` never returns `null`.</span></span> <span data-ttu-id="24cdb-540">Eşleşen bir bölümü olmadığından bulunamazsa, boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="24cdb-540">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="24cdb-541">GetChildren</span><span class="sxs-lookup"><span data-stu-id="24cdb-541">GetChildren</span></span>

<span data-ttu-id="24cdb-542">Bir çağrı [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) üzerinde `section2` alır bir `IEnumerable<IConfigurationSection>` içeren:</span><span class="sxs-lookup"><span data-stu-id="24cdb-542">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="24cdb-543">Var</span><span class="sxs-lookup"><span data-stu-id="24cdb-543">Exists</span></span>

<span data-ttu-id="24cdb-544">Kullanım [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) yapılandırma bölümü olup olmadığını belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="24cdb-544">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="24cdb-545">Belirtilen örnek veri `sectionExists` olduğu `false` olmadığından bir `section2:subsection2` yapılandırma verilerini bir bölümde.</span><span class="sxs-lookup"><span data-stu-id="24cdb-545">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="24cdb-546">Bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="24cdb-546">Bind to a class</span></span>

<span data-ttu-id="24cdb-547">Yapılandırma kullanarak ilgili ayar gruplarını temsil eden sınıflar için bağlanabilir *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="24cdb-547">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="24cdb-548">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-548">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="24cdb-549">Yapılandırma değerleri döndürülür dizeler olarak çağıran <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> oluşumu sağlayan [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesneleri.</span><span class="sxs-lookup"><span data-stu-id="24cdb-549">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="24cdb-550">Örnek uygulamayı içeren bir `Starship` modeli (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="24cdb-550">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-551">`starship` Bölümünü *starship.json* örnek uygulamayı yapılandırma yüklemek için JSON yapılandırma sağlayıcısı kullandığında yapılandırma dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="24cdb-551">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="24cdb-552">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="24cdb-552">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="24cdb-553">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24cdb-553">Key</span></span>                   | <span data-ttu-id="24cdb-554">Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-554">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="24cdb-555">starship: adı</span><span class="sxs-lookup"><span data-stu-id="24cdb-555">starship:name</span></span>         | <span data-ttu-id="24cdb-556">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="24cdb-556">USS Enterprise</span></span>                                    |
| <span data-ttu-id="24cdb-557">starship: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="24cdb-557">starship:registry</span></span>     | <span data-ttu-id="24cdb-558">NCC 1701</span><span class="sxs-lookup"><span data-stu-id="24cdb-558">NCC-1701</span></span>                                          |
| <span data-ttu-id="24cdb-559">starship: sınıfı</span><span class="sxs-lookup"><span data-stu-id="24cdb-559">starship:class</span></span>        | <span data-ttu-id="24cdb-560">Anayasa</span><span class="sxs-lookup"><span data-stu-id="24cdb-560">Constitution</span></span>                                      |
| <span data-ttu-id="24cdb-561">starship: uzunluğu</span><span class="sxs-lookup"><span data-stu-id="24cdb-561">starship:length</span></span>       | <span data-ttu-id="24cdb-562">304.8</span><span class="sxs-lookup"><span data-stu-id="24cdb-562">304.8</span></span>                                             |
| <span data-ttu-id="24cdb-563">starship: yetkilendirilen</span><span class="sxs-lookup"><span data-stu-id="24cdb-563">starship:commissioned</span></span> | <span data-ttu-id="24cdb-564">False</span><span class="sxs-lookup"><span data-stu-id="24cdb-564">False</span></span>                                             |
| <span data-ttu-id="24cdb-565">Ticari marka</span><span class="sxs-lookup"><span data-stu-id="24cdb-565">trademark</span></span>             | <span data-ttu-id="24cdb-566">Paramount resimleri Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="24cdb-566">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="24cdb-567">Örnek Uygulama çağrıları `GetSection` ile `starship` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-567">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="24cdb-568">`starship` Anahtar-değer çiftleridir yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="24cdb-568">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="24cdb-569">`Bind` Bir örneğini geçirerek Altbölüm yöntemi çağrıldığında `Starship` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-569">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="24cdb-570">Örnek değerleri bağlandıktan sonra işleme için bir özellik için örneği atanır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-570">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="24cdb-571">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="24cdb-571">Bind to an object graph</span></span>

<span data-ttu-id="24cdb-572"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Tüm POCO Nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-572"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="24cdb-573">Örnek içeren bir `TvShow` olan nesne grafiğini içeren model `Metadata` ve `Actors` sınıfları (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="24cdb-573">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-574">Örnek uygulamanın bir *tvshow.xml* yapılandırma verilerini içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="24cdb-574">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="24cdb-575">Yapılandırma tüm bağlı `TvShow` Nesne grafiği ile `Bind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="24cdb-575">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="24cdb-576">İlişkili örneği, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-576">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="24cdb-577">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="24cdb-577">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="24cdb-578">`Get<T>` kullanmaktan daha kullanışlı olan `Bind`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-578">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="24cdb-579">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle birlikte işlemek için kullanılan özellik doğrudan atanan bağlı örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="24cdb-579">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="24cdb-580">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="24cdb-580">Bind an array to a class</span></span>

<span data-ttu-id="24cdb-581">*Örnek uygulama, bu bölümde açıklanan kavramları göstermektedir.*</span><span class="sxs-lookup"><span data-stu-id="24cdb-581">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="24cdb-582"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="24cdb-582">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="24cdb-583">Sayısal bir anahtar kesimi sunan herhangi bir dizi biçimi (`:0:`, `:1:`, &hellip; `:{n}:`) dizisi bağlama POCO sınıfı dizisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-583">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="24cdb-584">Bağlama, kural olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-584">Binding is provided by convention.</span></span> <span data-ttu-id="24cdb-585">Özel yapılandırma sağlayıcıları dizi bağlama uygulamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-585">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="24cdb-586">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="24cdb-586">**In-memory array processing**</span></span>

<span data-ttu-id="24cdb-587">Yapılandırma anahtarları ve değerleri aşağıdaki tabloda gösterilen göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="24cdb-587">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="24cdb-588">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24cdb-588">Key</span></span>             | <span data-ttu-id="24cdb-589">Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-589">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="24cdb-590">dizi: girişler: 0</span><span class="sxs-lookup"><span data-stu-id="24cdb-590">array:entries:0</span></span> | <span data-ttu-id="24cdb-591">value0</span><span class="sxs-lookup"><span data-stu-id="24cdb-591">value0</span></span> |
| <span data-ttu-id="24cdb-592">dizi: girişler: 1</span><span class="sxs-lookup"><span data-stu-id="24cdb-592">array:entries:1</span></span> | <span data-ttu-id="24cdb-593">Değer1</span><span class="sxs-lookup"><span data-stu-id="24cdb-593">value1</span></span> |
| <span data-ttu-id="24cdb-594">dizi: girişler: 2</span><span class="sxs-lookup"><span data-stu-id="24cdb-594">array:entries:2</span></span> | <span data-ttu-id="24cdb-595">Value2</span><span class="sxs-lookup"><span data-stu-id="24cdb-595">value2</span></span> |
| <span data-ttu-id="24cdb-596">dizi: girişler: 4</span><span class="sxs-lookup"><span data-stu-id="24cdb-596">array:entries:4</span></span> | <span data-ttu-id="24cdb-597">Değer4</span><span class="sxs-lookup"><span data-stu-id="24cdb-597">value4</span></span> |
| <span data-ttu-id="24cdb-598">dizi: girişler: 5</span><span class="sxs-lookup"><span data-stu-id="24cdb-598">array:entries:5</span></span> | <span data-ttu-id="24cdb-599">Değeri5</span><span class="sxs-lookup"><span data-stu-id="24cdb-599">value5</span></span> |

<span data-ttu-id="24cdb-600">Bellek yapılandırma sağlayıcısı kullanan örnek uygulamasında, bu anahtarların ve değerlerin yüklenir:</span><span class="sxs-lookup"><span data-stu-id="24cdb-600">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="24cdb-601">Dizi dizini için bir değer atlar &num;3.</span><span class="sxs-lookup"><span data-stu-id="24cdb-601">The array skips a value for index &num;3.</span></span> <span data-ttu-id="24cdb-602">Yapılandırma bağlayıcı temizleyin, bu dizi için bir nesne bağlamanın sonucunu gösterilmiştir birazdan haline geldikten bağlama null değerler veya null girişler bağımlı nesneleri oluşturma yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-602">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="24cdb-603">Örnek uygulamada, ilişkili yapılandırma verilerini tutmak bir POCO sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="24cdb-603">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-604">Yapılandırma verilerini nesnesine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-604">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="24cdb-605">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) söz dizimi de kullanılabilir, daha küçük kod sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="24cdb-605">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="24cdb-606">İlişkili nesne, örneği `ArrayExample`, yapılandırmasından dizisi verileri alır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-606">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="24cdb-607">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="24cdb-607">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="24cdb-608">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-608">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="24cdb-609">0</span><span class="sxs-lookup"><span data-stu-id="24cdb-609">0</span></span>                            | <span data-ttu-id="24cdb-610">value0</span><span class="sxs-lookup"><span data-stu-id="24cdb-610">value0</span></span>                       |
| <span data-ttu-id="24cdb-611">1.</span><span class="sxs-lookup"><span data-stu-id="24cdb-611">1</span></span>                            | <span data-ttu-id="24cdb-612">Değer1</span><span class="sxs-lookup"><span data-stu-id="24cdb-612">value1</span></span>                       |
| <span data-ttu-id="24cdb-613">2</span><span class="sxs-lookup"><span data-stu-id="24cdb-613">2</span></span>                            | <span data-ttu-id="24cdb-614">Value2</span><span class="sxs-lookup"><span data-stu-id="24cdb-614">value2</span></span>                       |
| <span data-ttu-id="24cdb-615">3</span><span class="sxs-lookup"><span data-stu-id="24cdb-615">3</span></span>                            | <span data-ttu-id="24cdb-616">Değer4</span><span class="sxs-lookup"><span data-stu-id="24cdb-616">value4</span></span>                       |
| <span data-ttu-id="24cdb-617">4</span><span class="sxs-lookup"><span data-stu-id="24cdb-617">4</span></span>                            | <span data-ttu-id="24cdb-618">Değeri5</span><span class="sxs-lookup"><span data-stu-id="24cdb-618">value5</span></span>                       |

<span data-ttu-id="24cdb-619">Dizin &num;3'te ilişkili nesne için yapılandırma verilerini tutan `array:4` yapılandırma anahtarı ve değeri `value4`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-619">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="24cdb-620">Bir diziyi içeren yapılandırma verilerini bağlandığında, dizi dizinleri yapılandırma anahtarları yalnızca nesne oluşturma sırasında yapılandırma verileri yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-620">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="24cdb-621">Yapılandırma verilerinde bir null değer tutulamıyor ve yapılandırma anahtarları bir dizide bir veya daha fazla dizinlerini atladığında null değerli bir girişi bir bağımlı nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="24cdb-621">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="24cdb-622">Eksik yapılandırma öğesi için dizin &num;bağlama önce 3 sağlanabilir `ArrayExample` örneği üretir yapılandırma doğru anahtar-değer çifti herhangi bir yapılandırma sağlayıcısı tarafından.</span><span class="sxs-lookup"><span data-stu-id="24cdb-622">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="24cdb-623">Ek bir JSON yapılandırma sağlayıcısı eksik anahtar-değer çifti ile örneği varsa `ArrayExample.Entries` yapılandırmanın tamamı dizisi ile eşleşen:</span><span class="sxs-lookup"><span data-stu-id="24cdb-623">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="24cdb-624">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-624">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="24cdb-625">İçinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="24cdb-625">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="24cdb-626">İçinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="24cdb-626">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="24cdb-627">Tabloda belirtilen anahtar-değer çifti yapılandırma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-627">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="24cdb-628">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24cdb-628">Key</span></span>             | <span data-ttu-id="24cdb-629">Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-629">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="24cdb-630">dizi: girişler: 3</span><span class="sxs-lookup"><span data-stu-id="24cdb-630">array:entries:3</span></span> | <span data-ttu-id="24cdb-631">Değeri3</span><span class="sxs-lookup"><span data-stu-id="24cdb-631">value3</span></span> |

<span data-ttu-id="24cdb-632">Varsa `ArrayExample` sınıf örneği bağlı dizin için giriş JSON yapılandırma sağlayıcısı içerir sonra &num;3 `ArrayExample.Entries` dizi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="24cdb-632">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="24cdb-633">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="24cdb-633">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="24cdb-634">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-634">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="24cdb-635">0</span><span class="sxs-lookup"><span data-stu-id="24cdb-635">0</span></span>                            | <span data-ttu-id="24cdb-636">value0</span><span class="sxs-lookup"><span data-stu-id="24cdb-636">value0</span></span>                       |
| <span data-ttu-id="24cdb-637">1.</span><span class="sxs-lookup"><span data-stu-id="24cdb-637">1</span></span>                            | <span data-ttu-id="24cdb-638">Değer1</span><span class="sxs-lookup"><span data-stu-id="24cdb-638">value1</span></span>                       |
| <span data-ttu-id="24cdb-639">2</span><span class="sxs-lookup"><span data-stu-id="24cdb-639">2</span></span>                            | <span data-ttu-id="24cdb-640">Value2</span><span class="sxs-lookup"><span data-stu-id="24cdb-640">value2</span></span>                       |
| <span data-ttu-id="24cdb-641">3</span><span class="sxs-lookup"><span data-stu-id="24cdb-641">3</span></span>                            | <span data-ttu-id="24cdb-642">Değeri3</span><span class="sxs-lookup"><span data-stu-id="24cdb-642">value3</span></span>                       |
| <span data-ttu-id="24cdb-643">4</span><span class="sxs-lookup"><span data-stu-id="24cdb-643">4</span></span>                            | <span data-ttu-id="24cdb-644">Değer4</span><span class="sxs-lookup"><span data-stu-id="24cdb-644">value4</span></span>                       |
| <span data-ttu-id="24cdb-645">5</span><span class="sxs-lookup"><span data-stu-id="24cdb-645">5</span></span>                            | <span data-ttu-id="24cdb-646">Değeri5</span><span class="sxs-lookup"><span data-stu-id="24cdb-646">value5</span></span>                       |

<span data-ttu-id="24cdb-647">**JSON dizisi işleme**</span><span class="sxs-lookup"><span data-stu-id="24cdb-647">**JSON array processing**</span></span>

<span data-ttu-id="24cdb-648">Bir JSON dosyası bir dizi varsa, dizi öğeleri bölümünde sıfır tabanlı dizine sahip için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24cdb-648">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="24cdb-649">Aşağıdaki yapılandırma dosyasında `subsection` dizisi:</span><span class="sxs-lookup"><span data-stu-id="24cdb-649">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="24cdb-650">JSON yapılandırma sağlayıcısı, aşağıdaki anahtar-değer çiftlerine yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="24cdb-650">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="24cdb-651">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24cdb-651">Key</span></span>                     | <span data-ttu-id="24cdb-652">Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-652">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="24cdb-653">json_array:Key</span><span class="sxs-lookup"><span data-stu-id="24cdb-653">json_array:key</span></span>          | <span data-ttu-id="24cdb-654">Değera</span><span class="sxs-lookup"><span data-stu-id="24cdb-654">valueA</span></span> |
| <span data-ttu-id="24cdb-655">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="24cdb-655">json_array:subsection:0</span></span> | <span data-ttu-id="24cdb-656">Değerb</span><span class="sxs-lookup"><span data-stu-id="24cdb-656">valueB</span></span> |
| <span data-ttu-id="24cdb-657">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="24cdb-657">json_array:subsection:1</span></span> | <span data-ttu-id="24cdb-658">valueC</span><span class="sxs-lookup"><span data-stu-id="24cdb-658">valueC</span></span> |
| <span data-ttu-id="24cdb-659">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="24cdb-659">json_array:subsection:2</span></span> | <span data-ttu-id="24cdb-660">Değerli</span><span class="sxs-lookup"><span data-stu-id="24cdb-660">valueD</span></span> |

<span data-ttu-id="24cdb-661">Örnek uygulamada, aşağıdaki POCO sınıfı yapılandırma anahtar-değer çiftleri bağlamak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="24cdb-661">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-662">Bağlama sonra `JsonArrayExample.Key` değerine `valueA`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-662">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="24cdb-663">Alt değerleri POCO dizi özelliğinde depolanır `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-663">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="24cdb-664">`JsonArrayExample.Subsection` Dizin</span><span class="sxs-lookup"><span data-stu-id="24cdb-664">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="24cdb-665">`JsonArrayExample.Subsection` Değer</span><span class="sxs-lookup"><span data-stu-id="24cdb-665">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="24cdb-666">0</span><span class="sxs-lookup"><span data-stu-id="24cdb-666">0</span></span>                                   | <span data-ttu-id="24cdb-667">Değerb</span><span class="sxs-lookup"><span data-stu-id="24cdb-667">valueB</span></span>                              |
| <span data-ttu-id="24cdb-668">1.</span><span class="sxs-lookup"><span data-stu-id="24cdb-668">1</span></span>                                   | <span data-ttu-id="24cdb-669">valueC</span><span class="sxs-lookup"><span data-stu-id="24cdb-669">valueC</span></span>                              |
| <span data-ttu-id="24cdb-670">2</span><span class="sxs-lookup"><span data-stu-id="24cdb-670">2</span></span>                                   | <span data-ttu-id="24cdb-671">Değerli</span><span class="sxs-lookup"><span data-stu-id="24cdb-671">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="24cdb-672">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24cdb-672">Custom configuration provider</span></span>

<span data-ttu-id="24cdb-673">Örnek uygulamayı yapılandırma anahtar-değer çiftleri kullanarak bir veritabanını okuyan bir temel yapılandırma sağlayıcısı oluşturma gösterilmektedir [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="24cdb-673">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="24cdb-674">Sağlayıcı, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="24cdb-674">The provider has the following characteristics:</span></span>

* <span data-ttu-id="24cdb-675">EF bellek içi veritabanına tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-675">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="24cdb-676">Bir bağlantı dizesi gerektiren bir veritabanı kullanmak için ikincil uygulama `ConfigurationBuilder` başka bir yapılandırma sağlayıcısı bağlantı dizesinden sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="24cdb-676">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="24cdb-677">Sağlayıcı bir veritabanı tablosu, başlangıç yapılandırmasını içine okur.</span><span class="sxs-lookup"><span data-stu-id="24cdb-677">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="24cdb-678">Sağlayıcı anahtarı başına temelinde veritabanını sorgulamak değil.</span><span class="sxs-lookup"><span data-stu-id="24cdb-678">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="24cdb-679">Uygulama başlatılır sahip olduktan sonra uygulamanın yapılandırma üzerinde hiçbir etkisi kadar veritabanını güncelleme yeniden üzerinde değişiklik uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-679">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="24cdb-680">Tanımlayan bir `EFConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık.</span><span class="sxs-lookup"><span data-stu-id="24cdb-680">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="24cdb-681">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-681">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-682">Ekleme bir `EFConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="24cdb-682">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="24cdb-683">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-683">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-684">Uygulayan bir sınıf oluşturma <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-684">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="24cdb-685">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-685">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-686">Özel yapılandırma sağlayıcısını devralarak oluşturma <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-686">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="24cdb-687">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır.</span><span class="sxs-lookup"><span data-stu-id="24cdb-687">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="24cdb-688">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-688">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-689">Bir `AddEFConfiguration` genişletme yöntemi izin veren yapılandırması kaynağına ekleme bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-689">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="24cdb-690">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-690">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24cdb-691">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigurationProvider` içinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="24cdb-691">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="24cdb-692">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24cdb-692">Access configuration during startup</span></span>

<span data-ttu-id="24cdb-693">Ekleme `IConfiguration` içine `Startup` oluşturucuya erişim yapılandırma değerlerini `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24cdb-693">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="24cdb-694">Erişim yapılandırmaya `Startup.Configure`, ya da ekleme `IConfiguration` doğrudan yöntem veya oluşturucu örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="24cdb-694">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="24cdb-695">Başlangıç kullanışlı yöntemler kullanarak yapılandırma erişme ilişkin bir örnek için bkz [uygulama başlatma: Yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="24cdb-695">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="24cdb-696">Erişim yapılandırmasında bir Razor sayfaları sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="24cdb-696">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="24cdb-697">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](xref:Microsoft.Extensions.Configuration) ve ekleme <xref:Microsoft.Extensions.Configuration.IConfiguration> sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="24cdb-697">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="24cdb-698">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="24cdb-698">In a Razor Pages page:</span></span>

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

<span data-ttu-id="24cdb-699">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="24cdb-699">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="24cdb-700">Dış bütünleştirilmiş koddan Yapılandırması Ekle</span><span class="sxs-lookup"><span data-stu-id="24cdb-700">Add configuration from an external assembly</span></span>

<span data-ttu-id="24cdb-701">Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="24cdb-701">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="24cdb-702">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="24cdb-702">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24cdb-703">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="24cdb-703">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="24cdb-704">Microsoft yapılandırma hakkında ayrıntılı bir inceleme</span><span class="sxs-lookup"><span data-stu-id="24cdb-704">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
