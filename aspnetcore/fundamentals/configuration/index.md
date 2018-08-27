---
title: ASP.NET core'da yapılandırma
author: guardrex
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: a0c57e75b28bc7c5590d20a8fa59b00b6bb9af4e
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927884"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="fc863-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fc863-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="fc863-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fc863-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fc863-105">ASP.NET core'da uygulama yapılandırması tarafından kurulan anahtar-değer çiftleri temel *yapılandırma sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="fc863-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="fc863-106">Yapılandırma sağlayıcıları, yapılandırma kaynaklarını çeşitli anahtar-değer çiftlerine yapılandırma verilerini okuyun:</span><span class="sxs-lookup"><span data-stu-id="fc863-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="fc863-107">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="fc863-107">Azure Key Vault</span></span>
* <span data-ttu-id="fc863-108">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-108">Command-line arguments</span></span>
* <span data-ttu-id="fc863-109">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="fc863-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="fc863-110">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="fc863-110">Directory files</span></span>
* <span data-ttu-id="fc863-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-111">Environment variables</span></span>
* <span data-ttu-id="fc863-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="fc863-112">In-memory .NET objects</span></span>
* <span data-ttu-id="fc863-113">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="fc863-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="fc863-114">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="fc863-114">Azure Key Vault</span></span>
* <span data-ttu-id="fc863-115">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-115">Command-line arguments</span></span>
* <span data-ttu-id="fc863-116">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="fc863-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="fc863-117">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-117">Environment variables</span></span>
* <span data-ttu-id="fc863-118">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="fc863-118">In-memory .NET objects</span></span>
* <span data-ttu-id="fc863-119">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="fc863-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="fc863-120">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-120">Command-line arguments</span></span>
* <span data-ttu-id="fc863-121">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="fc863-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="fc863-122">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-122">Environment variables</span></span>
* <span data-ttu-id="fc863-123">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="fc863-123">In-memory .NET objects</span></span>
* <span data-ttu-id="fc863-124">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="fc863-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="fc863-125">*Seçenekleri deseni* bu konuda açıklanan yapılandırma kavramları bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="fc863-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="fc863-126">Seçenekler, ilgili ayar gruplarını temsil etmek için sınıflar kullanır.</span><span class="sxs-lookup"><span data-stu-id="fc863-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="fc863-127">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="fc863-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="fc863-128">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fc863-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fc863-129">Bu konudaki sağladığınız örnekleri temel kullanır:</span><span class="sxs-lookup"><span data-stu-id="fc863-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="fc863-130">Temel yol ile uygulama ayarı <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="fc863-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="fc863-131">`SetBasePath` başvurarak bir uygulama için kullanılabilir hale getirileceğini [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.</span><span class="sxs-lookup"><span data-stu-id="fc863-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="fc863-132">Yapılandırma dosyalarıyla bölümlerini çözümleme <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="fc863-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="fc863-133">`GetSection` başvurarak bir uygulama için kullanılabilir hale getirileceğini [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.</span><span class="sxs-lookup"><span data-stu-id="fc863-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="fc863-134">.NET için bağlama yapılandırması sınıfları ile <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> ve [alma&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="fc863-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="fc863-135">`Bind` ve `Get<T>` başvurarak bir uygulama için kullanılabilir yapılır [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.</span><span class="sxs-lookup"><span data-stu-id="fc863-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="fc863-136">`Get<T>` ASP.NET Core 1.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fc863-137">Bu üç paketi içinde yer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="fc863-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fc863-138">Bu üç paketi içinde yer [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="fc863-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="fc863-139">Uygulama yapılandırması barındırın</span><span class="sxs-lookup"><span data-stu-id="fc863-139">Host vs. app configuration</span></span>

<span data-ttu-id="fc863-140">Uygulama yapılandırılmış ve başlatıldı, önce bir *konak* başlatılan ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="fc863-141">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="fc863-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="fc863-142">Bu konuda açıklanan yapılandırma sağlayıcıları kullanarak, hem uygulama hem de konak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="fc863-143">Ana bilgisayar yapılandırma anahtar-değer çiftleri uygulamanın genel yapılandırmasının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="fc863-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="fc863-144">Yapılandırma sağlayıcıları konak oluşturulduğunda kullanılan yapılandırma ve yapılandırma kaynaklarını nasıl etkileyeceğini nasıl barındırmak daha fazla bilgi için bkz: <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="fc863-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="fc863-145">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="fc863-145">Security</span></span>

<span data-ttu-id="fc863-146">Aşağıdaki en iyi benimseme:</span><span class="sxs-lookup"><span data-stu-id="fc863-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="fc863-147">Hiçbir zaman parolaları ve diğer hassas verileri yapılandırma sağlayıcısı kodda veya düz metin yapılandırma dosyalarında depolayın.</span><span class="sxs-lookup"><span data-stu-id="fc863-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="fc863-148">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="fc863-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="fc863-149">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="fc863-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="fc863-150">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [gizli dizi Yöneticisi ile geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) (depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. hassas verileri).</span><span class="sxs-lookup"><span data-stu-id="fc863-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="fc863-151">Gizli dizi Yöneticisi'ni dosya yapılandırma sağlayıcısı bir JSON dosyası yerel sistemdeki kullanıcı gizli dizileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="fc863-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="fc863-152">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="fc863-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="fc863-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) uygulama gizli anahtarlarının güvenli bir şekilde depolanması için bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="fc863-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="fc863-154">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="fc863-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="fc863-155">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="fc863-155">Hierarchical configuration data</span></span>

<span data-ttu-id="fc863-156">Yapılandırma API configuration anahtarlarında bir sınırlayıcı kullanarak hiyerarşik veri düzleştirme tarafından hiyerarşik yapılandırma verileri koruma özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="fc863-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="fc863-157">Aşağıdaki JSON dosyasında iki bölüm yapılandırılmış bir hiyerarşide dört anahtarları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="fc863-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="fc863-158">Dosya yapılandırma okuduğunuzda benzersiz anahtarlar özgün hiyerarşik veri yapısını yapılandırma kaynağı korumak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fc863-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="fc863-159">Bölümler ve anahtarlar ile bir iki nokta üst üste kullanımını düzleştirilir (`:`) özgün yapıyı korumak için:</span><span class="sxs-lookup"><span data-stu-id="fc863-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="fc863-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-160">section0:key0</span></span>
* <span data-ttu-id="fc863-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-161">section0:key1</span></span>
* <span data-ttu-id="fc863-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-162">section1:key0</span></span>
* <span data-ttu-id="fc863-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-163">section1:key1</span></span>

<span data-ttu-id="fc863-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemlerdir bölümler ve yapılandırma verilerini bir bölümde alt yalıtmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="fc863-165">Bu yöntem daha sonra açıklanmıştır [GetSection GetChildren ve Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="fc863-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="fc863-166">Kurallar</span><span class="sxs-lookup"><span data-stu-id="fc863-166">Conventions</span></span>

<span data-ttu-id="fc863-167">Uygulama başlangıcında, yapılandırma kaynaklarını yapılandırma sağlayıcıları belirttiğiniz sırayla okunur.</span><span class="sxs-lookup"><span data-stu-id="fc863-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="fc863-168">Dosya yapılandırma sağlayıcıları uygulama başlangıcından sonra bir temel alınan ayarları dosyası değiştirildiğinde yapılandırmayı yeniden yükle seçeneğine sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="fc863-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="fc863-169">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="fc863-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="fc863-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> uygulamanın kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="fc863-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="fc863-171">Bunlar ana bilgisayar tarafından kurduktan olduğunda değil olarak yapılandırma sağlayıcıları DI, faydalanamaz.</span><span class="sxs-lookup"><span data-stu-id="fc863-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="fc863-172">Yapılandırma anahtarları, aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="fc863-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="fc863-173">Anahtarlar büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="fc863-173">Keys are case-insensitive.</span></span> <span data-ttu-id="fc863-174">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="fc863-175">Aynı veya farklı yapılandırma sağlayıcıları tarafından aynı anahtar için bir değer ayarlarsanız, anahtarda ayarlanan son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="fc863-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="fc863-176">Hiyerarşik anahtarları</span><span class="sxs-lookup"><span data-stu-id="fc863-176">Hierarchical keys</span></span>
  * <span data-ttu-id="fc863-177">Yapılandırma API'sinin, iki nokta üst üste ayırıcı (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc863-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="fc863-178">Ortam değişkenleri, iki nokta üst üste ayırıcı tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="fc863-179">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve bir iki nokta üst üste dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="fc863-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="fc863-180">Azure anahtar Kasası'nda hiyerarşik tuşlarını `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="fc863-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="fc863-181">Gizli dizileri uygulama yapılandırma yüklendiğinde tireler iki nokta üst üste ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc863-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="fc863-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="fc863-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="fc863-183">Dizi bağlama açıklanan [bir dizi bir sınıfa Bağla](#bind-an-array-to-a-class) bölümü.</span><span class="sxs-lookup"><span data-stu-id="fc863-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="fc863-184">Yapılandırma değerleri aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="fc863-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="fc863-185">Dizeleri değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="fc863-185">Values are strings.</span></span>
* <span data-ttu-id="fc863-186">Null değerler yapılandırmasında depolanmış veya nesnelere bağlı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fc863-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="fc863-187">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="fc863-187">Providers</span></span>

<span data-ttu-id="fc863-188">Aşağıdaki tabloda, ASP.NET Core uygulamaları için kullanılabilir yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fc863-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="fc863-189">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="fc863-189">Provider</span></span> | <span data-ttu-id="fc863-190">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="fc863-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="fc863-191">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="fc863-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="fc863-192">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="fc863-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="fc863-193">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="fc863-194">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="fc863-194">Command-line parameters</span></span> |
| [<span data-ttu-id="fc863-195">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="fc863-196">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="fc863-196">Custom source</span></span> |
| [<span data-ttu-id="fc863-197">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="fc863-198">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-198">Environment variables</span></span> |
| [<span data-ttu-id="fc863-199">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="fc863-200">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="fc863-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="fc863-201">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="fc863-202">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="fc863-202">Directory files</span></span> |
| [<span data-ttu-id="fc863-203">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="fc863-204">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="fc863-204">In-memory collections</span></span> |
| <span data-ttu-id="fc863-205">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="fc863-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="fc863-206">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="fc863-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="fc863-207">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="fc863-207">Provider</span></span> | <span data-ttu-id="fc863-208">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="fc863-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="fc863-209">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="fc863-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="fc863-210">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="fc863-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="fc863-211">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="fc863-212">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="fc863-212">Command-line parameters</span></span> |
| [<span data-ttu-id="fc863-213">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="fc863-214">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="fc863-214">Custom source</span></span> |
| [<span data-ttu-id="fc863-215">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="fc863-216">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-216">Environment variables</span></span> |
| [<span data-ttu-id="fc863-217">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="fc863-218">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="fc863-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="fc863-219">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="fc863-220">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="fc863-220">In-memory collections</span></span> |
| <span data-ttu-id="fc863-221">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="fc863-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="fc863-222">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="fc863-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="fc863-223">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="fc863-223">Provider</span></span> | <span data-ttu-id="fc863-224">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="fc863-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="fc863-225">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="fc863-226">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="fc863-226">Command-line parameters</span></span> |
| [<span data-ttu-id="fc863-227">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="fc863-228">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="fc863-228">Custom source</span></span> |
| [<span data-ttu-id="fc863-229">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="fc863-230">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-230">Environment variables</span></span> |
| [<span data-ttu-id="fc863-231">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="fc863-232">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="fc863-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="fc863-233">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="fc863-234">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="fc863-234">In-memory collections</span></span> |
| <span data-ttu-id="fc863-235">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="fc863-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="fc863-236">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="fc863-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="fc863-237">Yapılandırma sağlayıcıları başlatma sırasında belirttiğiniz sırayla yapılandırma kaynaklarını okunur.</span><span class="sxs-lookup"><span data-stu-id="fc863-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="fc863-238">Bu konuda açıklanan yapılandırma sağlayıcıları açıklanan alfabetik sırada, bunları kodunuzu düzenleme sırasını değil.</span><span class="sxs-lookup"><span data-stu-id="fc863-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="fc863-239">Temel yapılandırma kaynakları için önceliklerinizden uyacak şekilde yapılandırma sağlayıcıları kodunuzu sırası.</span><span class="sxs-lookup"><span data-stu-id="fc863-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="fc863-240">Yapılandırma sağlayıcıları, tipik bir dizisidir:</span><span class="sxs-lookup"><span data-stu-id="fc863-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="fc863-241">Dosyaları (*appsettings.json*, *appsettings.&lt; Ortam&gt;.json*burada `<Environment>` uygulamanın geçerli barındırma ortamı)</span><span class="sxs-lookup"><span data-stu-id="fc863-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="fc863-242">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki yalnızca)</span><span class="sxs-lookup"><span data-stu-id="fc863-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="fc863-243">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-243">Environment variables</span></span>
1. <span data-ttu-id="fc863-244">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="fc863-244">Command-line arguments</span></span>

<span data-ttu-id="fc863-245">Komut satırı yapılandırma sağlayıcısı yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılmak komut satırı bağımsız değişkenlerine izin vermek için sağlayıcıları serisinin son konumlandırmak için yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="fc863-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-246">Yeni bir başlattığınızda bu sağlayıcıları dizi yerine konur <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="fc863-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="fc863-247">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="fc863-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="fc863-248">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırma sağlayıcıları belirtmek için Web ana bilgisayarı oluşturulurken:</span><span class="sxs-lookup"><span data-stu-id="fc863-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="fc863-249">`ConfigureAppConfiguration` *ASP.NET Core 2.1 veya üzeri sunulur.*</span><span class="sxs-lookup"><span data-stu-id="fc863-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-250">Sağlayıcıları bu dizi için (ana değil) uygulaması ile oluşturulabilir bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ve bir çağrı, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> yönteminde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="fc863-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="fc863-251">Yukarıdaki örnekte, ortam adı (`env.EnvironmentName`) ve uygulama derleme adı (`env.ApplicationName`) tarafından sağlanan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="fc863-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="fc863-252">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="fc863-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="fc863-253">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="fc863-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Yapılandırma komut satırı bağımsız değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="fc863-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-255">Komut satırı yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-256">`AddCommandLine` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="fc863-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="fc863-257">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="fc863-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="fc863-258">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="fc863-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="fc863-259">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings.&lt; Ortam&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="fc863-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="fc863-260">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="fc863-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="fc863-261">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="fc863-261">Environment variables.</span></span>

<span data-ttu-id="fc863-262">`CreateDefaultBuilder` Son komut satırı yapılandırma sağlayıcısı ekler.</span><span class="sxs-lookup"><span data-stu-id="fc863-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="fc863-263">Çalışma zamanında geçirilen komut satırı bağımsız değişkenleri yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fc863-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="fc863-264">`CreateDefaultBuilder` konak oluşturulduğunda işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="fc863-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="fc863-265">Bu nedenle, komut satırı yapılandırma etkinleştirildi tarafından `CreateDefaultBuilder` konak nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="fc863-266">Konak el ile oluştururken ve değil çağırma `CreateDefaultBuilder`, çağrı `AddCommandLine` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="fc863-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-267">Komut satırı yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-268">Son çalışma zamanında yapılandırması diğer yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenleri izin vermek için sağlayıcı çağırın.</span><span class="sxs-lookup"><span data-stu-id="fc863-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="fc863-269">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fc863-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="fc863-270">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="fc863-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-271">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="fc863-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-272">1.x örnek uygulama çağrıları <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> üzerinde bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="fc863-273">Proje dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="fc863-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="fc863-274">Bir komut satırı bağımsız değişken `dotnet run` komutu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="fc863-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="fc863-275">Uygulama çalıştıktan sonra bir uygulamaya tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fc863-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="fc863-276">Çıkış için sağlanan yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çifti içeren gözlemleyin `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="fc863-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="fc863-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="fc863-277">Arguments</span></span>

<span data-ttu-id="fc863-278">Değeri bir eşittir işareti gelmelidir (`=`), veya bir önek anahtarı olmalıdır (`--` veya `/`) zaman değeri bir boşluk izler.</span><span class="sxs-lookup"><span data-stu-id="fc863-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="fc863-279">Değer bir eşittir işareti kullanılıyorsa, null olabilir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="fc863-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="fc863-280">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="fc863-280">Key prefix</span></span>               | <span data-ttu-id="fc863-281">Örnek</span><span class="sxs-lookup"><span data-stu-id="fc863-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="fc863-282">Önek yok</span><span class="sxs-lookup"><span data-stu-id="fc863-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="fc863-283">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="fc863-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="fc863-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="fc863-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="fc863-285">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="fc863-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="fc863-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="fc863-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="fc863-287">Aynı komut içinde komut satırı bağımsız değişkeni bir eşittir işareti ile bir alanı kullanan anahtar-değer çiftleri kullanan anahtar-değer çiftleri karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="fc863-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="fc863-288">Örnek komutları:</span><span class="sxs-lookup"><span data-stu-id="fc863-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="fc863-289">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="fc863-289">Switch mappings</span></span>

<span data-ttu-id="fc863-290">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="fc863-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="fc863-291">El ile yapı kurarken yapılandırmayla bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, anahtar değişiklik için bir sözlük sağlayabilir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc863-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="fc863-292">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="fc863-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="fc863-293">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) anahtar-değer çifti uygulamanın yapılandırmasını döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="fc863-294">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="fc863-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="fc863-295">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="fc863-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="fc863-296">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="fc863-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="fc863-297">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="fc863-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="fc863-298">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc863-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="fc863-299">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fc863-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="fc863-300">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="fc863-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="fc863-301">Bağımsız değişkenler değil çözümdür `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc863-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="fc863-302">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="fc863-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="fc863-303">Anahtar</span><span class="sxs-lookup"><span data-stu-id="fc863-303">Key</span></span>       | <span data-ttu-id="fc863-304">Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="fc863-305">Anahtar eşlemeli anahtarları uygulamayı başlatırken kullandıysanız, yapılandırma sözlüğü tarafından sağlanan anahtardaki yapılandırma değeri alır:</span><span class="sxs-lookup"><span data-stu-id="fc863-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="fc863-306">Önceki komutu çalıştırdıktan sonra yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="fc863-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="fc863-307">Anahtar</span><span class="sxs-lookup"><span data-stu-id="fc863-307">Key</span></span>               | <span data-ttu-id="fc863-308">Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="fc863-309">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="fc863-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Yapılandırma ortam değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="fc863-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="fc863-311">Ortam değişkenlerini yapılandırma etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-312">Ortam değişkenleri, iki nokta üst üste ayırıcı hiyerarşik anahtarlarla çalışırken (`:`) tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="fc863-313">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve virgül ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="fc863-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) ortam değişkenlerini yapılandırma Sağlayıcısı'nı kullanarak uygulama yapılandırması geçersiz kılabilirsiniz Azure portalında ortam değişkenlerini ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="fc863-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="fc863-315">Daha fazla bilgi için [Azure uygulamaları: Geçersiz Uygulama yapılandırması Azure portalını kullanarak](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="fc863-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-316">`AddEnvironmentVariables` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="fc863-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="fc863-317">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="fc863-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="fc863-318">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="fc863-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="fc863-319">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings.&lt; Ortam&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="fc863-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="fc863-320">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="fc863-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="fc863-321">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="fc863-321">Command-line arguments.</span></span>

<span data-ttu-id="fc863-322">Yapılandırma, kullanıcı parolalarının kurulduktan sonra ortam değişkeni yapılandırma sağlayıcısı denir ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="fc863-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="fc863-323">Bu konumda sağlayıcıya çağrı ortam değişkenlerini okuma yapılandırması tarafından kullanıcı parolalarını ayarlanmış geçersiz kılmak için çalışma zamanında sağlar ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="fc863-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="fc863-324">Ayrıca doğrudan çağırabilir miyim `AddEnvironmentVariables` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="fc863-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-325">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fc863-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="fc863-326">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="fc863-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-327">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="fc863-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-328">1.x örnek uygulama çağrıları `AddEnvironmentVariables` üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fc863-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="fc863-329">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fc863-329">Run the sample app.</span></span> <span data-ttu-id="fc863-330">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fc863-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="fc863-331">Çıkış ortam değişkeni için anahtar-değer çifti içeren gözlemleyin `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="fc863-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="fc863-332">Değeri, uygulamanın çalıştığı, genellikle ortamın yansıtır `Development` yerel olarak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="fc863-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="fc863-333">Ortam değişkenlerini kısa uygulama tarafından işlenen listesini tutmak için aşağıdaki ile başlayan bu ortam değişkenleri uygulama filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc863-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="fc863-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="fc863-334">ASPNETCORE_</span></span>
* <span data-ttu-id="fc863-335">URL'leri</span><span class="sxs-lookup"><span data-stu-id="fc863-335">urls</span></span>
* <span data-ttu-id="fc863-336">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="fc863-336">Logging</span></span>
* <span data-ttu-id="fc863-337">ORTAM</span><span class="sxs-lookup"><span data-stu-id="fc863-337">ENVIRONMENT</span></span>
* <span data-ttu-id="fc863-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="fc863-338">contentRoot</span></span>
* <span data-ttu-id="fc863-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="fc863-339">AllowedHosts</span></span>
* <span data-ttu-id="fc863-340">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="fc863-340">applicationName</span></span>
* <span data-ttu-id="fc863-341">komut satırı</span><span class="sxs-lookup"><span data-stu-id="fc863-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-342">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Pages/Index.cshtml.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="fc863-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-343">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Controllers/HomeController.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="fc863-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="fc863-344">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="fc863-344">Prefixes</span></span>

<span data-ttu-id="fc863-345">Uygulamanın yapılandırma yüklendi ortam değişkenleri, bir ön ek sağladığında filtrelenir `AddEnvironmentVariables` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc863-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="fc863-346">Örneğin, ortam değişkenlerini önek filtresi için `CUSTOM_`, yapılandırma sağlayıcısı için önek sağlayın:</span><span class="sxs-lookup"><span data-stu-id="fc863-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="fc863-347">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek yapılandırıldıktan devre dışı.</span><span class="sxs-lookup"><span data-stu-id="fc863-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-348">Statik kolaylık yöntemi `CreateDefaultBuilder` oluşturur bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> uygulamanın ana bilgisayar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="fc863-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="fc863-349">Zaman `WebHostBuilder` olan oluşturulan, kendi ana bilgisayar yapılandırması ön ekine sahip ortam değişkenleri içindeki bulduğu `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="fc863-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="fc863-350">**Bağlantı dizesi ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="fc863-350">**Connection string prefixes**</span></span>

<span data-ttu-id="fc863-351">Yapılandırma API'si, dört bağlantı dizesi ortam değişkenleri için app ortamı için Azure bağlantı dizelerini yapılandırılmasıyla ilgili özel işleme kurallarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc863-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="fc863-352">Önek yok belirtilirse tabloda gösterilen ön ekine sahip ortam değişkenleri uygulamaya yüklenir `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="fc863-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="fc863-353">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="fc863-353">Connection string prefix</span></span> | <span data-ttu-id="fc863-354">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="fc863-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="fc863-355">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="fc863-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="fc863-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="fc863-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="fc863-357">Azure SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="fc863-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="fc863-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="fc863-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="fc863-359">Ne zaman bir ortam değişkeni bulunur ve herhangi bir tabloda gösterilen dört öneklerini yapılandırmasını yüklendi:</span><span class="sxs-lookup"><span data-stu-id="fc863-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="fc863-360">Yapılandırma anahtarı ortam değişkeni ön eki kaldırma ve yapılandırma anahtar bölümünü ekleyerek oluşturulur (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="fc863-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="fc863-361">Veritabanı bağlantı sağlayıcısı temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (dışında `CUSTOMCONNSTR_`, belirtilen sağlayıcı yok sahiptir).</span><span class="sxs-lookup"><span data-stu-id="fc863-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="fc863-362">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="fc863-362">Environment variable key</span></span> | <span data-ttu-id="fc863-363">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="fc863-363">Converted configuration key</span></span> | <span data-ttu-id="fc863-364">Sağlayıcı Yapılandırması girdisi</span><span class="sxs-lookup"><span data-stu-id="fc863-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="fc863-365">Yapılandırma girişi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="fc863-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="fc863-366">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="fc863-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="fc863-367">Değer: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="fc863-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="fc863-368">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="fc863-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="fc863-369">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="fc863-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="fc863-370">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="fc863-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="fc863-371">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="fc863-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="fc863-372">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-372">File Configuration Provider</span></span>

<span data-ttu-id="fc863-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> yapılandırma dosya sisteminden yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="fc863-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="fc863-374">Aşağıdaki yapılandırma sağlayıcıları için belirli dosya türleri ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="fc863-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="fc863-375">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="fc863-376">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="fc863-377">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="fc863-378">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-378">INI Configuration Provider</span></span>

<span data-ttu-id="fc863-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Yapılandırma ını dosyası anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="fc863-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="fc863-380">INI dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-381">İki nokta üst üste INI dosya yapılandırması bölüm ayırıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc863-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="fc863-382">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="fc863-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="fc863-383">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="fc863-383">Whether the file is optional.</span></span>
* <span data-ttu-id="fc863-384">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="fc863-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="fc863-385"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-386">Çağrılırken `CreateDefaultBuilder`, çağrı `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="fc863-387">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-388">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fc863-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="fc863-389">Genel bir INI yapılandırma dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="fc863-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="fc863-390">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="fc863-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="fc863-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-391">section0:key0</span></span>
* <span data-ttu-id="fc863-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-392">section0:key1</span></span>
* <span data-ttu-id="fc863-393">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="fc863-393">section1:subsection:key</span></span>
* <span data-ttu-id="fc863-394">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="fc863-394">section2:subsection0:key</span></span>
* <span data-ttu-id="fc863-395">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="fc863-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="fc863-396">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-396">JSON Configuration Provider</span></span>

<span data-ttu-id="fc863-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Yapılandırma JSON dosyası anahtar-değer çiftlerinden çalışma zamanı sırasında yükler.</span><span class="sxs-lookup"><span data-stu-id="fc863-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="fc863-398">JSON dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-399">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="fc863-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="fc863-400">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="fc863-400">Whether the file is optional.</span></span>
* <span data-ttu-id="fc863-401">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="fc863-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="fc863-402"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-403">`AddJsonFile` Yeni bir başlattığınızda iki kez otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="fc863-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="fc863-404">Yöntem yapılandırmasından yüklenemedi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="fc863-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="fc863-405">*appSettings.JSON* &ndash; bu dosyayı ilk okuyun.</span><span class="sxs-lookup"><span data-stu-id="fc863-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="fc863-406">Dosyanın ortam sürümü tarafından sağlanan değerleri geçersiz kılabilir *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="fc863-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="fc863-407">*appSettings. &lt;Ortam&gt;.json* &ndash; dosyanın ortam sürümünü temel alınarak yüklenir [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="fc863-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="fc863-408">Daha fazla bilgi için [Web ana bilgisayarı: bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="fc863-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="fc863-409">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="fc863-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="fc863-410">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="fc863-410">Environment variables.</span></span>
* <span data-ttu-id="fc863-411">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="fc863-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="fc863-412">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="fc863-412">Command-line arguments.</span></span>

<span data-ttu-id="fc863-413">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fc863-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="fc863-414">Bu nedenle, yapılandırma kümesi kullanıcı parolaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri geçersiz kılma *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="fc863-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="fc863-415">Ayrıca doğrudan çağırabilir miyim `AddJsonFile` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-416">Çağrılırken `CreateDefaultBuilder`, çağrı `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="fc863-417">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-418">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fc863-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="fc863-419">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="fc863-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-420">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` iki çağrıları içeren ana bilgisayar oluşturmak için `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="fc863-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="fc863-421">Yapılandırma yüklenir *appsettings.json* ve *appsettings.&lt; Ortam&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="fc863-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-422">1.x örnek uygulama çağrıları `AddJsonFile` iki kez üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fc863-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="fc863-423">Yapılandırma yüklenir *appsettings.json* ve *appsettings.&lt; Ortam&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="fc863-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="fc863-424">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fc863-424">Run the sample app.</span></span> <span data-ttu-id="fc863-425">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fc863-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="fc863-426">Çıkış ortamına bağlı olarak tabloda gösterilen yapılandırması için anahtar-değer çiftleri içeren gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="fc863-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="fc863-427">Günlük kaydı yapılandırması tuşlarını iki nokta üst üste (`:`) hiyerarşik ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="fc863-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="fc863-428">Anahtar</span><span class="sxs-lookup"><span data-stu-id="fc863-428">Key</span></span>                        | <span data-ttu-id="fc863-429">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="fc863-429">Development Value</span></span> | <span data-ttu-id="fc863-430">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="fc863-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="fc863-431">Günlüğe kaydetme: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="fc863-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="fc863-432">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="fc863-432">Information</span></span>       | <span data-ttu-id="fc863-433">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="fc863-433">Information</span></span>      |
| <span data-ttu-id="fc863-434">Günlüğe kaydetme: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="fc863-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="fc863-435">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="fc863-435">Information</span></span>       | <span data-ttu-id="fc863-436">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="fc863-436">Information</span></span>      |
| <span data-ttu-id="fc863-437">Günlüğe kaydetme: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="fc863-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="fc863-438">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="fc863-438">Debug</span></span>             | <span data-ttu-id="fc863-439">Hata</span><span class="sxs-lookup"><span data-stu-id="fc863-439">Error</span></span>            |
| <span data-ttu-id="fc863-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="fc863-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="fc863-441">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-441">XML Configuration Provider</span></span>

<span data-ttu-id="fc863-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Yapılandırma XML dosyası anahtar-değer çiftlerinin zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="fc863-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="fc863-443">XML dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-444">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="fc863-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="fc863-445">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="fc863-445">Whether the file is optional.</span></span>
* <span data-ttu-id="fc863-446">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="fc863-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="fc863-447"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="fc863-448">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayıldı.</span><span class="sxs-lookup"><span data-stu-id="fc863-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="fc863-449">Bir belge türü tanımı (DTD'nin) veya ad alanı dosyasında belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="fc863-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-450">Çağrılırken `CreateDefaultBuilder`, çağrı `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="fc863-451">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-452">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fc863-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="fc863-453">XML yapılandırma dosyalarını, yinelenen bölümler için ayrı bir öğe adları kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fc863-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="fc863-454">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="fc863-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="fc863-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-455">section0:key0</span></span>
* <span data-ttu-id="fc863-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-456">section0:key1</span></span>
* <span data-ttu-id="fc863-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-457">section1:key0</span></span>
* <span data-ttu-id="fc863-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-458">section1:key1</span></span>

<span data-ttu-id="fc863-459">İş öğesi adının aynısını kullanın öğeleri, yinelenen `name` özniteliği öğeleri ayırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="fc863-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="fc863-460">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="fc863-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="fc863-461">Bölüm: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-461">section:section0:key:key0</span></span>
* <span data-ttu-id="fc863-462">Bölüm: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-462">section:section0:key:key1</span></span>
* <span data-ttu-id="fc863-463">Bölüm: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-463">section:section1:key:key0</span></span>
* <span data-ttu-id="fc863-464">Bölüm: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-464">section:section1:key:key1</span></span>

<span data-ttu-id="fc863-465">Öznitelik değerlerini sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fc863-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="fc863-466">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="fc863-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="fc863-467">Anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="fc863-467">key:attribute</span></span>
* <span data-ttu-id="fc863-468">Bölüm: anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="fc863-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="fc863-469">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="fc863-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bir dizin dosyalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="fc863-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="fc863-471">Anahtar dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="fc863-471">The key is the file name.</span></span> <span data-ttu-id="fc863-472">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="fc863-472">The value contains the file's contents.</span></span> <span data-ttu-id="fc863-473">Dosya başına anahtar yapılandırma sağlayıcısı Docker'da barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="fc863-474">Dosya başına anahtar yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="fc863-475">`directoryPath` Dosyalar için mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc863-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="fc863-476">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="fc863-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="fc863-477">Bir `Action<KeyPerFileConfigurationSource>` kaynağını yapılandırır temsilci.</span><span class="sxs-lookup"><span data-stu-id="fc863-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="fc863-478">Dizin isteğe bağlı olup olmadığını ve dizinin yolu.</span><span class="sxs-lookup"><span data-stu-id="fc863-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="fc863-479">Çağrılırken `CreateDefaultBuilder`, çağrı `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="fc863-480">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="fc863-481">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-481">Memory Configuration Provider</span></span>

<span data-ttu-id="fc863-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Bir bellek içi koleksiyonu yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="fc863-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="fc863-483">Bellek içi toplama yapılandırması etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="fc863-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="fc863-484">Yapılandırma sağlayıcısı ile başlatılabilir bir `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="fc863-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-485">Çağrılırken `CreateDefaultBuilder`, çağrı `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="fc863-486">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak `UseConfiguration` yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="fc863-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-487">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile `UseConfiguration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fc863-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="fc863-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="fc863-488">GetValue</span></span>

<span data-ttu-id="fc863-489">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) bir değeri belirtilen bir anahtarla yapılandırmasından ayıklar ve onu belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="fc863-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="fc863-490">Aşırı yükleme anahtarı bulunmazsa, varsayılan bir değer sağlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="fc863-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="fc863-491">Aşağıdaki örnek bir dize değeri yapılandırmasından anahtarıyla ayıklar `NumberKey`, değer olarak türleri bir `int`ve değeri değişkeninde depolar `intValue`.</span><span class="sxs-lookup"><span data-stu-id="fc863-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="fc863-492">Varsa `NumberKey` yapılandırma anahtarlarını bulunamadığında `intValue` varsayılan değerini alır `99`:</span><span class="sxs-lookup"><span data-stu-id="fc863-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="fc863-493">GetSection, GetChildren ve var.</span><span class="sxs-lookup"><span data-stu-id="fc863-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="fc863-494">Aşağıdaki örneklerde için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="fc863-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="fc863-495">Dört anahtarları içeren bir çift alt bölümleri içerir, iki bölümlerde bulunur:</span><span class="sxs-lookup"><span data-stu-id="fc863-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="fc863-496">Dosya yapılandırma okuduğunuzda aşağıdaki benzersiz hiyerarşik anahtarları yapılandırma değerleri tutmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="fc863-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="fc863-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-497">section0:key0</span></span>
* <span data-ttu-id="fc863-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-498">section0:key1</span></span>
* <span data-ttu-id="fc863-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-499">section1:key0</span></span>
* <span data-ttu-id="fc863-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-500">section1:key1</span></span>
* <span data-ttu-id="fc863-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="fc863-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="fc863-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="fc863-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="fc863-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="fc863-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="fc863-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="fc863-505">GetSection</span></span>

<span data-ttu-id="fc863-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) yapılandırma bölümüne belirtilen alt anahtarını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="fc863-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="fc863-507">Döndürülecek bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> yalnızca anahtar-değer çiftlerini içeren `section1`, çağrı `GetSection` ve bölüm adı verin:</span><span class="sxs-lookup"><span data-stu-id="fc863-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="fc863-508">Benzer şekilde, anahtarları değerlerini almak için `section2:subsection0`, çağrı `GetSection` ve bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="fc863-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="fc863-509">`GetSection` hiç dönmüyor `null`.</span><span class="sxs-lookup"><span data-stu-id="fc863-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="fc863-510">Eşleşen bir bölümü olmadığından bulunamazsa, boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="fc863-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="fc863-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="fc863-511">GetChildren</span></span>

<span data-ttu-id="fc863-512">Bir çağrı [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) üzerinde `section2` alır bir `IEnumerable<IConfigurationSection>` içeren:</span><span class="sxs-lookup"><span data-stu-id="fc863-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="fc863-513">Var.</span><span class="sxs-lookup"><span data-stu-id="fc863-513">Exists</span></span>

<span data-ttu-id="fc863-514">Kullanım [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) yapılandırma bölümü olup olmadığını belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="fc863-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="fc863-515">Belirtilen örnek veri `sectionExists` olduğu `false` olmadığından bir `section2:subsection2` yapılandırma verilerini bir bölümde.</span><span class="sxs-lookup"><span data-stu-id="fc863-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="fc863-516">Bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="fc863-516">Bind to a class</span></span>

<span data-ttu-id="fc863-517">Yapılandırma kullanarak ilgili ayar gruplarını temsil eden sınıflar için bağlanabilir *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="fc863-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="fc863-518">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="fc863-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="fc863-519">Yapılandırma değerleri döndürülür dizeler olarak çağıran <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> oluşumu sağlayan [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesneleri.</span><span class="sxs-lookup"><span data-stu-id="fc863-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="fc863-520">Örnek uygulamayı içeren bir `Starship` modeli (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="fc863-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-521">`starship` Bölümünü *starship.json* örnek uygulamayı yapılandırma yüklemek için JSON yapılandırma sağlayıcısı kullandığında yapılandırma dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="fc863-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="fc863-522">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="fc863-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="fc863-523">Anahtar</span><span class="sxs-lookup"><span data-stu-id="fc863-523">Key</span></span>                   | <span data-ttu-id="fc863-524">Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="fc863-525">starship: adı</span><span class="sxs-lookup"><span data-stu-id="fc863-525">starship:name</span></span>         | <span data-ttu-id="fc863-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="fc863-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="fc863-527">starship: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="fc863-527">starship:registry</span></span>     | <span data-ttu-id="fc863-528">NCC 1701</span><span class="sxs-lookup"><span data-stu-id="fc863-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="fc863-529">starship: sınıfı</span><span class="sxs-lookup"><span data-stu-id="fc863-529">starship:class</span></span>        | <span data-ttu-id="fc863-530">Anayasa</span><span class="sxs-lookup"><span data-stu-id="fc863-530">Constitution</span></span>                                      |
| <span data-ttu-id="fc863-531">starship: uzunluğu</span><span class="sxs-lookup"><span data-stu-id="fc863-531">starship:length</span></span>       | <span data-ttu-id="fc863-532">304.8</span><span class="sxs-lookup"><span data-stu-id="fc863-532">304.8</span></span>                                             |
| <span data-ttu-id="fc863-533">starship: yetkilendirilen</span><span class="sxs-lookup"><span data-stu-id="fc863-533">starship:commissioned</span></span> | <span data-ttu-id="fc863-534">False</span><span class="sxs-lookup"><span data-stu-id="fc863-534">False</span></span>                                             |
| <span data-ttu-id="fc863-535">Ticari marka</span><span class="sxs-lookup"><span data-stu-id="fc863-535">trademark</span></span>             | <span data-ttu-id="fc863-536">Paramount resimleri Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="fc863-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="fc863-537">Örnek Uygulama çağrıları `GetSection` ile `starship` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="fc863-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="fc863-538">`starship` Anahtar-değer çiftleridir yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="fc863-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="fc863-539">`Bind` Bir örneğini geçirerek Altbölüm yöntemi çağrıldığında `Starship` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="fc863-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="fc863-540">Örnek değerleri bağlandıktan sonra işleme için bir özellik için örneği atanır:</span><span class="sxs-lookup"><span data-stu-id="fc863-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="fc863-541">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="fc863-541">Bind to an object graph</span></span>

<span data-ttu-id="fc863-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Tüm POCO Nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc863-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="fc863-543">Örnek içeren bir `TvShow` olan nesne grafiğini içeren model `Metadata` ve `Actors` sınıfları (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="fc863-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-544">Örnek uygulamanın bir *tvshow.xml* yapılandırma verilerini içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="fc863-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="fc863-545">Yapılandırma tüm bağlı `TvShow` Nesne grafiği ile `Bind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fc863-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="fc863-546">İlişkili örneği, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="fc863-546">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="fc863-547">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="fc863-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="fc863-548">`Get<T>` kullanmaktan daha kullanışlı olan `Bind`.</span><span class="sxs-lookup"><span data-stu-id="fc863-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="fc863-549">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle birlikte işlemek için kullanılan özellik doğrudan atanan bağlı örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="fc863-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="fc863-550">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="fc863-550">Bind an array to a class</span></span>

<span data-ttu-id="fc863-551">*Örnek uygulama, bu bölümde açıklanan kavramları göstermektedir.*</span><span class="sxs-lookup"><span data-stu-id="fc863-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="fc863-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="fc863-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="fc863-553">Sayısal bir anahtar kesimi sunan herhangi bir dizi biçimi (`:0:`, `:1:`, &hellip; `:{n}:`) dizisi bağlama POCO sınıfı dizisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc863-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="fc863-554">Bağlama, kural olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fc863-554">Binding is provided by convention.</span></span> <span data-ttu-id="fc863-555">Özel yapılandırma sağlayıcıları dizi bağlama uygulamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fc863-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="fc863-556">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="fc863-556">**In-memory array processing**</span></span>

<span data-ttu-id="fc863-557">Yapılandırma anahtarları ve değerleri aşağıdaki tabloda gösterilen göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="fc863-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="fc863-558">Anahtar</span><span class="sxs-lookup"><span data-stu-id="fc863-558">Key</span></span>     | <span data-ttu-id="fc863-559">Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="fc863-560">dizi: 0</span><span class="sxs-lookup"><span data-stu-id="fc863-560">array:0</span></span> | <span data-ttu-id="fc863-561">value0</span><span class="sxs-lookup"><span data-stu-id="fc863-561">value0</span></span> |
| <span data-ttu-id="fc863-562">dizi: 1</span><span class="sxs-lookup"><span data-stu-id="fc863-562">array:1</span></span> | <span data-ttu-id="fc863-563">Değer1</span><span class="sxs-lookup"><span data-stu-id="fc863-563">value1</span></span> |
| <span data-ttu-id="fc863-564">dizi: 2</span><span class="sxs-lookup"><span data-stu-id="fc863-564">array:2</span></span> | <span data-ttu-id="fc863-565">Value2</span><span class="sxs-lookup"><span data-stu-id="fc863-565">value2</span></span> |
| <span data-ttu-id="fc863-566">dizi: 4</span><span class="sxs-lookup"><span data-stu-id="fc863-566">array:4</span></span> | <span data-ttu-id="fc863-567">Değer4</span><span class="sxs-lookup"><span data-stu-id="fc863-567">value4</span></span> |
| <span data-ttu-id="fc863-568">dizi: 5</span><span class="sxs-lookup"><span data-stu-id="fc863-568">array:5</span></span> | <span data-ttu-id="fc863-569">Değeri5</span><span class="sxs-lookup"><span data-stu-id="fc863-569">value5</span></span> |

<span data-ttu-id="fc863-570">Bellek yapılandırma sağlayıcısı kullanan örnek uygulamasında, bu anahtarların ve değerlerin yüklenir:</span><span class="sxs-lookup"><span data-stu-id="fc863-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="fc863-571">Dizi dizini için bir değer atlar &num;3.</span><span class="sxs-lookup"><span data-stu-id="fc863-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="fc863-572">Yapılandırma bağlayıcı temizleyin, bu dizi için bir nesne bağlamanın sonucunu gösterilmiştir birazdan haline geldikten bağlama null değerler veya null girişler bağımlı nesneleri oluşturma yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="fc863-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="fc863-573">Örnek uygulamada, ilişkili yapılandırma verilerini tutmak bir POCO sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fc863-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-574">Yapılandırma verilerini nesnesine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="fc863-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="fc863-575">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) söz dizimi de kullanılabilir, daha küçük kod sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="fc863-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="fc863-576">İlişkili nesne, örneği `ArrayExample`, yapılandırmasından dizisi verileri alır.</span><span class="sxs-lookup"><span data-stu-id="fc863-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="fc863-577">`ArrayExamples.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="fc863-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="fc863-578">`ArrayExamples.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="fc863-579">0</span><span class="sxs-lookup"><span data-stu-id="fc863-579">0</span></span>                             | <span data-ttu-id="fc863-580">value0</span><span class="sxs-lookup"><span data-stu-id="fc863-580">value0</span></span>                        |
| <span data-ttu-id="fc863-581">1.</span><span class="sxs-lookup"><span data-stu-id="fc863-581">1</span></span>                             | <span data-ttu-id="fc863-582">Değer1</span><span class="sxs-lookup"><span data-stu-id="fc863-582">value1</span></span>                        |
| <span data-ttu-id="fc863-583">2</span><span class="sxs-lookup"><span data-stu-id="fc863-583">2</span></span>                             | <span data-ttu-id="fc863-584">Value2</span><span class="sxs-lookup"><span data-stu-id="fc863-584">value2</span></span>                        |
| <span data-ttu-id="fc863-585">3</span><span class="sxs-lookup"><span data-stu-id="fc863-585">3</span></span>                             | <span data-ttu-id="fc863-586">Değer4</span><span class="sxs-lookup"><span data-stu-id="fc863-586">value4</span></span>                        |
| <span data-ttu-id="fc863-587">4</span><span class="sxs-lookup"><span data-stu-id="fc863-587">4</span></span>                             | <span data-ttu-id="fc863-588">Değeri5</span><span class="sxs-lookup"><span data-stu-id="fc863-588">value5</span></span>                        |

<span data-ttu-id="fc863-589">Dizin &num;3'te ilişkili nesne için yapılandırma verilerini tutan `array:4` yapılandırma anahtarı ve değeri `value4`.</span><span class="sxs-lookup"><span data-stu-id="fc863-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="fc863-590">Bir diziyi içeren yapılandırma verilerini bağlandığında, dizi dizinleri yapılandırma anahtarları yalnızca nesne oluşturma sırasında yapılandırma verileri yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="fc863-591">Yapılandırma verilerinde bir null değer tutulamıyor ve yapılandırma anahtarları bir dizide bir veya daha fazla dizinlerini atladığında null değerli bir girişi bir bağımlı nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="fc863-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="fc863-592">Eksik yapılandırma öğesi için dizin &num;bağlama önce 3 sağlanabilir `ArrayExamples` örneği üretir yapılandırma doğru anahtar-değer çifti herhangi bir yapılandırma sağlayıcısı tarafından.</span><span class="sxs-lookup"><span data-stu-id="fc863-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="fc863-593">Ek bir JSON yapılandırma sağlayıcısı eksik anahtar-değer çifti ile örneği varsa `ArrayExamples.Entries` yapılandırmanın tamamı dizisi ile eşleşen:</span><span class="sxs-lookup"><span data-stu-id="fc863-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="fc863-594">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="fc863-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fc863-595">İçinde `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="fc863-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fc863-596">İçinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="fc863-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="fc863-597">Tabloda belirtilen anahtar-değer çifti yapılandırma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="fc863-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="fc863-598">Anahtar</span><span class="sxs-lookup"><span data-stu-id="fc863-598">Key</span></span>             | <span data-ttu-id="fc863-599">Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="fc863-600">dizi: girişler: 3</span><span class="sxs-lookup"><span data-stu-id="fc863-600">array:entries:3</span></span> | <span data-ttu-id="fc863-601">Değeri3</span><span class="sxs-lookup"><span data-stu-id="fc863-601">value3</span></span> |

<span data-ttu-id="fc863-602">Varsa `ArrayExamples` sınıf örneği bağlı dizin için giriş JSON yapılandırma sağlayıcısı içerir sonra &num;3 `ArrayExamples.Entries` dizi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="fc863-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="fc863-603">`ArrayExamples.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="fc863-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="fc863-604">`ArrayExamples.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="fc863-605">0</span><span class="sxs-lookup"><span data-stu-id="fc863-605">0</span></span>                             | <span data-ttu-id="fc863-606">value0</span><span class="sxs-lookup"><span data-stu-id="fc863-606">value0</span></span>                        |
| <span data-ttu-id="fc863-607">1.</span><span class="sxs-lookup"><span data-stu-id="fc863-607">1</span></span>                             | <span data-ttu-id="fc863-608">Değer1</span><span class="sxs-lookup"><span data-stu-id="fc863-608">value1</span></span>                        |
| <span data-ttu-id="fc863-609">2</span><span class="sxs-lookup"><span data-stu-id="fc863-609">2</span></span>                             | <span data-ttu-id="fc863-610">Value2</span><span class="sxs-lookup"><span data-stu-id="fc863-610">value2</span></span>                        |
| <span data-ttu-id="fc863-611">3</span><span class="sxs-lookup"><span data-stu-id="fc863-611">3</span></span>                             | <span data-ttu-id="fc863-612">Değeri3</span><span class="sxs-lookup"><span data-stu-id="fc863-612">value3</span></span>                        |
| <span data-ttu-id="fc863-613">4</span><span class="sxs-lookup"><span data-stu-id="fc863-613">4</span></span>                             | <span data-ttu-id="fc863-614">Değer4</span><span class="sxs-lookup"><span data-stu-id="fc863-614">value4</span></span>                        |
| <span data-ttu-id="fc863-615">5</span><span class="sxs-lookup"><span data-stu-id="fc863-615">5</span></span>                             | <span data-ttu-id="fc863-616">Değeri5</span><span class="sxs-lookup"><span data-stu-id="fc863-616">value5</span></span>                        |

<span data-ttu-id="fc863-617">**JSON dizisi işleme**</span><span class="sxs-lookup"><span data-stu-id="fc863-617">**JSON array processing**</span></span>

<span data-ttu-id="fc863-618">Bir JSON dosyası bir dizi varsa, dizi öğeleri bölümünde sıfır tabanlı dizine sahip için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fc863-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="fc863-619">Aşağıdaki yapılandırma dosyasında `subsection` dizisi:</span><span class="sxs-lookup"><span data-stu-id="fc863-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="fc863-620">JSON yapılandırma sağlayıcısı, aşağıdaki anahtar-değer çiftlerine yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="fc863-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="fc863-621">Anahtar</span><span class="sxs-lookup"><span data-stu-id="fc863-621">Key</span></span>                     | <span data-ttu-id="fc863-622">Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="fc863-623">json_array:Key</span><span class="sxs-lookup"><span data-stu-id="fc863-623">json_array:key</span></span>          | <span data-ttu-id="fc863-624">Değera</span><span class="sxs-lookup"><span data-stu-id="fc863-624">valueA</span></span> |
| <span data-ttu-id="fc863-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="fc863-625">json_array:subsection:0</span></span> | <span data-ttu-id="fc863-626">Değerb</span><span class="sxs-lookup"><span data-stu-id="fc863-626">valueB</span></span> |
| <span data-ttu-id="fc863-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="fc863-627">json_array:subsection:1</span></span> | <span data-ttu-id="fc863-628">valueC</span><span class="sxs-lookup"><span data-stu-id="fc863-628">valueC</span></span> |
| <span data-ttu-id="fc863-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="fc863-629">json_array:subsection:2</span></span> | <span data-ttu-id="fc863-630">Değerli</span><span class="sxs-lookup"><span data-stu-id="fc863-630">valueD</span></span> |

<span data-ttu-id="fc863-631">Örnek uygulamada, aşağıdaki POCO sınıfı yapılandırma anahtar-değer çiftleri bağlamak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fc863-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-632">Bağlama sonra `JsonArrayExample.Key` değerine `valueA`.</span><span class="sxs-lookup"><span data-stu-id="fc863-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="fc863-633">Alt değerleri POCO dizi özelliğinde depolanır `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="fc863-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="fc863-634">`JsonArrayExample.Subsection` Dizin</span><span class="sxs-lookup"><span data-stu-id="fc863-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="fc863-635">`JsonArrayExample.Subsection` Değer</span><span class="sxs-lookup"><span data-stu-id="fc863-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="fc863-636">0</span><span class="sxs-lookup"><span data-stu-id="fc863-636">0</span></span>                                   | <span data-ttu-id="fc863-637">Değerb</span><span class="sxs-lookup"><span data-stu-id="fc863-637">valueB</span></span>                              |
| <span data-ttu-id="fc863-638">1.</span><span class="sxs-lookup"><span data-stu-id="fc863-638">1</span></span>                                   | <span data-ttu-id="fc863-639">valueC</span><span class="sxs-lookup"><span data-stu-id="fc863-639">valueC</span></span>                              |
| <span data-ttu-id="fc863-640">2</span><span class="sxs-lookup"><span data-stu-id="fc863-640">2</span></span>                                   | <span data-ttu-id="fc863-641">Değerli</span><span class="sxs-lookup"><span data-stu-id="fc863-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="fc863-642">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="fc863-642">Custom configuration provider</span></span>

<span data-ttu-id="fc863-643">Örnek uygulamayı yapılandırma anahtar-değer çiftleri kullanarak bir veritabanını okuyan bir temel yapılandırma sağlayıcısı oluşturma gösterilmektedir [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="fc863-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="fc863-644">Sağlayıcı, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="fc863-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="fc863-645">EF bellek içi veritabanına tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc863-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="fc863-646">Bir bağlantı dizesi gerektiren bir veritabanı kullanmak için ikincil uygulama `ConfigurationBuilder` başka bir yapılandırma sağlayıcısı bağlantı dizesinden sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="fc863-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="fc863-647">Sağlayıcı bir veritabanı tablosu, başlangıç yapılandırmasını içine okur.</span><span class="sxs-lookup"><span data-stu-id="fc863-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="fc863-648">Sağlayıcı anahtarı başına temelinde veritabanını sorgulamak değil.</span><span class="sxs-lookup"><span data-stu-id="fc863-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="fc863-649">Uygulama başlatılır sahip olduktan sonra uygulamanın yapılandırma üzerinde hiçbir etkisi kadar veritabanını güncelleme yeniden üzerinde değişiklik uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="fc863-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="fc863-650">Tanımlayan bir `EFConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık.</span><span class="sxs-lookup"><span data-stu-id="fc863-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="fc863-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc863-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-652">Ekleme bir `EFConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="fc863-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="fc863-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc863-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-654">Uygulayan bir sınıf oluşturma <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="fc863-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="fc863-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc863-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-656">Özel yapılandırma sağlayıcısını devralarak oluşturma <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="fc863-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="fc863-657">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır.</span><span class="sxs-lookup"><span data-stu-id="fc863-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="fc863-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc863-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-659">Bir `AddEFConfiguration` genişletme yöntemi izin veren yapılandırması kaynağına ekleme bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fc863-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="fc863-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc863-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="fc863-661">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigurationProvider` içinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc863-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="fc863-662">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fc863-662">Access configuration during startup</span></span>

<span data-ttu-id="fc863-663">Ekleme `IConfiguration` içine `Startup` oluşturucuya erişim yapılandırma değerlerini `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fc863-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fc863-664">Erişim yapılandırmaya `Startup.Configure`, ya da ekleme `IConfiguration` doğrudan yöntem veya oluşturucu örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="fc863-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="fc863-665">Başlangıç kullanışlı yöntemler kullanarak yapılandırma erişme ilişkin bir örnek için bkz [uygulama başlatma: yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="fc863-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="fc863-666">Erişim yapılandırmasında bir Razor sayfaları sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="fc863-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="fc863-667">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](xref:Microsoft.Extensions.Configuration) ve ekleme <xref:Microsoft.Extensions.Configuration.IConfiguration> sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="fc863-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="fc863-668">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="fc863-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="fc863-669">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="fc863-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="fc863-670">Dış bütünleştirilmiş koddan Yapılandırması Ekle</span><span class="sxs-lookup"><span data-stu-id="fc863-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="fc863-671">Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="fc863-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="fc863-672">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="fc863-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fc863-673">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fc863-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="fc863-674">Microsoft yapılandırma hakkında ayrıntılı bir inceleme</span><span class="sxs-lookup"><span data-stu-id="fc863-674">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
