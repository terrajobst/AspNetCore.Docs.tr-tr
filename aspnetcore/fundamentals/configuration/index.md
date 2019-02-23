---
title: ASP.NET core'da yapılandırma
author: guardrex
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/configuration/index
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="2b200-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2b200-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="2b200-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2b200-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2b200-105">ASP.NET core'da uygulama yapılandırması tarafından kurulan anahtar-değer çiftleri temel *yapılandırma sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="2b200-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="2b200-106">Yapılandırma sağlayıcıları, yapılandırma kaynaklarını çeşitli anahtar-değer çiftlerine yapılandırma verilerini okuyun:</span><span class="sxs-lookup"><span data-stu-id="2b200-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="2b200-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2b200-107">Azure Key Vault</span></span>
* <span data-ttu-id="2b200-108">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-108">Command-line arguments</span></span>
* <span data-ttu-id="2b200-109">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="2b200-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="2b200-110">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="2b200-110">Directory files</span></span>
* <span data-ttu-id="2b200-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-111">Environment variables</span></span>
* <span data-ttu-id="2b200-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="2b200-112">In-memory .NET objects</span></span>
* <span data-ttu-id="2b200-113">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="2b200-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="2b200-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2b200-114">Azure Key Vault</span></span>
* <span data-ttu-id="2b200-115">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-115">Command-line arguments</span></span>
* <span data-ttu-id="2b200-116">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="2b200-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="2b200-117">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-117">Environment variables</span></span>
* <span data-ttu-id="2b200-118">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="2b200-118">In-memory .NET objects</span></span>
* <span data-ttu-id="2b200-119">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="2b200-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="2b200-120">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-120">Command-line arguments</span></span>
* <span data-ttu-id="2b200-121">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="2b200-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="2b200-122">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-122">Environment variables</span></span>
* <span data-ttu-id="2b200-123">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="2b200-123">In-memory .NET objects</span></span>
* <span data-ttu-id="2b200-124">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="2b200-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="2b200-125">*Seçenekleri deseni* bu konuda açıklanan yapılandırma kavramları bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="2b200-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="2b200-126">Seçenekler, ilgili ayar gruplarını temsil etmek için sınıflar kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b200-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="2b200-127">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="2b200-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="2b200-128">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b200-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-129">Bu üç paketi içinde yer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-129">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2b200-130">Bu üç paketi içinde yer [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="2b200-130">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="2b200-131">Uygulama yapılandırması barındırın</span><span class="sxs-lookup"><span data-stu-id="2b200-131">Host vs. app configuration</span></span>

<span data-ttu-id="2b200-132">Uygulama yapılandırılmış ve başlatıldı, önce bir *konak* başlatılan ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-132">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="2b200-133">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="2b200-133">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="2b200-134">Bu konuda açıklanan yapılandırma sağlayıcıları kullanarak, hem uygulama hem de konak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-134">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="2b200-135">Ana bilgisayar yapılandırma anahtar-değer çiftleri uygulamanın genel yapılandırmasının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2b200-135">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="2b200-136">Yapılandırma sağlayıcıları konak oluşturulduğunda kullanılan yapılandırma ve yapılandırma kaynaklarını nasıl etkileyeceğini nasıl barındırmak daha fazla bilgi için bkz: [konak](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="2b200-136">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="2b200-137">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2b200-137">Default configuration</span></span>

<span data-ttu-id="2b200-138">Web uygulamaları üzerinde ASP.NET Core tabanlı [yeni dotnet](/dotnet/core/tools/dotnet-new) şablonları çağrı <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="2b200-138">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="2b200-139">`CreateDefaultBuilder` uygulama için varsayılan yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="2b200-139">`CreateDefaultBuilder` provides default configuration for the app.</span></span>

* <span data-ttu-id="2b200-140">Ana bilgisayar yapılandırma öğesinden sağlanır:</span><span class="sxs-lookup"><span data-stu-id="2b200-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="2b200-141">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2b200-141">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="2b200-142">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2b200-142">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="2b200-143">Uygulama Yapılandırma (aşağıdaki sırayla) sağlanır:</span><span class="sxs-lookup"><span data-stu-id="2b200-143">App configuration is provided from (in the following order):</span></span>
  * <span data-ttu-id="2b200-144">*appSettings.JSON* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2b200-144">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2b200-145">*appSettings. {Ortamı} .json* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2b200-145">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2b200-146">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="2b200-146">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="2b200-147">Ortam değişkenlerini kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2b200-147">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="2b200-148">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2b200-148">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="2b200-149">Yapılandırma sağlayıcıları, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2b200-149">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="2b200-150">Konak hakkında daha fazla bilgi ve `CreateDefaultBuilder`, bkz: <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="2b200-150">For more information on the host and `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="2b200-151">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="2b200-151">Security</span></span>

<span data-ttu-id="2b200-152">Aşağıdaki en iyi benimseme:</span><span class="sxs-lookup"><span data-stu-id="2b200-152">Adopt the following best practices:</span></span>

* <span data-ttu-id="2b200-153">Hiçbir zaman parolaları ve diğer hassas verileri yapılandırma sağlayıcısı kodda veya düz metin yapılandırma dosyalarında depolayın.</span><span class="sxs-lookup"><span data-stu-id="2b200-153">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="2b200-154">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="2b200-154">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="2b200-155">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="2b200-155">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="2b200-156">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [gizli dizi Yöneticisi ile geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) (depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. hassas verileri).</span><span class="sxs-lookup"><span data-stu-id="2b200-156">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="2b200-157">Gizli dizi Yöneticisi'ni dosya yapılandırma sağlayıcısı bir JSON dosyası yerel sistemdeki kullanıcı gizli dizileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b200-157">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="2b200-158">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2b200-158">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="2b200-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) uygulama gizli anahtarlarının güvenli bir şekilde depolanması için bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="2b200-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="2b200-160">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="2b200-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="2b200-161">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="2b200-161">Hierarchical configuration data</span></span>

<span data-ttu-id="2b200-162">Yapılandırma API configuration anahtarlarında bir sınırlayıcı kullanarak hiyerarşik veri düzleştirme tarafından hiyerarşik yapılandırma verileri koruma özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="2b200-162">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="2b200-163">Aşağıdaki JSON dosyasında iki bölüm yapılandırılmış bir hiyerarşide dört anahtarları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="2b200-163">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="2b200-164">Dosya yapılandırma okuduğunuzda benzersiz anahtarlar özgün hiyerarşik veri yapısını yapılandırma kaynağı korumak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2b200-164">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="2b200-165">Bölümler ve anahtarlar ile bir iki nokta üst üste kullanımını düzleştirilir (`:`) özgün yapıyı korumak için:</span><span class="sxs-lookup"><span data-stu-id="2b200-165">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="2b200-166">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-166">section0:key0</span></span>
* <span data-ttu-id="2b200-167">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-167">section0:key1</span></span>
* <span data-ttu-id="2b200-168">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-168">section1:key0</span></span>
* <span data-ttu-id="2b200-169">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-169">section1:key1</span></span>

<span data-ttu-id="2b200-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemlerdir bölümler ve yapılandırma verilerini bir bölümde alt yalıtmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="2b200-171">Bu yöntem daha sonra açıklanmıştır [GetSection GetChildren ve Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="2b200-171">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="2b200-172">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-172">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="2b200-173">Kurallar</span><span class="sxs-lookup"><span data-stu-id="2b200-173">Conventions</span></span>

<span data-ttu-id="2b200-174">Uygulama başlangıcında, yapılandırma kaynaklarını yapılandırma sağlayıcıları belirttiğiniz sırayla okunur.</span><span class="sxs-lookup"><span data-stu-id="2b200-174">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="2b200-175">Dosya yapılandırma sağlayıcıları uygulama başlangıcından sonra bir temel alınan ayarları dosyası değiştirildiğinde yapılandırmayı yeniden yükle seçeneğine sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b200-175">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="2b200-176">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2b200-176">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="2b200-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> uygulamanın kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="2b200-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2b200-178">Bunlar ana bilgisayar tarafından kurduktan olduğunda değil olarak yapılandırma sağlayıcıları DI, faydalanamaz.</span><span class="sxs-lookup"><span data-stu-id="2b200-178">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="2b200-179">Yapılandırma anahtarları, aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="2b200-179">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="2b200-180">Anahtarlar büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="2b200-180">Keys are case-insensitive.</span></span> <span data-ttu-id="2b200-181">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-181">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="2b200-182">Aynı veya farklı yapılandırma sağlayıcıları tarafından aynı anahtar için bir değer ayarlarsanız, anahtarda ayarlanan son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="2b200-182">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="2b200-183">Hiyerarşik anahtarları</span><span class="sxs-lookup"><span data-stu-id="2b200-183">Hierarchical keys</span></span>
  * <span data-ttu-id="2b200-184">Yapılandırma API'sinin, iki nokta üst üste ayırıcı (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="2b200-184">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="2b200-185">Ortam değişkenleri, iki nokta üst üste ayırıcı tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-185">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="2b200-186">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve bir iki nokta üst üste dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2b200-186">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="2b200-187">Azure anahtar Kasası'nda hiyerarşik tuşlarını `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="2b200-187">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="2b200-188">Gizli dizileri uygulama yapılandırma yüklendiğinde tireler iki nokta üst üste ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2b200-188">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="2b200-189"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="2b200-189">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2b200-190">Dizi bağlama açıklanan [bir dizi bir sınıfa Bağla](#bind-an-array-to-a-class) bölümü.</span><span class="sxs-lookup"><span data-stu-id="2b200-190">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="2b200-191">Yapılandırma değerleri aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="2b200-191">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="2b200-192">Dizeleri değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="2b200-192">Values are strings.</span></span>
* <span data-ttu-id="2b200-193">Null değerler yapılandırmasında depolanmış veya nesnelere bağlı olamaz.</span><span class="sxs-lookup"><span data-stu-id="2b200-193">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="2b200-194">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="2b200-194">Providers</span></span>

<span data-ttu-id="2b200-195">Aşağıdaki tabloda, ASP.NET Core uygulamaları için kullanılabilir yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2b200-195">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="2b200-196">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="2b200-196">Provider</span></span> | <span data-ttu-id="2b200-197">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="2b200-197">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="2b200-198">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="2b200-198">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="2b200-199">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2b200-199">Azure Key Vault</span></span> |
| [<span data-ttu-id="2b200-200">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-200">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="2b200-201">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="2b200-201">Command-line parameters</span></span> |
| [<span data-ttu-id="2b200-202">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-202">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="2b200-203">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="2b200-203">Custom source</span></span> |
| [<span data-ttu-id="2b200-204">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-204">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="2b200-205">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-205">Environment variables</span></span> |
| [<span data-ttu-id="2b200-206">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-206">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="2b200-207">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="2b200-207">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="2b200-208">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-208">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="2b200-209">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="2b200-209">Directory files</span></span> |
| [<span data-ttu-id="2b200-210">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-210">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="2b200-211">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="2b200-211">In-memory collections</span></span> |
| <span data-ttu-id="2b200-212">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="2b200-212">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="2b200-213">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="2b200-213">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="2b200-214">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="2b200-214">Provider</span></span> | <span data-ttu-id="2b200-215">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="2b200-215">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="2b200-216">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="2b200-216">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="2b200-217">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2b200-217">Azure Key Vault</span></span> |
| [<span data-ttu-id="2b200-218">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="2b200-219">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="2b200-219">Command-line parameters</span></span> |
| [<span data-ttu-id="2b200-220">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="2b200-221">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="2b200-221">Custom source</span></span> |
| [<span data-ttu-id="2b200-222">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="2b200-223">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-223">Environment variables</span></span> |
| [<span data-ttu-id="2b200-224">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="2b200-225">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="2b200-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="2b200-226">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-226">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="2b200-227">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="2b200-227">In-memory collections</span></span> |
| <span data-ttu-id="2b200-228">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="2b200-228">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="2b200-229">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="2b200-229">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="2b200-230">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="2b200-230">Provider</span></span> | <span data-ttu-id="2b200-231">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="2b200-231">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="2b200-232">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-232">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="2b200-233">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="2b200-233">Command-line parameters</span></span> |
| [<span data-ttu-id="2b200-234">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-234">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="2b200-235">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="2b200-235">Custom source</span></span> |
| [<span data-ttu-id="2b200-236">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-236">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="2b200-237">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-237">Environment variables</span></span> |
| [<span data-ttu-id="2b200-238">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-238">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="2b200-239">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="2b200-239">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="2b200-240">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-240">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="2b200-241">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="2b200-241">In-memory collections</span></span> |
| <span data-ttu-id="2b200-242">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="2b200-242">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="2b200-243">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="2b200-243">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="2b200-244">Yapılandırma sağlayıcıları başlatma sırasında belirttiğiniz sırayla yapılandırma kaynaklarını okunur.</span><span class="sxs-lookup"><span data-stu-id="2b200-244">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="2b200-245">Bu konuda açıklanan yapılandırma sağlayıcıları açıklanan alfabetik sırada, bunları kodunuzu düzenleme sırasını değil.</span><span class="sxs-lookup"><span data-stu-id="2b200-245">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="2b200-246">Temel yapılandırma kaynakları için önceliklerinizden uyacak şekilde yapılandırma sağlayıcıları kodunuzu sırası.</span><span class="sxs-lookup"><span data-stu-id="2b200-246">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="2b200-247">Yapılandırma sağlayıcıları, tipik bir dizisidir:</span><span class="sxs-lookup"><span data-stu-id="2b200-247">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="2b200-248">Dosyaları (*appsettings.json*, *appsettings. { Ortam} .json*burada `{Environment}` uygulamanın geçerli barındırma ortamı)</span><span class="sxs-lookup"><span data-stu-id="2b200-248">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="2b200-249">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="2b200-249">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="2b200-250">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki yalnızca)</span><span class="sxs-lookup"><span data-stu-id="2b200-250">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="2b200-251">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-251">Environment variables</span></span>
1. <span data-ttu-id="2b200-252">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2b200-252">Command-line arguments</span></span>

<span data-ttu-id="2b200-253">Komut satırı yapılandırma sağlayıcısı yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılmak komut satırı bağımsız değişkenlerine izin vermek için sağlayıcıları serisinin son konumlandırmak için yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="2b200-253">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-254">Yeni bir başlattığınızda bu sağlayıcıları dizi yerine konur <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-254">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="2b200-255">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="2b200-255">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-256">Sağlayıcıları bu dizi için (ana değil) uygulaması ile oluşturulabilir bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ve bir çağrı, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> yönteminde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2b200-256">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="2b200-257">Yukarıdaki örnekte, ortam adı (`env.EnvironmentName`) ve uygulama derleme adı (`env.ApplicationName`) tarafından sağlanan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="2b200-257">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="2b200-258">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="2b200-258">For more information, see <xref:fundamentals/environments>.</span></span> <span data-ttu-id="2b200-259">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-259">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-260">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-260">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
<span data-ttu-id="2b200-261">biçimindeki telefon numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="2b200-261">.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="2b200-262">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="2b200-262">ConfigureAppConfiguration</span></span>

<span data-ttu-id="2b200-263">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> zaman oluşturmaya ek olarak, uygulamanın yapılandırma sağlayıcıları belirtmek için bir konak eklediğiniz tarafından otomatik olarak <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="2b200-263">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="2b200-264">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-264">Command-line Configuration Provider</span></span>

<span data-ttu-id="2b200-265"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Yapılandırma komut satırı bağımsız değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="2b200-265">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="2b200-266">Komut satırı yapılandırmasını etkinleştirmek için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> genişletme yöntemi bir örneğinde çağrıldığında <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-266">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-267">`AddCommandLine` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-267">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="2b200-268">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="2b200-268">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="2b200-269">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="2b200-269">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2b200-270">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="2b200-270">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="2b200-271">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="2b200-271">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="2b200-272">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="2b200-272">Environment variables.</span></span>

<span data-ttu-id="2b200-273">`CreateDefaultBuilder` Son komut satırı yapılandırma sağlayıcısı ekler.</span><span class="sxs-lookup"><span data-stu-id="2b200-273">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="2b200-274">Çalışma zamanında geçirilen komut satırı bağımsız değişkenleri yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="2b200-274">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="2b200-275">`CreateDefaultBuilder` konak oluşturulduğunda işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="2b200-275">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="2b200-276">Bu nedenle, komut satırı yapılandırma etkinleştirildi tarafından `CreateDefaultBuilder` konak nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-276">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-277">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="2b200-277">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="2b200-278">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2b200-278">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2b200-279">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="2b200-279">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="2b200-280">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-280">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2b200-281">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2b200-281">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="2b200-282">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder` olduğunda `UseConfiguration` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-282">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="2b200-283">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar çağırmak bir `ConfigurationBuilder` ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="2b200-283">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="2b200-284">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-284">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-285">Komut satırı yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-285">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2b200-286">Son çalışma zamanında yapılandırması diğer yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenleri izin vermek için sağlayıcı çağırın.</span><span class="sxs-lookup"><span data-stu-id="2b200-286">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="2b200-287">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2b200-287">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="2b200-288">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="2b200-288">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-289">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-289">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-290">1.x örnek uygulama çağrıları <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> üzerinde bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-290">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="2b200-291">Proje dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="2b200-291">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="2b200-292">Bir komut satırı bağımsız değişken `dotnet run` komutu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="2b200-292">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="2b200-293">Uygulama çalıştıktan sonra bir uygulamaya tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2b200-293">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2b200-294">Çıkış için sağlanan yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çifti içeren gözlemleyin `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="2b200-294">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="2b200-295">Arguments</span><span class="sxs-lookup"><span data-stu-id="2b200-295">Arguments</span></span>

<span data-ttu-id="2b200-296">Değeri bir eşittir işareti gelmelidir (`=`), veya bir önek anahtarı olmalıdır (`--` veya `/`) zaman değeri bir boşluk izler.</span><span class="sxs-lookup"><span data-stu-id="2b200-296">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="2b200-297">Değer bir eşittir işareti kullanılıyorsa, null olabilir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="2b200-297">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="2b200-298">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="2b200-298">Key prefix</span></span>               | <span data-ttu-id="2b200-299">Örnek</span><span class="sxs-lookup"><span data-stu-id="2b200-299">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="2b200-300">Önek yok</span><span class="sxs-lookup"><span data-stu-id="2b200-300">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="2b200-301">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="2b200-301">Two dashes (`--`)</span></span>        | <span data-ttu-id="2b200-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="2b200-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="2b200-303">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="2b200-303">Forward slash (`/`)</span></span>      | <span data-ttu-id="2b200-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="2b200-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="2b200-305">Aynı komut içinde komut satırı bağımsız değişkeni bir eşittir işareti ile bir alanı kullanan anahtar-değer çiftleri kullanan anahtar-değer çiftleri karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="2b200-305">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="2b200-306">Örnek komutları:</span><span class="sxs-lookup"><span data-stu-id="2b200-306">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="2b200-307">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="2b200-307">Switch mappings</span></span>

<span data-ttu-id="2b200-308">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="2b200-308">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="2b200-309">El ile yapı kurarken yapılandırmayla bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, anahtar değişiklik için bir sözlük sağlayabilir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2b200-309">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="2b200-310">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="2b200-310">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="2b200-311">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) anahtar-değer çifti uygulamanın yapılandırmasını döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-311">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="2b200-312">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="2b200-312">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="2b200-313">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="2b200-313">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="2b200-314">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="2b200-314">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="2b200-315">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="2b200-315">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-316">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="2b200-316">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="2b200-317">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b200-317">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="2b200-318">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2b200-318">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2b200-319">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="2b200-319">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="2b200-320">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2b200-320">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="2b200-321">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b200-321">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="2b200-322">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2b200-322">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2b200-323">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="2b200-323">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="2b200-324">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2b200-324">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="2b200-325">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="2b200-325">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="2b200-326">Anahtar</span><span class="sxs-lookup"><span data-stu-id="2b200-326">Key</span></span>       | <span data-ttu-id="2b200-327">Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-327">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="2b200-328">Anahtar eşlemeli anahtarları uygulamayı başlatırken kullandıysanız, yapılandırma sözlüğü tarafından sağlanan anahtardaki yapılandırma değeri alır:</span><span class="sxs-lookup"><span data-stu-id="2b200-328">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="2b200-329">Önceki komutu çalıştırdıktan sonra yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="2b200-329">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="2b200-330">Anahtar</span><span class="sxs-lookup"><span data-stu-id="2b200-330">Key</span></span>               | <span data-ttu-id="2b200-331">Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-331">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="2b200-332">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-332">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="2b200-333"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Yapılandırma ortam değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="2b200-333">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="2b200-334">Ortam değişkenlerini yapılandırma etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-334">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2b200-335">Ortam değişkenleri, iki nokta üst üste ayırıcı hiyerarşik anahtarlarla çalışırken (`:`) tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-335">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="2b200-336">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve virgül ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-336">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="2b200-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) ortam değişkenlerini yapılandırma Sağlayıcısı'nı kullanarak uygulama yapılandırması geçersiz kılabilirsiniz Azure portalında ortam değişkenlerini ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="2b200-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="2b200-338">Daha fazla bilgi için [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="2b200-338">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-339">`AddEnvironmentVariables` ortam değişkenlerini ön eki için otomatik olarak çağrılır `ASPNETCORE_` yeni başlatırken <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-339">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="2b200-340">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="2b200-340">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="2b200-341">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="2b200-341">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2b200-342">Uygulama yapılandırması çağırarak unprefixed ortam değişkenlerinden `AddEnvironmentVariables` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="2b200-342">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="2b200-343">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="2b200-343">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="2b200-344">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="2b200-344">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="2b200-345">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="2b200-345">Command-line arguments.</span></span>

<span data-ttu-id="2b200-346">Yapılandırma kullanıcı parolalarının kurulduktan sonra ortam değişkenlerini yapılandırma sağlayıcısı denir ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="2b200-346">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="2b200-347">Bu konumda sağlayıcıya çağrı ortam değişkenlerini okuma yapılandırması tarafından kullanıcı parolalarını ayarlanmış geçersiz kılmak için çalışma zamanında sağlar ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="2b200-347">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-348">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="2b200-348">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="2b200-349">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="2b200-349">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="2b200-350">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-350">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2b200-351">Çağrı `AddEnvironmentVariables` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-351">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="2b200-352">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2b200-352">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="2b200-353">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="2b200-353">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="2b200-354">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-354">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-355">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2b200-355">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="2b200-356">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="2b200-356">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-357">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="2b200-357">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-358">1.x örnek uygulama çağrıları `AddEnvironmentVariables` üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2b200-358">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="2b200-359">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2b200-359">Run the sample app.</span></span> <span data-ttu-id="2b200-360">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2b200-360">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2b200-361">Çıkış ortam değişkeni için anahtar-değer çifti içeren gözlemleyin `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="2b200-361">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="2b200-362">Değeri, uygulamanın çalıştığı, genellikle ortamın yansıtır `Development` yerel olarak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="2b200-362">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="2b200-363">Ortam değişkenlerini kısa uygulama tarafından işlenen listesini tutmak için aşağıdaki ile başlayan bu ortam değişkenleri uygulama filtreleri:</span><span class="sxs-lookup"><span data-stu-id="2b200-363">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="2b200-364">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="2b200-364">ASPNETCORE_</span></span>
* <span data-ttu-id="2b200-365">URL'leri</span><span class="sxs-lookup"><span data-stu-id="2b200-365">urls</span></span>
* <span data-ttu-id="2b200-366">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="2b200-366">Logging</span></span>
* <span data-ttu-id="2b200-367">ORTAM</span><span class="sxs-lookup"><span data-stu-id="2b200-367">ENVIRONMENT</span></span>
* <span data-ttu-id="2b200-368">contentRoot</span><span class="sxs-lookup"><span data-stu-id="2b200-368">contentRoot</span></span>
* <span data-ttu-id="2b200-369">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="2b200-369">AllowedHosts</span></span>
* <span data-ttu-id="2b200-370">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="2b200-370">applicationName</span></span>
* <span data-ttu-id="2b200-371">komut satırı</span><span class="sxs-lookup"><span data-stu-id="2b200-371">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-372">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Pages/Index.cshtml.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="2b200-372">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-373">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Controllers/HomeController.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="2b200-373">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="2b200-374">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="2b200-374">Prefixes</span></span>

<span data-ttu-id="2b200-375">Uygulamanın yapılandırma yüklendi ortam değişkenleri, bir ön ek sağladığında filtrelenir `AddEnvironmentVariables` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2b200-375">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="2b200-376">Örneğin, ortam değişkenlerini önek filtresi için `CUSTOM_`, yapılandırma sağlayıcısı için önek sağlayın:</span><span class="sxs-lookup"><span data-stu-id="2b200-376">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="2b200-377">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek yapılandırıldıktan devre dışı.</span><span class="sxs-lookup"><span data-stu-id="2b200-377">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-378">Statik kolaylık yöntemi `CreateDefaultBuilder` oluşturur bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> uygulamanın ana bilgisayar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="2b200-378">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="2b200-379">Zaman `WebHostBuilder` olan oluşturulan, kendi ana bilgisayar yapılandırması ön ekine sahip ortam değişkenleri içindeki bulduğu `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2b200-379">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="2b200-380">**Bağlantı dizesi ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="2b200-380">**Connection string prefixes**</span></span>

<span data-ttu-id="2b200-381">Yapılandırma API'si, dört bağlantı dizesi ortam değişkenleri için app ortamı için Azure bağlantı dizelerini yapılandırılmasıyla ilgili özel işleme kurallarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2b200-381">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="2b200-382">Önek yok belirtilirse tabloda gösterilen ön ekine sahip ortam değişkenleri uygulamaya yüklenir `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="2b200-382">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="2b200-383">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="2b200-383">Connection string prefix</span></span> | <span data-ttu-id="2b200-384">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="2b200-384">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="2b200-385">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="2b200-385">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="2b200-386">MySQL</span><span class="sxs-lookup"><span data-stu-id="2b200-386">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="2b200-387">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="2b200-387">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="2b200-388">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2b200-388">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="2b200-389">Ne zaman bir ortam değişkeni bulunur ve herhangi bir tabloda gösterilen dört öneklerini yapılandırmasını yüklendi:</span><span class="sxs-lookup"><span data-stu-id="2b200-389">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="2b200-390">Yapılandırma anahtarı ortam değişkeni ön eki kaldırma ve yapılandırma anahtar bölümünü ekleyerek oluşturulur (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="2b200-390">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="2b200-391">Veritabanı bağlantı sağlayıcısı temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (dışında `CUSTOMCONNSTR_`, belirtilen sağlayıcı yok sahiptir).</span><span class="sxs-lookup"><span data-stu-id="2b200-391">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="2b200-392">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="2b200-392">Environment variable key</span></span> | <span data-ttu-id="2b200-393">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="2b200-393">Converted configuration key</span></span> | <span data-ttu-id="2b200-394">Sağlayıcı Yapılandırması girdisi</span><span class="sxs-lookup"><span data-stu-id="2b200-394">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2b200-395">Yapılandırma girişi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="2b200-395">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2b200-396">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="2b200-396">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="2b200-397">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="2b200-397">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2b200-398">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="2b200-398">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="2b200-399">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="2b200-399">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="2b200-400">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="2b200-400">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="2b200-401">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="2b200-401">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="2b200-402">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-402">File Configuration Provider</span></span>

<span data-ttu-id="2b200-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> yapılandırma dosya sisteminden yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="2b200-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="2b200-404">Aşağıdaki yapılandırma sağlayıcıları için belirli dosya türleri ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="2b200-404">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="2b200-405">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-405">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="2b200-406">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-406">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="2b200-407">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-407">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="2b200-408">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-408">INI Configuration Provider</span></span>

<span data-ttu-id="2b200-409"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Yapılandırma ını dosyası anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="2b200-409">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="2b200-410">INI dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-410">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2b200-411">İki nokta üst üste INI dosya yapılandırması bölüm ayırıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-411">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="2b200-412">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="2b200-412">Overloads permit specifying:</span></span>

* <span data-ttu-id="2b200-413">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="2b200-413">Whether the file is optional.</span></span>
* <span data-ttu-id="2b200-414">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="2b200-414">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2b200-415"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-415">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-416">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="2b200-416">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="2b200-417">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-417">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-418">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-418">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-419">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-419">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2b200-420">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-420">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="2b200-421">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-421">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-422">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-422">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-423">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-423">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-424">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2b200-424">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="2b200-425">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-426">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-427">Genel bir INI yapılandırma dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="2b200-427">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="2b200-428">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="2b200-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2b200-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-429">section0:key0</span></span>
* <span data-ttu-id="2b200-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-430">section0:key1</span></span>
* <span data-ttu-id="2b200-431">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="2b200-431">section1:subsection:key</span></span>
* <span data-ttu-id="2b200-432">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="2b200-432">section2:subsection0:key</span></span>
* <span data-ttu-id="2b200-433">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="2b200-433">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="2b200-434">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-434">JSON Configuration Provider</span></span>

<span data-ttu-id="2b200-435"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Yapılandırma JSON dosyası anahtar-değer çiftlerinden çalışma zamanı sırasında yükler.</span><span class="sxs-lookup"><span data-stu-id="2b200-435">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="2b200-436">JSON dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-436">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2b200-437">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="2b200-437">Overloads permit specifying:</span></span>

* <span data-ttu-id="2b200-438">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="2b200-438">Whether the file is optional.</span></span>
* <span data-ttu-id="2b200-439">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="2b200-439">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2b200-440"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-440">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-441">`AddJsonFile` Yeni bir başlattığınızda iki kez otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-441">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="2b200-442">Yöntem yapılandırmasından yüklenemedi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="2b200-442">The method is called to load configuration from:</span></span>

* <span data-ttu-id="2b200-443">*appSettings.JSON* &ndash; bu dosyayı ilk okuyun.</span><span class="sxs-lookup"><span data-stu-id="2b200-443">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="2b200-444">Dosyanın ortam sürümü tarafından sağlanan değerleri geçersiz kılabilir *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="2b200-444">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="2b200-445">*appSettings. {Ortamı} .json* &ndash; dosyanın ortam sürümünü temel alınarak yüklenir [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="2b200-445">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="2b200-446">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="2b200-446">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="2b200-447">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="2b200-447">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2b200-448">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="2b200-448">Environment variables.</span></span>
* <span data-ttu-id="2b200-449">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="2b200-449">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="2b200-450">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="2b200-450">Command-line arguments.</span></span>

<span data-ttu-id="2b200-451">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2b200-451">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="2b200-452">Bu nedenle, yapılandırma kümesi kullanıcı parolaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri geçersiz kılma *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="2b200-452">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-453">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırma dosyaları için dışında belirtmek için konak oluştururken *appsettings.json* ve *appsettings. { Ortam} .json*:</span><span class="sxs-lookup"><span data-stu-id="2b200-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="2b200-454">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-455">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-456">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2b200-457">Ayrıca doğrudan çağırabilir miyim `AddJsonFile` örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-457">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2b200-458">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-458">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="2b200-459">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-459">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-460">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-460">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-461">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-461">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-462">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2b200-462">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="2b200-463">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-463">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-464">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-464">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-465">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="2b200-465">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-466">2.x örnek uygulamasını statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` iki çağrıları içeren ana bilgisayar oluşturmak için `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="2b200-466">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="2b200-467">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="2b200-467">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-468">1.x örnek uygulama çağrıları `AddJsonFile` iki kez üzerinde bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2b200-468">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="2b200-469">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="2b200-469">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="2b200-470">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2b200-470">Run the sample app.</span></span> <span data-ttu-id="2b200-471">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2b200-471">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2b200-472">Çıkış ortamına bağlı olarak tabloda gösterilen yapılandırması için anahtar-değer çiftleri içeren gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="2b200-472">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="2b200-473">Günlük kaydı yapılandırması tuşlarını iki nokta üst üste (`:`) hiyerarşik ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="2b200-473">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="2b200-474">Anahtar</span><span class="sxs-lookup"><span data-stu-id="2b200-474">Key</span></span>                        | <span data-ttu-id="2b200-475">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="2b200-475">Development Value</span></span> | <span data-ttu-id="2b200-476">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="2b200-476">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="2b200-477">Günlüğe kaydetme: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="2b200-477">Logging:LogLevel:System</span></span>    | <span data-ttu-id="2b200-478">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="2b200-478">Information</span></span>       | <span data-ttu-id="2b200-479">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="2b200-479">Information</span></span>      |
| <span data-ttu-id="2b200-480">Günlüğe kaydetme: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="2b200-480">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="2b200-481">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="2b200-481">Information</span></span>       | <span data-ttu-id="2b200-482">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="2b200-482">Information</span></span>      |
| <span data-ttu-id="2b200-483">Günlüğe kaydetme: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="2b200-483">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="2b200-484">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="2b200-484">Debug</span></span>             | <span data-ttu-id="2b200-485">Hata</span><span class="sxs-lookup"><span data-stu-id="2b200-485">Error</span></span>            |
| <span data-ttu-id="2b200-486">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="2b200-486">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="2b200-487">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-487">XML Configuration Provider</span></span>

<span data-ttu-id="2b200-488"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Yapılandırma XML dosyası anahtar-değer çiftlerinin zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="2b200-488">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="2b200-489">XML dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-489">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2b200-490">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="2b200-490">Overloads permit specifying:</span></span>

* <span data-ttu-id="2b200-491">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="2b200-491">Whether the file is optional.</span></span>
* <span data-ttu-id="2b200-492">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="2b200-492">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2b200-493"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-493">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="2b200-494">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayıldı.</span><span class="sxs-lookup"><span data-stu-id="2b200-494">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="2b200-495">Bir belge türü tanımı (DTD'nin) veya ad alanı dosyasında belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="2b200-495">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-496">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="2b200-496">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="2b200-497">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-497">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-498">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-498">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-499">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-499">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2b200-500">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-500">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="2b200-501">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-501">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-502">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-502">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-503">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-503">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-504">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2b200-504">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="2b200-505">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-505">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-506">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-506">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-507">XML yapılandırma dosyalarını, yinelenen bölümler için ayrı bir öğe adları kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2b200-507">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="2b200-508">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="2b200-508">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2b200-509">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-509">section0:key0</span></span>
* <span data-ttu-id="2b200-510">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-510">section0:key1</span></span>
* <span data-ttu-id="2b200-511">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-511">section1:key0</span></span>
* <span data-ttu-id="2b200-512">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-512">section1:key1</span></span>

<span data-ttu-id="2b200-513">İş öğesi adının aynısını kullanın öğeleri, yinelenen `name` özniteliği öğeleri ayırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="2b200-513">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="2b200-514">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="2b200-514">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2b200-515">Bölüm: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-515">section:section0:key:key0</span></span>
* <span data-ttu-id="2b200-516">Bölüm: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-516">section:section0:key:key1</span></span>
* <span data-ttu-id="2b200-517">Bölüm: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-517">section:section1:key:key0</span></span>
* <span data-ttu-id="2b200-518">Bölüm: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-518">section:section1:key:key1</span></span>

<span data-ttu-id="2b200-519">Öznitelik değerlerini sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b200-519">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="2b200-520">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="2b200-520">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2b200-521">Anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="2b200-521">key:attribute</span></span>
* <span data-ttu-id="2b200-522">Bölüm: anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="2b200-522">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="2b200-523">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-523">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="2b200-524"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bir dizin dosyalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b200-524">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="2b200-525">Anahtar dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="2b200-525">The key is the file name.</span></span> <span data-ttu-id="2b200-526">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="2b200-526">The value contains the file's contents.</span></span> <span data-ttu-id="2b200-527">Dosya başına anahtar yapılandırma sağlayıcısı Docker'da barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-527">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="2b200-528">Dosya başına anahtar yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-528">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="2b200-529">`directoryPath` Dosyalar için mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b200-529">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="2b200-530">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="2b200-530">Overloads permit specifying:</span></span>

* <span data-ttu-id="2b200-531">Bir `Action<KeyPerFileConfigurationSource>` kaynağını yapılandırır temsilci.</span><span class="sxs-lookup"><span data-stu-id="2b200-531">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="2b200-532">Dizin isteğe bağlı olup olmadığını ve dizinin yolu.</span><span class="sxs-lookup"><span data-stu-id="2b200-532">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="2b200-533">Çift alt çizgi (`__`) dosya adları içinde yapılandırma anahtar sınırlayıcı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-533">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="2b200-534">Örneğin, dosya adı `Logging__LogLevel__System` üretir yapılandırma anahtarı `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="2b200-534">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="2b200-535">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="2b200-535">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="2b200-536">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="2b200-536">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="2b200-537">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-537">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-538">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-538">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="2b200-539">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-539">Memory Configuration Provider</span></span>

<span data-ttu-id="2b200-540"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Bir bellek içi koleksiyonu yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b200-540">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="2b200-541">Bellek içi toplama yapılandırması etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2b200-541">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2b200-542">Yapılandırma sağlayıcısı ile başlatılabilir bir `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="2b200-542">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2b200-543">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="2b200-543">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="2b200-544">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-544">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2b200-545">Çağrılırken `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-545">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="2b200-546">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="2b200-546">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-547">Yapılandırmasını uygulamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2b200-547">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="2b200-548">GetValue</span><span class="sxs-lookup"><span data-stu-id="2b200-548">GetValue</span></span>

<span data-ttu-id="2b200-549">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) bir değeri belirtilen bir anahtarla yapılandırmasından ayıklar ve onu belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="2b200-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="2b200-550">Aşırı yükleme anahtarı bulunmazsa, varsayılan bir değer sağlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="2b200-550">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="2b200-551">Aşağıdaki örnek bir dize değeri yapılandırmasından anahtarıyla ayıklar `NumberKey`, değer olarak türleri bir `int`ve değeri değişkeninde depolar `intValue`.</span><span class="sxs-lookup"><span data-stu-id="2b200-551">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="2b200-552">Varsa `NumberKey` yapılandırma anahtarlarını bulunamadığında `intValue` varsayılan değerini alır `99`:</span><span class="sxs-lookup"><span data-stu-id="2b200-552">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="2b200-553">GetSection, GetChildren ve var.</span><span class="sxs-lookup"><span data-stu-id="2b200-553">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="2b200-554">Aşağıdaki örneklerde için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="2b200-554">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="2b200-555">Dört anahtarları içeren bir çift alt bölümleri içerir, iki bölümlerde bulunur:</span><span class="sxs-lookup"><span data-stu-id="2b200-555">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="2b200-556">Dosya yapılandırma okuduğunuzda aşağıdaki benzersiz hiyerarşik anahtarları yapılandırma değerleri tutmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2b200-556">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="2b200-557">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-557">section0:key0</span></span>
* <span data-ttu-id="2b200-558">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-558">section0:key1</span></span>
* <span data-ttu-id="2b200-559">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-559">section1:key0</span></span>
* <span data-ttu-id="2b200-560">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-560">section1:key1</span></span>
* <span data-ttu-id="2b200-561">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-561">section2:subsection0:key0</span></span>
* <span data-ttu-id="2b200-562">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-562">section2:subsection0:key1</span></span>
* <span data-ttu-id="2b200-563">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="2b200-563">section2:subsection1:key0</span></span>
* <span data-ttu-id="2b200-564">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="2b200-564">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="2b200-565">GetSection</span><span class="sxs-lookup"><span data-stu-id="2b200-565">GetSection</span></span>

<span data-ttu-id="2b200-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) yapılandırma bölümüne belirtilen alt anahtarını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="2b200-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="2b200-567">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-567">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-568">Döndürülecek bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> yalnızca anahtar-değer çiftlerini içeren `section1`, çağrı `GetSection` ve bölüm adı verin:</span><span class="sxs-lookup"><span data-stu-id="2b200-568">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="2b200-569">`configSection` Bir değeri, yalnızca bir anahtar ve bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="2b200-569">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="2b200-570">Benzer şekilde, anahtarları değerlerini almak için `section2:subsection0`, çağrı `GetSection` ve bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="2b200-570">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="2b200-571">`GetSection` hiç dönmüyor `null`.</span><span class="sxs-lookup"><span data-stu-id="2b200-571">`GetSection` never returns `null`.</span></span> <span data-ttu-id="2b200-572">Eşleşen bir bölümü olmadığından bulunamazsa, boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="2b200-572">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="2b200-573">Zaman `GetSection` eşleşen bir bölümü döndürür <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuş değil.</span><span class="sxs-lookup"><span data-stu-id="2b200-573">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="2b200-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> bölümü varsa, döndürülür.</span><span class="sxs-lookup"><span data-stu-id="2b200-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="2b200-575">GetChildren</span><span class="sxs-lookup"><span data-stu-id="2b200-575">GetChildren</span></span>

<span data-ttu-id="2b200-576">Bir çağrı [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) üzerinde `section2` alır bir `IEnumerable<IConfigurationSection>` içeren:</span><span class="sxs-lookup"><span data-stu-id="2b200-576">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="2b200-577">Var</span><span class="sxs-lookup"><span data-stu-id="2b200-577">Exists</span></span>

<span data-ttu-id="2b200-578">Kullanım [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) yapılandırma bölümü olup olmadığını belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="2b200-578">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="2b200-579">Belirtilen örnek veri `sectionExists` olduğu `false` olmadığından bir `section2:subsection2` yapılandırma verilerini bir bölümde.</span><span class="sxs-lookup"><span data-stu-id="2b200-579">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="2b200-580">Bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="2b200-580">Bind to a class</span></span>

<span data-ttu-id="2b200-581">Yapılandırma kullanarak ilgili ayar gruplarını temsil eden sınıflar için bağlanabilir *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="2b200-581">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="2b200-582">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="2b200-582">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="2b200-583">Yapılandırma değerleri döndürülür dizeler olarak çağıran <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> oluşumu sağlayan [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesneleri.</span><span class="sxs-lookup"><span data-stu-id="2b200-583">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="2b200-584">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-584">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-585">Örnek uygulamayı içeren bir `Starship` modeli (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="2b200-585">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-586">`starship` Bölümünü *starship.json* örnek uygulamayı yapılandırma yüklemek için JSON yapılandırma sağlayıcısı kullandığında yapılandırma dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="2b200-586">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="2b200-587">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2b200-587">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="2b200-588">Anahtar</span><span class="sxs-lookup"><span data-stu-id="2b200-588">Key</span></span>                   | <span data-ttu-id="2b200-589">Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-589">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="2b200-590">starship: adı</span><span class="sxs-lookup"><span data-stu-id="2b200-590">starship:name</span></span>         | <span data-ttu-id="2b200-591">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="2b200-591">USS Enterprise</span></span>                                    |
| <span data-ttu-id="2b200-592">starship:registry</span><span class="sxs-lookup"><span data-stu-id="2b200-592">starship:registry</span></span>     | <span data-ttu-id="2b200-593">NCC 1701</span><span class="sxs-lookup"><span data-stu-id="2b200-593">NCC-1701</span></span>                                          |
| <span data-ttu-id="2b200-594">starship:class</span><span class="sxs-lookup"><span data-stu-id="2b200-594">starship:class</span></span>        | <span data-ttu-id="2b200-595">Anayasa</span><span class="sxs-lookup"><span data-stu-id="2b200-595">Constitution</span></span>                                      |
| <span data-ttu-id="2b200-596">starship:length</span><span class="sxs-lookup"><span data-stu-id="2b200-596">starship:length</span></span>       | <span data-ttu-id="2b200-597">304.8</span><span class="sxs-lookup"><span data-stu-id="2b200-597">304.8</span></span>                                             |
| <span data-ttu-id="2b200-598">starship: yetkilendirilen</span><span class="sxs-lookup"><span data-stu-id="2b200-598">starship:commissioned</span></span> | <span data-ttu-id="2b200-599">False</span><span class="sxs-lookup"><span data-stu-id="2b200-599">False</span></span>                                             |
| <span data-ttu-id="2b200-600">Ticari marka</span><span class="sxs-lookup"><span data-stu-id="2b200-600">trademark</span></span>             | <span data-ttu-id="2b200-601">Paramount resimleri Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="2b200-601">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="2b200-602">Örnek Uygulama çağrıları `GetSection` ile `starship` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="2b200-602">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="2b200-603">`starship` Anahtar-değer çiftleridir yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="2b200-603">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="2b200-604">`Bind` Bir örneğini geçirerek Altbölüm yöntemi çağrıldığında `Starship` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2b200-604">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="2b200-605">Örnek değerleri bağlandıktan sonra işleme için bir özellik için örneği atanır:</span><span class="sxs-lookup"><span data-stu-id="2b200-605">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

<span data-ttu-id="2b200-606">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-606">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="2b200-607">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="2b200-607">Bind to an object graph</span></span>

<span data-ttu-id="2b200-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Tüm POCO Nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2b200-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="2b200-609">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-609">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2b200-610">Örnek içeren bir `TvShow` olan nesne grafiğini içeren model `Metadata` ve `Actors` sınıfları (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="2b200-610">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-611">Örnek uygulamanın bir *tvshow.xml* yapılandırma verilerini içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="2b200-611">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="2b200-612">Yapılandırma tüm bağlı `TvShow` Nesne grafiği ile `Bind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2b200-612">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="2b200-613">İlişkili örneği, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="2b200-613">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="2b200-614">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="2b200-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="2b200-615">`Get<T>` kullanmaktan daha kullanışlı olan `Bind`.</span><span class="sxs-lookup"><span data-stu-id="2b200-615">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="2b200-616">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle birlikte işlemek için kullanılan özellik doğrudan atanan bağlı örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="2b200-616">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<span data-ttu-id="2b200-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2b200-618">`Get<T>` ASP.NET Core 1.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b200-618">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="2b200-619">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-619">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="2b200-620">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="2b200-620">Bind an array to a class</span></span>

<span data-ttu-id="2b200-621">*Örnek uygulama, bu bölümde açıklanan kavramları göstermektedir.*</span><span class="sxs-lookup"><span data-stu-id="2b200-621">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="2b200-622"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="2b200-622">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2b200-623">Sayısal bir anahtar kesimi sunan herhangi bir dizi biçimi (`:0:`, `:1:`, &hellip; `:{n}:`) dizisi bağlama POCO sınıfı dizisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2b200-623">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="2b200-624">' Bağlama '' bulunduğu [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-624">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="2b200-625">Bağlama, kural olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="2b200-625">Binding is provided by convention.</span></span> <span data-ttu-id="2b200-626">Özel yapılandırma sağlayıcıları dizi bağlama uygulamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2b200-626">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="2b200-627">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="2b200-627">**In-memory array processing**</span></span>

<span data-ttu-id="2b200-628">Yapılandırma anahtarları ve değerleri aşağıdaki tabloda gösterilen göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="2b200-628">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="2b200-629">Anahtar</span><span class="sxs-lookup"><span data-stu-id="2b200-629">Key</span></span>             | <span data-ttu-id="2b200-630">Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-630">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="2b200-631">dizi: girişler: 0</span><span class="sxs-lookup"><span data-stu-id="2b200-631">array:entries:0</span></span> | <span data-ttu-id="2b200-632">value0</span><span class="sxs-lookup"><span data-stu-id="2b200-632">value0</span></span> |
| <span data-ttu-id="2b200-633">dizi: girişler: 1</span><span class="sxs-lookup"><span data-stu-id="2b200-633">array:entries:1</span></span> | <span data-ttu-id="2b200-634">Değer1</span><span class="sxs-lookup"><span data-stu-id="2b200-634">value1</span></span> |
| <span data-ttu-id="2b200-635">dizi: girişler: 2</span><span class="sxs-lookup"><span data-stu-id="2b200-635">array:entries:2</span></span> | <span data-ttu-id="2b200-636">Value2</span><span class="sxs-lookup"><span data-stu-id="2b200-636">value2</span></span> |
| <span data-ttu-id="2b200-637">dizi: girişler: 4</span><span class="sxs-lookup"><span data-stu-id="2b200-637">array:entries:4</span></span> | <span data-ttu-id="2b200-638">Değer4</span><span class="sxs-lookup"><span data-stu-id="2b200-638">value4</span></span> |
| <span data-ttu-id="2b200-639">dizi: girişler: 5</span><span class="sxs-lookup"><span data-stu-id="2b200-639">array:entries:5</span></span> | <span data-ttu-id="2b200-640">Değeri5</span><span class="sxs-lookup"><span data-stu-id="2b200-640">value5</span></span> |

<span data-ttu-id="2b200-641">Bellek yapılandırma sağlayıcısı kullanan örnek uygulamasında, bu anahtarların ve değerlerin yüklenir:</span><span class="sxs-lookup"><span data-stu-id="2b200-641">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="2b200-642">Dizi dizini için bir değer atlar &num;3.</span><span class="sxs-lookup"><span data-stu-id="2b200-642">The array skips a value for index &num;3.</span></span> <span data-ttu-id="2b200-643">Yapılandırma bağlayıcı temizleyin, bu dizi için bir nesne bağlamanın sonucunu gösterilmiştir birazdan haline geldikten bağlama null değerler veya null girişler bağımlı nesneleri oluşturma yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="2b200-643">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="2b200-644">Örnek uygulamada, ilişkili yapılandırma verilerini tutmak bir POCO sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b200-644">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-645">Yapılandırma verilerini nesnesine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="2b200-645">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="2b200-646">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2b200-646">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="2b200-647">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) söz dizimi de kullanılabilir, daha küçük kod sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="2b200-647">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="2b200-648">İlişkili nesne, örneği `ArrayExample`, yapılandırmasından dizisi verileri alır.</span><span class="sxs-lookup"><span data-stu-id="2b200-648">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="2b200-649">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="2b200-649">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="2b200-650">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-650">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="2b200-651">0</span><span class="sxs-lookup"><span data-stu-id="2b200-651">0</span></span>                            | <span data-ttu-id="2b200-652">value0</span><span class="sxs-lookup"><span data-stu-id="2b200-652">value0</span></span>                       |
| <span data-ttu-id="2b200-653">1.</span><span class="sxs-lookup"><span data-stu-id="2b200-653">1</span></span>                            | <span data-ttu-id="2b200-654">Değer1</span><span class="sxs-lookup"><span data-stu-id="2b200-654">value1</span></span>                       |
| <span data-ttu-id="2b200-655">2</span><span class="sxs-lookup"><span data-stu-id="2b200-655">2</span></span>                            | <span data-ttu-id="2b200-656">Value2</span><span class="sxs-lookup"><span data-stu-id="2b200-656">value2</span></span>                       |
| <span data-ttu-id="2b200-657">3</span><span class="sxs-lookup"><span data-stu-id="2b200-657">3</span></span>                            | <span data-ttu-id="2b200-658">Değer4</span><span class="sxs-lookup"><span data-stu-id="2b200-658">value4</span></span>                       |
| <span data-ttu-id="2b200-659">4</span><span class="sxs-lookup"><span data-stu-id="2b200-659">4</span></span>                            | <span data-ttu-id="2b200-660">Değeri5</span><span class="sxs-lookup"><span data-stu-id="2b200-660">value5</span></span>                       |

<span data-ttu-id="2b200-661">Dizin &num;3'te ilişkili nesne için yapılandırma verilerini tutan `array:4` yapılandırma anahtarı ve değeri `value4`.</span><span class="sxs-lookup"><span data-stu-id="2b200-661">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="2b200-662">Bir diziyi içeren yapılandırma verilerini bağlandığında, dizi dizinleri yapılandırma anahtarları yalnızca nesne oluşturma sırasında yapılandırma verileri yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-662">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="2b200-663">Yapılandırma verilerinde bir null değer tutulamıyor ve yapılandırma anahtarları bir dizide bir veya daha fazla dizinlerini atladığında null değerli bir girişi bir bağımlı nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="2b200-663">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="2b200-664">Eksik yapılandırma öğesi için dizin &num;bağlama önce 3 sağlanabilir `ArrayExample` örneği üretir yapılandırma doğru anahtar-değer çifti herhangi bir yapılandırma sağlayıcısı tarafından.</span><span class="sxs-lookup"><span data-stu-id="2b200-664">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="2b200-665">Ek bir JSON yapılandırma sağlayıcısı eksik anahtar-değer çifti ile örneği varsa `ArrayExample.Entries` yapılandırmanın tamamı dizisi ile eşleşen:</span><span class="sxs-lookup"><span data-stu-id="2b200-665">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="2b200-666">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="2b200-666">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2b200-667">İçinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="2b200-667">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2b200-668">İçinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="2b200-668">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="2b200-669">Tabloda belirtilen anahtar-değer çifti yapılandırma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="2b200-669">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="2b200-670">Anahtar</span><span class="sxs-lookup"><span data-stu-id="2b200-670">Key</span></span>             | <span data-ttu-id="2b200-671">Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-671">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="2b200-672">dizi: girişler: 3</span><span class="sxs-lookup"><span data-stu-id="2b200-672">array:entries:3</span></span> | <span data-ttu-id="2b200-673">Değeri3</span><span class="sxs-lookup"><span data-stu-id="2b200-673">value3</span></span> |

<span data-ttu-id="2b200-674">Varsa `ArrayExample` sınıf örneği bağlı dizin için giriş JSON yapılandırma sağlayıcısı içerir sonra &num;3 `ArrayExample.Entries` dizi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="2b200-674">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="2b200-675">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="2b200-675">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="2b200-676">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-676">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="2b200-677">0</span><span class="sxs-lookup"><span data-stu-id="2b200-677">0</span></span>                            | <span data-ttu-id="2b200-678">value0</span><span class="sxs-lookup"><span data-stu-id="2b200-678">value0</span></span>                       |
| <span data-ttu-id="2b200-679">1.</span><span class="sxs-lookup"><span data-stu-id="2b200-679">1</span></span>                            | <span data-ttu-id="2b200-680">Değer1</span><span class="sxs-lookup"><span data-stu-id="2b200-680">value1</span></span>                       |
| <span data-ttu-id="2b200-681">2</span><span class="sxs-lookup"><span data-stu-id="2b200-681">2</span></span>                            | <span data-ttu-id="2b200-682">Value2</span><span class="sxs-lookup"><span data-stu-id="2b200-682">value2</span></span>                       |
| <span data-ttu-id="2b200-683">3</span><span class="sxs-lookup"><span data-stu-id="2b200-683">3</span></span>                            | <span data-ttu-id="2b200-684">Değeri3</span><span class="sxs-lookup"><span data-stu-id="2b200-684">value3</span></span>                       |
| <span data-ttu-id="2b200-685">4</span><span class="sxs-lookup"><span data-stu-id="2b200-685">4</span></span>                            | <span data-ttu-id="2b200-686">Değer4</span><span class="sxs-lookup"><span data-stu-id="2b200-686">value4</span></span>                       |
| <span data-ttu-id="2b200-687">5</span><span class="sxs-lookup"><span data-stu-id="2b200-687">5</span></span>                            | <span data-ttu-id="2b200-688">Değeri5</span><span class="sxs-lookup"><span data-stu-id="2b200-688">value5</span></span>                       |

<span data-ttu-id="2b200-689">**JSON dizisi işleme**</span><span class="sxs-lookup"><span data-stu-id="2b200-689">**JSON array processing**</span></span>

<span data-ttu-id="2b200-690">Bir JSON dosyası bir dizi varsa, dizi öğeleri bölümünde sıfır tabanlı dizine sahip için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2b200-690">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="2b200-691">Aşağıdaki yapılandırma dosyasında `subsection` dizisi:</span><span class="sxs-lookup"><span data-stu-id="2b200-691">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="2b200-692">JSON yapılandırma sağlayıcısı, aşağıdaki anahtar-değer çiftlerine yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="2b200-692">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="2b200-693">Anahtar</span><span class="sxs-lookup"><span data-stu-id="2b200-693">Key</span></span>                     | <span data-ttu-id="2b200-694">Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-694">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="2b200-695">json_array:key</span><span class="sxs-lookup"><span data-stu-id="2b200-695">json_array:key</span></span>          | <span data-ttu-id="2b200-696">Değera</span><span class="sxs-lookup"><span data-stu-id="2b200-696">valueA</span></span> |
| <span data-ttu-id="2b200-697">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="2b200-697">json_array:subsection:0</span></span> | <span data-ttu-id="2b200-698">Değerb</span><span class="sxs-lookup"><span data-stu-id="2b200-698">valueB</span></span> |
| <span data-ttu-id="2b200-699">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="2b200-699">json_array:subsection:1</span></span> | <span data-ttu-id="2b200-700">valueC</span><span class="sxs-lookup"><span data-stu-id="2b200-700">valueC</span></span> |
| <span data-ttu-id="2b200-701">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="2b200-701">json_array:subsection:2</span></span> | <span data-ttu-id="2b200-702">Değerli</span><span class="sxs-lookup"><span data-stu-id="2b200-702">valueD</span></span> |

<span data-ttu-id="2b200-703">Örnek uygulamada, aşağıdaki POCO sınıfı yapılandırma anahtar-değer çiftleri bağlamak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b200-703">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-704">Bağlama sonra `JsonArrayExample.Key` değerine `valueA`.</span><span class="sxs-lookup"><span data-stu-id="2b200-704">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="2b200-705">Alt değerleri POCO dizi özelliğinde depolanır `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="2b200-705">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="2b200-706">`JsonArrayExample.Subsection` Dizin</span><span class="sxs-lookup"><span data-stu-id="2b200-706">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="2b200-707">`JsonArrayExample.Subsection` Değer</span><span class="sxs-lookup"><span data-stu-id="2b200-707">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="2b200-708">0</span><span class="sxs-lookup"><span data-stu-id="2b200-708">0</span></span>                                   | <span data-ttu-id="2b200-709">Değerb</span><span class="sxs-lookup"><span data-stu-id="2b200-709">valueB</span></span>                              |
| <span data-ttu-id="2b200-710">1.</span><span class="sxs-lookup"><span data-stu-id="2b200-710">1</span></span>                                   | <span data-ttu-id="2b200-711">valueC</span><span class="sxs-lookup"><span data-stu-id="2b200-711">valueC</span></span>                              |
| <span data-ttu-id="2b200-712">2</span><span class="sxs-lookup"><span data-stu-id="2b200-712">2</span></span>                                   | <span data-ttu-id="2b200-713">Değerli</span><span class="sxs-lookup"><span data-stu-id="2b200-713">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="2b200-714">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="2b200-714">Custom configuration provider</span></span>

<span data-ttu-id="2b200-715">Örnek uygulamayı yapılandırma anahtar-değer çiftleri kullanarak bir veritabanını okuyan bir temel yapılandırma sağlayıcısı oluşturma gösterilmektedir [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="2b200-715">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="2b200-716">Sağlayıcı, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="2b200-716">The provider has the following characteristics:</span></span>

* <span data-ttu-id="2b200-717">EF bellek içi veritabanına tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2b200-717">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="2b200-718">Bir bağlantı dizesi gerektiren bir veritabanı kullanmak için ikincil uygulama `ConfigurationBuilder` başka bir yapılandırma sağlayıcısı bağlantı dizesinden sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="2b200-718">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="2b200-719">Sağlayıcı bir veritabanı tablosu, başlangıç yapılandırmasını içine okur.</span><span class="sxs-lookup"><span data-stu-id="2b200-719">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="2b200-720">Sağlayıcı anahtarı başına temelinde veritabanını sorgulamak değil.</span><span class="sxs-lookup"><span data-stu-id="2b200-720">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="2b200-721">Uygulama başlatılır sahip olduktan sonra uygulamanın yapılandırma üzerinde hiçbir etkisi kadar veritabanını güncelleme yeniden üzerinde değişiklik uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="2b200-721">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="2b200-722">Tanımlayan bir `EFConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık.</span><span class="sxs-lookup"><span data-stu-id="2b200-722">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="2b200-723">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b200-723">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-724">Ekleme bir `EFConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="2b200-724">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="2b200-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b200-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-726">Uygulayan bir sınıf oluşturma <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="2b200-726">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="2b200-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b200-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-728">Özel yapılandırma sağlayıcısını devralarak oluşturma <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="2b200-728">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="2b200-729">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır.</span><span class="sxs-lookup"><span data-stu-id="2b200-729">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="2b200-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b200-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-731">Bir `AddEFConfiguration` genişletme yöntemi izin veren yapılandırması kaynağına ekleme bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2b200-731">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="2b200-732">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b200-732">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2b200-733">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigurationProvider` içinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2b200-733">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="2b200-734">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2b200-734">Access configuration during startup</span></span>

<span data-ttu-id="2b200-735">Ekleme `IConfiguration` içine `Startup` oluşturucuya erişim yapılandırma değerlerini `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2b200-735">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2b200-736">Erişim yapılandırmaya `Startup.Configure`, ya da ekleme `IConfiguration` doğrudan yöntem veya oluşturucu örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="2b200-736">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="2b200-737">Başlangıç kullanışlı yöntemler kullanarak yapılandırma erişme ilişkin bir örnek için bkz [uygulama başlatma: Yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="2b200-737">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="2b200-738">Erişim yapılandırmasında bir Razor sayfaları sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="2b200-738">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="2b200-739">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](xref:Microsoft.Extensions.Configuration) ve ekleme <xref:Microsoft.Extensions.Configuration.IConfiguration> sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="2b200-739">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="2b200-740">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="2b200-740">In a Razor Pages page:</span></span>

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

<span data-ttu-id="2b200-741">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="2b200-741">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="2b200-742">Dış bütünleştirilmiş koddan Yapılandırması Ekle</span><span class="sxs-lookup"><span data-stu-id="2b200-742">Add configuration from an external assembly</span></span>

<span data-ttu-id="2b200-743">Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2b200-743">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="2b200-744">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="2b200-744">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b200-745">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2b200-745">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="2b200-746">Microsoft yapılandırma hakkında ayrıntılı bir inceleme</span><span class="sxs-lookup"><span data-stu-id="2b200-746">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
