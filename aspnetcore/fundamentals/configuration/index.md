---
title: ASP.NET core'da yapılandırma
author: guardrex
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/24/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 3f7588f9ba18e300f5947e8bb0daf2e72d580a94
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223160"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="bb993-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bb993-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="bb993-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bb993-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bb993-105">ASP.NET core'da uygulama yapılandırması tarafından kurulan anahtar-değer çiftleri temel *yapılandırma sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="bb993-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="bb993-106">Yapılandırma sağlayıcıları, yapılandırma kaynaklarını çeşitli anahtar-değer çiftlerine yapılandırma verilerini okuyun:</span><span class="sxs-lookup"><span data-stu-id="bb993-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="bb993-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bb993-107">Azure Key Vault</span></span>
* <span data-ttu-id="bb993-108">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="bb993-108">Command-line arguments</span></span>
* <span data-ttu-id="bb993-109">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="bb993-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="bb993-110">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="bb993-110">Directory files</span></span>
* <span data-ttu-id="bb993-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="bb993-111">Environment variables</span></span>
* <span data-ttu-id="bb993-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="bb993-112">In-memory .NET objects</span></span>
* <span data-ttu-id="bb993-113">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="bb993-113">Settings files</span></span>

<span data-ttu-id="bb993-114">Yapılandırma paketlerini ortak yapılandırma sağlayıcısı senaryoları dahil edilecek [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-114">Configuration packages for common configuration provider scenarios are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="bb993-115">Kodu izleyen ve örnek uygulama kullanma örnekleri <xref:Microsoft.Extensions.Configuration> ad alanı:</span><span class="sxs-lookup"><span data-stu-id="bb993-115">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="bb993-116">*Seçenekleri deseni* bu konuda açıklanan yapılandırma kavramları bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="bb993-116">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="bb993-117">Seçenekler, ilgili ayar gruplarını temsil etmek için sınıflar kullanır.</span><span class="sxs-lookup"><span data-stu-id="bb993-117">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="bb993-118">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="bb993-118">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="bb993-119">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bb993-119">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="bb993-120">Uygulama yapılandırması barındırın</span><span class="sxs-lookup"><span data-stu-id="bb993-120">Host vs. app configuration</span></span>

<span data-ttu-id="bb993-121">Uygulama yapılandırılmış ve başlatıldı, önce bir *konak* başlatılan ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-121">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="bb993-122">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="bb993-122">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="bb993-123">Bu konuda açıklanan yapılandırma sağlayıcıları kullanarak, hem uygulama hem de konak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-123">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="bb993-124">Ana bilgisayar yapılandırma anahtar-değer çiftleri uygulamanın genel yapılandırmasının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="bb993-124">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="bb993-125">Yapılandırma sağlayıcıları konak oluşturulduğunda kullanılan yapılandırma ve yapılandırma kaynaklarını nasıl etkileyeceğini nasıl barındırmak daha fazla bilgi için bkz: [konak](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="bb993-125">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="bb993-126">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bb993-126">Default configuration</span></span>

<span data-ttu-id="bb993-127">Web uygulamaları üzerinde ASP.NET Core tabanlı [yeni dotnet](/dotnet/core/tools/dotnet-new) şablonları çağrı <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="bb993-127">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="bb993-128"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aşağıdaki sırayla uygulaması için varsayılan yapılandırması sağlar:</span><span class="sxs-lookup"><span data-stu-id="bb993-128"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="bb993-129">Ana bilgisayar yapılandırma öğesinden sağlanır:</span><span class="sxs-lookup"><span data-stu-id="bb993-129">Host configuration is provided from:</span></span>
  * <span data-ttu-id="bb993-130">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bb993-130">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="bb993-131">Önek (`ASPNETCORE_`) yapılandırma anahtar-değer çiftleri yüklendiğinde çıkartılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-131">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="bb993-132">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bb993-132">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="bb993-133">Uygulama yapılandırma öğesinden sağlanır:</span><span class="sxs-lookup"><span data-stu-id="bb993-133">App configuration is provided from:</span></span>
  * <span data-ttu-id="bb993-134">*appSettings.JSON* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bb993-134">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="bb993-135">*appSettings. {Ortamı} .json* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bb993-135">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="bb993-136">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="bb993-136">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="bb993-137">Ortam değişkenlerini kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bb993-137">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="bb993-138">Özel bir önek kullanılıyorsa (örneğin, `PREFIX_` ile `.AddEnvironmentVariables(prefix: "PREFIX_")`), yapılandırma anahtar-değer çiftleri yüklendiğinde önek yapılandırıldıktan.</span><span class="sxs-lookup"><span data-stu-id="bb993-138">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="bb993-139">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bb993-139">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="bb993-140">Yapılandırma sağlayıcıları, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="bb993-140">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="bb993-141">Konak hakkında daha fazla bilgi ve <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, bkz: <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="bb993-141">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="bb993-142">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="bb993-142">Security</span></span>

<span data-ttu-id="bb993-143">Aşağıdaki en iyi benimseme:</span><span class="sxs-lookup"><span data-stu-id="bb993-143">Adopt the following best practices:</span></span>

* <span data-ttu-id="bb993-144">Hiçbir zaman parolaları ve diğer hassas verileri yapılandırma sağlayıcısı kodda veya düz metin yapılandırma dosyalarında depolayın.</span><span class="sxs-lookup"><span data-stu-id="bb993-144">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="bb993-145">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="bb993-145">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="bb993-146">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="bb993-146">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="bb993-147">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [gizli dizi Yöneticisi ile geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) (depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. hassas verileri).</span><span class="sxs-lookup"><span data-stu-id="bb993-147">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="bb993-148">Gizli dizi Yöneticisi'ni dosya yapılandırma sağlayıcısı bir JSON dosyası yerel sistemdeki kullanıcı gizli dizileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="bb993-148">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="bb993-149">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="bb993-149">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="bb993-150">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) uygulama gizli anahtarlarının güvenli bir şekilde depolanması için bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="bb993-150">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="bb993-151">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="bb993-151">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="bb993-152">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="bb993-152">Hierarchical configuration data</span></span>

<span data-ttu-id="bb993-153">Yapılandırma API configuration anahtarlarında bir sınırlayıcı kullanarak hiyerarşik veri düzleştirme tarafından hiyerarşik yapılandırma verileri koruma özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="bb993-153">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="bb993-154">Aşağıdaki JSON dosyasında iki bölüm yapılandırılmış bir hiyerarşide dört anahtarları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="bb993-154">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="bb993-155">Dosya yapılandırma okuduğunuzda benzersiz anahtarlar özgün hiyerarşik veri yapısını yapılandırma kaynağı korumak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bb993-155">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="bb993-156">Bölümler ve anahtarlar ile bir iki nokta üst üste kullanımını düzleştirilir (`:`) özgün yapıyı korumak için:</span><span class="sxs-lookup"><span data-stu-id="bb993-156">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="bb993-157">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-157">section0:key0</span></span>
* <span data-ttu-id="bb993-158">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-158">section0:key1</span></span>
* <span data-ttu-id="bb993-159">section1:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-159">section1:key0</span></span>
* <span data-ttu-id="bb993-160">section1:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-160">section1:key1</span></span>

<span data-ttu-id="bb993-161"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemlerdir bölümler ve yapılandırma verilerini bir bölümde alt yalıtmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bb993-161"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="bb993-162">Bu yöntem daha sonra açıklanmıştır [GetSection GetChildren ve Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="bb993-162">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="bb993-163">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-163">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="bb993-164">Kurallar</span><span class="sxs-lookup"><span data-stu-id="bb993-164">Conventions</span></span>

<span data-ttu-id="bb993-165">Uygulama başlangıcında, yapılandırma kaynaklarını yapılandırma sağlayıcıları belirttiğiniz sırayla okunur.</span><span class="sxs-lookup"><span data-stu-id="bb993-165">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="bb993-166">Değişiklik algılama uygulayan yapılandırma sağlayıcıları temel alınan bir ayar değiştiğinde yapılandırmayı yeniden yükle seçeneğine sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb993-166">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="bb993-167">Örneğin, dosya yapılandırma (Bu konunun ilerleyen bölümlerinde açıklanmıştır) sağlayıcısı ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılama uygulayın.</span><span class="sxs-lookup"><span data-stu-id="bb993-167">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="bb993-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> uygulamanın kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="bb993-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="bb993-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> bir Razor sayfaları yerleştirilebilir <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı yapılandırmasını almak için:</span><span class="sxs-lookup"><span data-stu-id="bb993-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="bb993-170">Bunlar ana bilgisayar tarafından kurduktan olduğunda değil olarak yapılandırma sağlayıcıları DI, faydalanamaz.</span><span class="sxs-lookup"><span data-stu-id="bb993-170">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="bb993-171">Yapılandırma anahtarları, aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="bb993-171">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="bb993-172">Anahtarlar büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="bb993-172">Keys are case-insensitive.</span></span> <span data-ttu-id="bb993-173">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="bb993-173">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="bb993-174">Aynı veya farklı yapılandırma sağlayıcıları tarafından aynı anahtar için bir değer ayarlarsanız, anahtarda ayarlanan son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="bb993-174">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="bb993-175">Hiyerarşik anahtarları</span><span class="sxs-lookup"><span data-stu-id="bb993-175">Hierarchical keys</span></span>
  * <span data-ttu-id="bb993-176">Yapılandırma API'sinin, iki nokta üst üste ayırıcı (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="bb993-176">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="bb993-177">Ortam değişkenleri, iki nokta üst üste ayırıcı tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="bb993-177">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="bb993-178">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve bir iki nokta üst üste dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="bb993-178">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="bb993-179">Azure anahtar Kasası'nda hiyerarşik tuşlarını `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="bb993-179">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="bb993-180">Gizli dizileri uygulama yapılandırma yüklendiğinde tireler iki nokta üst üste ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb993-180">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="bb993-181"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="bb993-181">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="bb993-182">Dizi bağlama açıklanan [bir dizi bir sınıfa Bağla](#bind-an-array-to-a-class) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bb993-182">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="bb993-183">Yapılandırma değerleri aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="bb993-183">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="bb993-184">Dizeleri değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="bb993-184">Values are strings.</span></span>
* <span data-ttu-id="bb993-185">Null değerler yapılandırmasında depolanmış veya nesnelere bağlı olamaz.</span><span class="sxs-lookup"><span data-stu-id="bb993-185">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="bb993-186">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="bb993-186">Providers</span></span>

<span data-ttu-id="bb993-187">Aşağıdaki tabloda, ASP.NET Core uygulamaları için kullanılabilir yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bb993-187">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="bb993-188">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="bb993-188">Provider</span></span> | <span data-ttu-id="bb993-189">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="bb993-189">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="bb993-190">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="bb993-190">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="bb993-191">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bb993-191">Azure Key Vault</span></span> |
| [<span data-ttu-id="bb993-192">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-192">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="bb993-193">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="bb993-193">Command-line parameters</span></span> |
| [<span data-ttu-id="bb993-194">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-194">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="bb993-195">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="bb993-195">Custom source</span></span> |
| [<span data-ttu-id="bb993-196">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-196">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="bb993-197">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="bb993-197">Environment variables</span></span> |
| [<span data-ttu-id="bb993-198">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-198">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="bb993-199">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="bb993-199">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="bb993-200">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-200">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="bb993-201">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="bb993-201">Directory files</span></span> |
| [<span data-ttu-id="bb993-202">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-202">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="bb993-203">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="bb993-203">In-memory collections</span></span> |
| <span data-ttu-id="bb993-204">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="bb993-204">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="bb993-205">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="bb993-205">File in the user profile directory</span></span> |

<span data-ttu-id="bb993-206">Yapılandırma sağlayıcıları başlatma sırasında belirttiğiniz sırayla yapılandırma kaynaklarını okunur.</span><span class="sxs-lookup"><span data-stu-id="bb993-206">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="bb993-207">Bu konuda açıklanan yapılandırma sağlayıcıları açıklanan alfabetik sırada, bunları kodunuzu düzenleme sırasını değil.</span><span class="sxs-lookup"><span data-stu-id="bb993-207">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="bb993-208">Temel yapılandırma kaynakları için önceliklerinizden uyacak şekilde yapılandırma sağlayıcıları kodunuzu sırası.</span><span class="sxs-lookup"><span data-stu-id="bb993-208">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="bb993-209">Yapılandırma sağlayıcıları, tipik bir dizisidir:</span><span class="sxs-lookup"><span data-stu-id="bb993-209">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="bb993-210">Dosyaları (*appsettings.json*, *appsettings. { Ortam} .json*burada `{Environment}` uygulamanın geçerli barındırma ortamı)</span><span class="sxs-lookup"><span data-stu-id="bb993-210">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="bb993-211">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="bb993-211">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="bb993-212">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki yalnızca)</span><span class="sxs-lookup"><span data-stu-id="bb993-212">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="bb993-213">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="bb993-213">Environment variables</span></span>
1. <span data-ttu-id="bb993-214">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="bb993-214">Command-line arguments</span></span>

<span data-ttu-id="bb993-215">Komut satırı yapılandırma sağlayıcısı yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılmak komut satırı bağımsız değişkenlerine izin vermek için sağlayıcıları serisinin son konumlandırmak için yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="bb993-215">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="bb993-216">Yeni bir başlattığınızda bu sağlayıcıları dizi yerine konur <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-216">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="bb993-217">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bb993-217">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="bb993-218">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="bb993-218">ConfigureAppConfiguration</span></span>

<span data-ttu-id="bb993-219">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> zaman oluşturmaya ek olarak, uygulamanın yapılandırma sağlayıcıları belirtmek için bir konak eklediğiniz tarafından otomatik olarak <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="bb993-219">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

<span data-ttu-id="bb993-220">Uygulama için sağlanan yapılandırma <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın başlatma sırasında kullanılabilir dahil olmak üzere `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bb993-220">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bb993-221">Daha fazla bilgi için [erişim yapılandırması başlatılırken](#access-configuration-during-startup) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bb993-221">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="bb993-222">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-222">Command-line Configuration Provider</span></span>

<span data-ttu-id="bb993-223"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Yapılandırma komut satırı bağımsız değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="bb993-223">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="bb993-224">Komut satırı yapılandırmasını etkinleştirmek için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> genişletme yöntemi bir örneğinde çağrıldığında <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-224">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bb993-225">`AddCommandLine` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-225">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="bb993-226">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bb993-226">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="bb993-227">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="bb993-227">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="bb993-228">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="bb993-228">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="bb993-229">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="bb993-229">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="bb993-230">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="bb993-230">Environment variables.</span></span>

<span data-ttu-id="bb993-231">`CreateDefaultBuilder` Son komut satırı yapılandırma sağlayıcısı ekler.</span><span class="sxs-lookup"><span data-stu-id="bb993-231">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="bb993-232">Çalışma zamanında geçirilen komut satırı bağımsız değişkenleri yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="bb993-232">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="bb993-233">`CreateDefaultBuilder` konak oluşturulduğunda işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="bb993-233">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="bb993-234">Bu nedenle, komut satırı yapılandırma etkinleştirildi tarafından `CreateDefaultBuilder` konak nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="bb993-234">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="bb993-235">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="bb993-235">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="bb993-236">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bb993-236">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="bb993-237">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="bb993-237">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="bb993-238">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="bb993-238">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bb993-239">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="bb993-239">**Example**</span></span>

<span data-ttu-id="bb993-240">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-240">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="bb993-241">Proje dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="bb993-241">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="bb993-242">Bir komut satırı bağımsız değişken `dotnet run` komutu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="bb993-242">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="bb993-243">Uygulama çalıştıktan sonra bir uygulamaya tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bb993-243">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="bb993-244">Çıkış için sağlanan yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çifti içeren gözlemleyin `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="bb993-244">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="bb993-245">Arguments</span><span class="sxs-lookup"><span data-stu-id="bb993-245">Arguments</span></span>

<span data-ttu-id="bb993-246">Değeri bir eşittir işareti gelmelidir (`=`), veya bir önek anahtarı olmalıdır (`--` veya `/`) zaman değeri bir boşluk izler.</span><span class="sxs-lookup"><span data-stu-id="bb993-246">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="bb993-247">Değer bir eşittir işareti kullanılıyorsa, null olabilir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="bb993-247">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="bb993-248">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="bb993-248">Key prefix</span></span>               | <span data-ttu-id="bb993-249">Örnek</span><span class="sxs-lookup"><span data-stu-id="bb993-249">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="bb993-250">Önek yok</span><span class="sxs-lookup"><span data-stu-id="bb993-250">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="bb993-251">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="bb993-251">Two dashes (`--`)</span></span>        | <span data-ttu-id="bb993-252">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="bb993-252">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="bb993-253">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="bb993-253">Forward slash (`/`)</span></span>      | <span data-ttu-id="bb993-254">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="bb993-254">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="bb993-255">Aynı komut içinde komut satırı bağımsız değişkeni bir eşittir işareti ile bir alanı kullanan anahtar-değer çiftleri kullanan anahtar-değer çiftleri karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="bb993-255">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="bb993-256">Örnek komutları:</span><span class="sxs-lookup"><span data-stu-id="bb993-256">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="bb993-257">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="bb993-257">Switch mappings</span></span>

<span data-ttu-id="bb993-258">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="bb993-258">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="bb993-259">El ile yapı kurarken yapılandırmayla bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, anahtar değişiklik için bir sözlük sağlayabilir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bb993-259">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="bb993-260">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="bb993-260">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="bb993-261">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) anahtar-değer çifti uygulamanın yapılandırmasını döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="bb993-261">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="bb993-262">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="bb993-262">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="bb993-263">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="bb993-263">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="bb993-264">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="bb993-264">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="bb993-265">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="bb993-265">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="bb993-266">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="bb993-266">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="bb993-267">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bb993-267">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="bb993-268">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bb993-268">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="bb993-269">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="bb993-269">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="bb993-270">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bb993-270">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="bb993-271">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="bb993-271">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="bb993-272">Anahtar</span><span class="sxs-lookup"><span data-stu-id="bb993-272">Key</span></span>       | <span data-ttu-id="bb993-273">Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-273">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="bb993-274">Anahtar eşlemeli anahtarları uygulamayı başlatırken kullandıysanız, yapılandırma sözlüğü tarafından sağlanan anahtardaki yapılandırma değeri alır:</span><span class="sxs-lookup"><span data-stu-id="bb993-274">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="bb993-275">Önceki komutu çalıştırdıktan sonra yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="bb993-275">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="bb993-276">Anahtar</span><span class="sxs-lookup"><span data-stu-id="bb993-276">Key</span></span>               | <span data-ttu-id="bb993-277">Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-277">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="bb993-278">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-278">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="bb993-279"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Yapılandırma ortam değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="bb993-279">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="bb993-280">Ortam değişkenlerini yapılandırma etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-280">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="bb993-281">[Azure App Service](https://azure.microsoft.com/services/app-service/) ortam değişkenlerini yapılandırma Sağlayıcısı'nı kullanarak uygulama yapılandırması geçersiz kılabilirsiniz Azure portalında ortam değişkenlerini ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="bb993-281">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="bb993-282">Daha fazla bilgi için [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="bb993-282">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="bb993-283">`AddEnvironmentVariables` ortam değişkenlerini ön eki için otomatik olarak çağrılır `ASPNETCORE_` yeni başlatırken <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-283">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="bb993-284">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bb993-284">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="bb993-285">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="bb993-285">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="bb993-286">Uygulama yapılandırması çağırarak unprefixed ortam değişkenlerinden `AddEnvironmentVariables` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="bb993-286">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="bb993-287">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="bb993-287">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="bb993-288">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="bb993-288">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="bb993-289">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="bb993-289">Command-line arguments.</span></span>

<span data-ttu-id="bb993-290">Yapılandırma kullanıcı parolalarının kurulduktan sonra ortam değişkenlerini yapılandırma sağlayıcısı denir ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="bb993-290">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="bb993-291">Bu konumda sağlayıcıya çağrı ortam değişkenlerini okuma yapılandırması tarafından kullanıcı parolalarını ayarlanmış geçersiz kılmak için çalışma zamanında sağlar ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="bb993-291">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="bb993-292">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="bb993-292">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="bb993-293">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="bb993-293">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
                // Call AddEnvironmentVariables last if you need to allow
                // environment variables to override values from other 
                // providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bb993-294">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="bb993-294">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="bb993-295">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="bb993-295">**Example**</span></span>

<span data-ttu-id="bb993-296">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="bb993-296">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="bb993-297">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bb993-297">Run the sample app.</span></span> <span data-ttu-id="bb993-298">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bb993-298">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="bb993-299">Çıkış ortam değişkeni için anahtar-değer çifti içeren gözlemleyin `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="bb993-299">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="bb993-300">Değeri, uygulamanın çalıştığı, genellikle ortamın yansıtır `Development` yerel olarak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="bb993-300">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="bb993-301">Ortam değişkenlerini kısa uygulama tarafından işlenen listesini tutmak için aşağıdaki ile başlayan bu ortam değişkenleri uygulama filtreleri:</span><span class="sxs-lookup"><span data-stu-id="bb993-301">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="bb993-302">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="bb993-302">ASPNETCORE_</span></span>
* <span data-ttu-id="bb993-303">URL'leri</span><span class="sxs-lookup"><span data-stu-id="bb993-303">urls</span></span>
* <span data-ttu-id="bb993-304">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="bb993-304">Logging</span></span>
* <span data-ttu-id="bb993-305">ORTAM</span><span class="sxs-lookup"><span data-stu-id="bb993-305">ENVIRONMENT</span></span>
* <span data-ttu-id="bb993-306">contentRoot</span><span class="sxs-lookup"><span data-stu-id="bb993-306">contentRoot</span></span>
* <span data-ttu-id="bb993-307">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="bb993-307">AllowedHosts</span></span>
* <span data-ttu-id="bb993-308">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="bb993-308">applicationName</span></span>
* <span data-ttu-id="bb993-309">komut satırı</span><span class="sxs-lookup"><span data-stu-id="bb993-309">CommandLine</span></span>

<span data-ttu-id="bb993-310">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Pages/Index.cshtml.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="bb993-310">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="bb993-311">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="bb993-311">Prefixes</span></span>

<span data-ttu-id="bb993-312">Uygulamanın yapılandırma yüklendi ortam değişkenleri, bir ön ek sağladığında filtrelenir `AddEnvironmentVariables` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bb993-312">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="bb993-313">Örneğin, ortam değişkenlerini önek filtresi için `CUSTOM_`, yapılandırma sağlayıcısı için önek sağlayın:</span><span class="sxs-lookup"><span data-stu-id="bb993-313">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="bb993-314">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek yapılandırıldıktan devre dışı.</span><span class="sxs-lookup"><span data-stu-id="bb993-314">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="bb993-315">Statik kolaylık yöntemi `CreateDefaultBuilder` oluşturur bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> uygulamanın ana bilgisayar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="bb993-315">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="bb993-316">Zaman `WebHostBuilder` olan oluşturulan, kendi ana bilgisayar yapılandırması ön ekine sahip ortam değişkenleri içindeki bulduğu `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="bb993-316">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="bb993-317">**Bağlantı dizesi ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="bb993-317">**Connection string prefixes**</span></span>

<span data-ttu-id="bb993-318">Yapılandırma API'si, dört bağlantı dizesi ortam değişkenleri için app ortamı için Azure bağlantı dizelerini yapılandırılmasıyla ilgili özel işleme kurallarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bb993-318">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="bb993-319">Önek yok belirtilirse tabloda gösterilen ön ekine sahip ortam değişkenleri uygulamaya yüklenir `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="bb993-319">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="bb993-320">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="bb993-320">Connection string prefix</span></span> | <span data-ttu-id="bb993-321">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="bb993-321">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="bb993-322">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="bb993-322">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="bb993-323">MySQL</span><span class="sxs-lookup"><span data-stu-id="bb993-323">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="bb993-324">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="bb993-324">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="bb993-325">SQL Server</span><span class="sxs-lookup"><span data-stu-id="bb993-325">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="bb993-326">Ne zaman bir ortam değişkeni bulunur ve herhangi bir tabloda gösterilen dört öneklerini yapılandırmasını yüklendi:</span><span class="sxs-lookup"><span data-stu-id="bb993-326">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="bb993-327">Yapılandırma anahtarı ortam değişkeni ön eki kaldırma ve yapılandırma anahtar bölümünü ekleyerek oluşturulur (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="bb993-327">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="bb993-328">Veritabanı bağlantı sağlayıcısı temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (dışında `CUSTOMCONNSTR_`, belirtilen sağlayıcı yok sahiptir).</span><span class="sxs-lookup"><span data-stu-id="bb993-328">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="bb993-329">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="bb993-329">Environment variable key</span></span> | <span data-ttu-id="bb993-330">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="bb993-330">Converted configuration key</span></span> | <span data-ttu-id="bb993-331">Sağlayıcı Yapılandırması girdisi</span><span class="sxs-lookup"><span data-stu-id="bb993-331">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bb993-332">Yapılandırma girişi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="bb993-332">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bb993-333">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="bb993-333">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="bb993-334">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="bb993-334">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bb993-335">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="bb993-335">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="bb993-336">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="bb993-336">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bb993-337">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="bb993-337">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="bb993-338">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="bb993-338">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="bb993-339">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-339">File Configuration Provider</span></span>

<span data-ttu-id="bb993-340"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> yapılandırma dosya sisteminden yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="bb993-340"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="bb993-341">Aşağıdaki yapılandırma sağlayıcıları için belirli dosya türleri ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="bb993-341">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="bb993-342">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-342">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="bb993-343">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-343">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="bb993-344">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-344">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="bb993-345">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-345">INI Configuration Provider</span></span>

<span data-ttu-id="bb993-346"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Yapılandırma ını dosyası anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="bb993-346">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="bb993-347">INI dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-347">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bb993-348">İki nokta üst üste INI dosya yapılandırması bölüm ayırıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bb993-348">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="bb993-349">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="bb993-349">Overloads permit specifying:</span></span>

* <span data-ttu-id="bb993-350">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="bb993-350">Whether the file is optional.</span></span>
* <span data-ttu-id="bb993-351">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="bb993-351">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="bb993-352"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-352">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="bb993-353">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="bb993-353">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddIniFile(
                    "config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bb993-354">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-354">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bb993-355">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-355">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-356">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="bb993-356">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bb993-357">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-357">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bb993-358">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-358">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-359">Genel bir INI yapılandırma dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="bb993-359">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="bb993-360">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="bb993-360">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bb993-361">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-361">section0:key0</span></span>
* <span data-ttu-id="bb993-362">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-362">section0:key1</span></span>
* <span data-ttu-id="bb993-363">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="bb993-363">section1:subsection:key</span></span>
* <span data-ttu-id="bb993-364">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="bb993-364">section2:subsection0:key</span></span>
* <span data-ttu-id="bb993-365">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="bb993-365">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="bb993-366">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-366">JSON Configuration Provider</span></span>

<span data-ttu-id="bb993-367"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Yapılandırma JSON dosyası anahtar-değer çiftlerinden çalışma zamanı sırasında yükler.</span><span class="sxs-lookup"><span data-stu-id="bb993-367">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="bb993-368">JSON dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-368">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bb993-369">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="bb993-369">Overloads permit specifying:</span></span>

* <span data-ttu-id="bb993-370">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="bb993-370">Whether the file is optional.</span></span>
* <span data-ttu-id="bb993-371">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="bb993-371">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="bb993-372"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-372">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="bb993-373">`AddJsonFile` Yeni bir başlattığınızda iki kez otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-373">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="bb993-374">Yöntem yapılandırmasından yüklenemedi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="bb993-374">The method is called to load configuration from:</span></span>

* <span data-ttu-id="bb993-375">*appSettings.JSON* &ndash; bu dosyayı ilk okuyun.</span><span class="sxs-lookup"><span data-stu-id="bb993-375">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="bb993-376">Dosyanın ortam sürümü tarafından sağlanan değerleri geçersiz kılabilir *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="bb993-376">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="bb993-377">*appSettings. {Ortamı} .json* &ndash; dosyanın ortam sürümünü temel alınarak yüklenir [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="bb993-377">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="bb993-378">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bb993-378">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="bb993-379">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="bb993-379">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="bb993-380">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="bb993-380">Environment variables.</span></span>
* <span data-ttu-id="bb993-381">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="bb993-381">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="bb993-382">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="bb993-382">Command-line arguments.</span></span>

<span data-ttu-id="bb993-383">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bb993-383">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="bb993-384">Bu nedenle, yapılandırma kümesi kullanıcı parolaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri geçersiz kılma *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="bb993-384">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="bb993-385">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırma dosyaları için dışında belirtmek için konak oluştururken *appsettings.json* ve *appsettings. { Ortam} .json*:</span><span class="sxs-lookup"><span data-stu-id="bb993-385">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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
                config.AddJsonFile(
                    "config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bb993-386">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-386">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bb993-387">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-387">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-388">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="bb993-388">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bb993-389">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-389">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bb993-390">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-390">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-391">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="bb993-391">**Example**</span></span>

<span data-ttu-id="bb993-392">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` iki çağrıları içeren ana bilgisayar oluşturmak için `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="bb993-392">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="bb993-393">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="bb993-393">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="bb993-394">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bb993-394">Run the sample app.</span></span> <span data-ttu-id="bb993-395">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bb993-395">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="bb993-396">Çıkış ortamına bağlı olarak tabloda gösterilen yapılandırması için anahtar-değer çiftleri içeren gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="bb993-396">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="bb993-397">Günlük kaydı yapılandırması tuşlarını iki nokta üst üste (`:`) hiyerarşik ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="bb993-397">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="bb993-398">Anahtar</span><span class="sxs-lookup"><span data-stu-id="bb993-398">Key</span></span>                        | <span data-ttu-id="bb993-399">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="bb993-399">Development Value</span></span> | <span data-ttu-id="bb993-400">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="bb993-400">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="bb993-401">Günlüğe kaydetme: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="bb993-401">Logging:LogLevel:System</span></span>    | <span data-ttu-id="bb993-402">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="bb993-402">Information</span></span>       | <span data-ttu-id="bb993-403">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="bb993-403">Information</span></span>      |
| <span data-ttu-id="bb993-404">Günlüğe kaydetme: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="bb993-404">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="bb993-405">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="bb993-405">Information</span></span>       | <span data-ttu-id="bb993-406">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="bb993-406">Information</span></span>      |
| <span data-ttu-id="bb993-407">Günlüğe kaydetme: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="bb993-407">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="bb993-408">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="bb993-408">Debug</span></span>             | <span data-ttu-id="bb993-409">Hata</span><span class="sxs-lookup"><span data-stu-id="bb993-409">Error</span></span>            |
| <span data-ttu-id="bb993-410">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="bb993-410">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="bb993-411">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-411">XML Configuration Provider</span></span>

<span data-ttu-id="bb993-412"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Yapılandırma XML dosyası anahtar-değer çiftlerinin zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="bb993-412">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="bb993-413">XML dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-413">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bb993-414">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="bb993-414">Overloads permit specifying:</span></span>

* <span data-ttu-id="bb993-415">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="bb993-415">Whether the file is optional.</span></span>
* <span data-ttu-id="bb993-416">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="bb993-416">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="bb993-417"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-417">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="bb993-418">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayıldı.</span><span class="sxs-lookup"><span data-stu-id="bb993-418">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="bb993-419">Bir belge türü tanımı (DTD'nin) veya ad alanı dosyasında belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="bb993-419">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="bb993-420">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="bb993-420">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddXmlFile(
                    "config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bb993-421">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-421">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bb993-422">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-422">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-423">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="bb993-423">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bb993-424">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-424">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bb993-425">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-425">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-426">XML yapılandırma dosyalarını, yinelenen bölümler için ayrı bir öğe adları kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bb993-426">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="bb993-427">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="bb993-427">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bb993-428">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-428">section0:key0</span></span>
* <span data-ttu-id="bb993-429">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-429">section0:key1</span></span>
* <span data-ttu-id="bb993-430">section1:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-430">section1:key0</span></span>
* <span data-ttu-id="bb993-431">section1:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-431">section1:key1</span></span>

<span data-ttu-id="bb993-432">İş öğesi adının aynısını kullanın öğeleri, yinelenen `name` özniteliği öğeleri ayırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="bb993-432">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="bb993-433">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="bb993-433">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bb993-434">Bölüm: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-434">section:section0:key:key0</span></span>
* <span data-ttu-id="bb993-435">Bölüm: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-435">section:section0:key:key1</span></span>
* <span data-ttu-id="bb993-436">Bölüm: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-436">section:section1:key:key0</span></span>
* <span data-ttu-id="bb993-437">Bölüm: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-437">section:section1:key:key1</span></span>

<span data-ttu-id="bb993-438">Öznitelik değerlerini sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bb993-438">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="bb993-439">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="bb993-439">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bb993-440">Anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="bb993-440">key:attribute</span></span>
* <span data-ttu-id="bb993-441">Bölüm: anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="bb993-441">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="bb993-442">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-442">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="bb993-443"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bir dizin dosyalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="bb993-443">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="bb993-444">Anahtar dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="bb993-444">The key is the file name.</span></span> <span data-ttu-id="bb993-445">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="bb993-445">The value contains the file's contents.</span></span> <span data-ttu-id="bb993-446">Dosya başına anahtar yapılandırma sağlayıcısı Docker'da barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-446">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="bb993-447">Dosya başına anahtar yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-447">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="bb993-448">`directoryPath` Dosyalar için mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bb993-448">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="bb993-449">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="bb993-449">Overloads permit specifying:</span></span>

* <span data-ttu-id="bb993-450">Bir `Action<KeyPerFileConfigurationSource>` kaynağını yapılandırır temsilci.</span><span class="sxs-lookup"><span data-stu-id="bb993-450">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="bb993-451">Dizin isteğe bağlı olup olmadığını ve dizinin yolu.</span><span class="sxs-lookup"><span data-stu-id="bb993-451">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="bb993-452">Çift alt çizgi (`__`) dosya adları içinde yapılandırma anahtar sınırlayıcı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-452">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="bb993-453">Örneğin, dosya adı `Logging__LogLevel__System` üretir yapılandırma anahtarı `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="bb993-453">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="bb993-454">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="bb993-454">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                var path = Path.Combine(
                    Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bb993-455">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bb993-455">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bb993-456">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-456">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-457">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="bb993-457">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="bb993-458">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-458">Memory Configuration Provider</span></span>

<span data-ttu-id="bb993-459"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Bir bellek içi koleksiyonu yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="bb993-459">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="bb993-460">Bellek içi toplama yapılandırması etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bb993-460">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bb993-461">Yapılandırma sağlayıcısı ile başlatılabilir bir `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="bb993-461">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="bb993-462">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="bb993-462">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="bb993-463">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="bb993-463">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="bb993-464">GetValue</span><span class="sxs-lookup"><span data-stu-id="bb993-464">GetValue</span></span>

<span data-ttu-id="bb993-465">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) bir değeri belirtilen bir anahtarla yapılandırmasından ayıklar ve onu belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="bb993-465">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="bb993-466">Aşırı yükleme anahtarı bulunmazsa, varsayılan bir değer sağlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="bb993-466">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="bb993-467">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="bb993-467">The following example:</span></span>

* <span data-ttu-id="bb993-468">Dize değeri yapılandırmasından anahtarıyla ayıklar `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="bb993-468">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="bb993-469">Varsa `NumberKey` yapılandırma anahtarları, varsayılan değeri bulunamadığında `99` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-469">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="bb993-470">Değer olarak türleri bir `int`.</span><span class="sxs-lookup"><span data-stu-id="bb993-470">Types the value as an `int`.</span></span>
* <span data-ttu-id="bb993-471">İçinde bir değer depolar `NumberConfig` özellik sayfası tarafından kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="bb993-471">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="bb993-472">GetSection, GetChildren ve var.</span><span class="sxs-lookup"><span data-stu-id="bb993-472">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="bb993-473">Aşağıdaki örneklerde için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="bb993-473">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="bb993-474">Dört anahtarları içeren bir çift alt bölümleri içerir, iki bölümlerde bulunur:</span><span class="sxs-lookup"><span data-stu-id="bb993-474">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="bb993-475">Dosya yapılandırma okuduğunuzda aşağıdaki benzersiz hiyerarşik anahtarları yapılandırma değerleri tutmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="bb993-475">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="bb993-476">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-476">section0:key0</span></span>
* <span data-ttu-id="bb993-477">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-477">section0:key1</span></span>
* <span data-ttu-id="bb993-478">section1:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-478">section1:key0</span></span>
* <span data-ttu-id="bb993-479">section1:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-479">section1:key1</span></span>
* <span data-ttu-id="bb993-480">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-480">section2:subsection0:key0</span></span>
* <span data-ttu-id="bb993-481">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-481">section2:subsection0:key1</span></span>
* <span data-ttu-id="bb993-482">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="bb993-482">section2:subsection1:key0</span></span>
* <span data-ttu-id="bb993-483">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="bb993-483">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="bb993-484">GetSection</span><span class="sxs-lookup"><span data-stu-id="bb993-484">GetSection</span></span>

<span data-ttu-id="bb993-485">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) yapılandırma bölümüne belirtilen alt anahtarını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="bb993-485">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="bb993-486">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-486">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-487">Döndürülecek bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> yalnızca anahtar-değer çiftlerini içeren `section1`, çağrı `GetSection` ve bölüm adı verin:</span><span class="sxs-lookup"><span data-stu-id="bb993-487">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="bb993-488">`configSection` Bir değeri, yalnızca bir anahtar ve bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="bb993-488">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="bb993-489">Benzer şekilde, anahtarları değerlerini almak için `section2:subsection0`, çağrı `GetSection` ve bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="bb993-489">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="bb993-490">`GetSection` hiç dönmüyor `null`.</span><span class="sxs-lookup"><span data-stu-id="bb993-490">`GetSection` never returns `null`.</span></span> <span data-ttu-id="bb993-491">Eşleşen bir bölümü olmadığından bulunamazsa, boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="bb993-491">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="bb993-492">Zaman `GetSection` eşleşen bir bölümü döndürür <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuş değil.</span><span class="sxs-lookup"><span data-stu-id="bb993-492">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="bb993-493">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> bölümü varsa, döndürülür.</span><span class="sxs-lookup"><span data-stu-id="bb993-493">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="bb993-494">GetChildren</span><span class="sxs-lookup"><span data-stu-id="bb993-494">GetChildren</span></span>

<span data-ttu-id="bb993-495">Bir çağrı [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) üzerinde `section2` alır bir `IEnumerable<IConfigurationSection>` içeren:</span><span class="sxs-lookup"><span data-stu-id="bb993-495">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="bb993-496">Var</span><span class="sxs-lookup"><span data-stu-id="bb993-496">Exists</span></span>

<span data-ttu-id="bb993-497">Kullanım [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) yapılandırma bölümü olup olmadığını belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="bb993-497">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="bb993-498">Belirtilen örnek veri `sectionExists` olduğu `false` olmadığından bir `section2:subsection2` yapılandırma verilerini bir bölümde.</span><span class="sxs-lookup"><span data-stu-id="bb993-498">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="bb993-499">Bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="bb993-499">Bind to a class</span></span>

<span data-ttu-id="bb993-500">Yapılandırma kullanarak ilgili ayar gruplarını temsil eden sınıflar için bağlanabilir *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="bb993-500">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="bb993-501">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="bb993-501">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="bb993-502">Yapılandırma değerleri döndürülür dizeler olarak çağıran <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> oluşumu sağlayan [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesneleri.</span><span class="sxs-lookup"><span data-stu-id="bb993-502">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="bb993-503">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-503">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-504">Örnek uygulamayı içeren bir `Starship` modeli (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="bb993-504">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="bb993-505">`starship` Bölümünü *starship.json* örnek uygulamayı yapılandırma yüklemek için JSON yapılandırma sağlayıcısı kullandığında yapılandırma dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="bb993-505">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="bb993-506">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="bb993-506">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="bb993-507">Anahtar</span><span class="sxs-lookup"><span data-stu-id="bb993-507">Key</span></span>                   | <span data-ttu-id="bb993-508">Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-508">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="bb993-509">starship: adı</span><span class="sxs-lookup"><span data-stu-id="bb993-509">starship:name</span></span>         | <span data-ttu-id="bb993-510">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="bb993-510">USS Enterprise</span></span>                                    |
| <span data-ttu-id="bb993-511">starship:registry</span><span class="sxs-lookup"><span data-stu-id="bb993-511">starship:registry</span></span>     | <span data-ttu-id="bb993-512">NCC 1701</span><span class="sxs-lookup"><span data-stu-id="bb993-512">NCC-1701</span></span>                                          |
| <span data-ttu-id="bb993-513">starship:class</span><span class="sxs-lookup"><span data-stu-id="bb993-513">starship:class</span></span>        | <span data-ttu-id="bb993-514">Anayasa</span><span class="sxs-lookup"><span data-stu-id="bb993-514">Constitution</span></span>                                      |
| <span data-ttu-id="bb993-515">starship:length</span><span class="sxs-lookup"><span data-stu-id="bb993-515">starship:length</span></span>       | <span data-ttu-id="bb993-516">304.8</span><span class="sxs-lookup"><span data-stu-id="bb993-516">304.8</span></span>                                             |
| <span data-ttu-id="bb993-517">starship: yetkilendirilen</span><span class="sxs-lookup"><span data-stu-id="bb993-517">starship:commissioned</span></span> | <span data-ttu-id="bb993-518">False</span><span class="sxs-lookup"><span data-stu-id="bb993-518">False</span></span>                                             |
| <span data-ttu-id="bb993-519">Ticari marka</span><span class="sxs-lookup"><span data-stu-id="bb993-519">trademark</span></span>             | <span data-ttu-id="bb993-520">Paramount resimleri Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="bb993-520">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="bb993-521">Örnek Uygulama çağrıları `GetSection` ile `starship` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="bb993-521">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="bb993-522">`starship` Anahtar-değer çiftleridir yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="bb993-522">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="bb993-523">`Bind` Bir örneğini geçirerek Altbölüm yöntemi çağrıldığında `Starship` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bb993-523">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="bb993-524">Örnek değerleri bağlandıktan sonra işleme için bir özellik için örneği atanır:</span><span class="sxs-lookup"><span data-stu-id="bb993-524">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="bb993-525">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-525">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="bb993-526">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="bb993-526">Bind to an object graph</span></span>

<span data-ttu-id="bb993-527"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Tüm POCO Nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bb993-527"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="bb993-528">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-528">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-529">Örnek içeren bir `TvShow` olan nesne grafiğini içeren model `Metadata` ve `Actors` sınıfları (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="bb993-529">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="bb993-530">Örnek uygulamanın bir *tvshow.xml* yapılandırma verilerini içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="bb993-530">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="bb993-531">Yapılandırma tüm bağlı `TvShow` Nesne grafiği ile `Bind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bb993-531">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="bb993-532">İlişkili örneği, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="bb993-532">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="bb993-533">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="bb993-533">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="bb993-534">`Get<T>` kullanmaktan daha kullanışlı olan `Bind`.</span><span class="sxs-lookup"><span data-stu-id="bb993-534">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="bb993-535">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle birlikte işlemek için kullanılan özellik doğrudan atanan bağlı örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="bb993-535">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="bb993-536"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-536"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="bb993-537">`Get<T>` ASP.NET Core 1.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bb993-537">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="bb993-538">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-538">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="bb993-539">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="bb993-539">Bind an array to a class</span></span>

<span data-ttu-id="bb993-540">*Örnek uygulama, bu bölümde açıklanan kavramları göstermektedir.*</span><span class="sxs-lookup"><span data-stu-id="bb993-540">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="bb993-541"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="bb993-541">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="bb993-542">Sayısal bir anahtar kesimi sunan herhangi bir dizi biçimi (`:0:`, `:1:`, &hellip; `:{n}:`) dizisi bağlama POCO sınıfı dizisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bb993-542">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="bb993-543">' Bağlama '' bulunduğu [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-543">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="bb993-544">Bağlama, kural olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="bb993-544">Binding is provided by convention.</span></span> <span data-ttu-id="bb993-545">Özel yapılandırma sağlayıcıları dizi bağlama uygulamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="bb993-545">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="bb993-546">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="bb993-546">**In-memory array processing**</span></span>

<span data-ttu-id="bb993-547">Yapılandırma anahtarları ve değerleri aşağıdaki tabloda gösterilen göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="bb993-547">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="bb993-548">Anahtar</span><span class="sxs-lookup"><span data-stu-id="bb993-548">Key</span></span>             | <span data-ttu-id="bb993-549">Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-549">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="bb993-550">dizi: girişler: 0</span><span class="sxs-lookup"><span data-stu-id="bb993-550">array:entries:0</span></span> | <span data-ttu-id="bb993-551">value0</span><span class="sxs-lookup"><span data-stu-id="bb993-551">value0</span></span> |
| <span data-ttu-id="bb993-552">dizi: girişler: 1</span><span class="sxs-lookup"><span data-stu-id="bb993-552">array:entries:1</span></span> | <span data-ttu-id="bb993-553">Değer1</span><span class="sxs-lookup"><span data-stu-id="bb993-553">value1</span></span> |
| <span data-ttu-id="bb993-554">dizi: girişler: 2</span><span class="sxs-lookup"><span data-stu-id="bb993-554">array:entries:2</span></span> | <span data-ttu-id="bb993-555">Value2</span><span class="sxs-lookup"><span data-stu-id="bb993-555">value2</span></span> |
| <span data-ttu-id="bb993-556">dizi: girişler: 4</span><span class="sxs-lookup"><span data-stu-id="bb993-556">array:entries:4</span></span> | <span data-ttu-id="bb993-557">Değer4</span><span class="sxs-lookup"><span data-stu-id="bb993-557">value4</span></span> |
| <span data-ttu-id="bb993-558">dizi: girişler: 5</span><span class="sxs-lookup"><span data-stu-id="bb993-558">array:entries:5</span></span> | <span data-ttu-id="bb993-559">Değeri5</span><span class="sxs-lookup"><span data-stu-id="bb993-559">value5</span></span> |

<span data-ttu-id="bb993-560">Bellek yapılandırma sağlayıcısı kullanan örnek uygulamasında, bu anahtarların ve değerlerin yüklenir:</span><span class="sxs-lookup"><span data-stu-id="bb993-560">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

<span data-ttu-id="bb993-561">Dizi dizini için bir değer atlar &num;3.</span><span class="sxs-lookup"><span data-stu-id="bb993-561">The array skips a value for index &num;3.</span></span> <span data-ttu-id="bb993-562">Yapılandırma bağlayıcı temizleyin, bu dizi için bir nesne bağlamanın sonucunu gösterilmiştir birazdan haline geldikten bağlama null değerler veya null girişler bağımlı nesneleri oluşturma yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="bb993-562">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="bb993-563">Örnek uygulamada, ilişkili yapılandırma verilerini tutmak bir POCO sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bb993-563">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="bb993-564">Yapılandırma verilerini nesnesine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="bb993-564">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="bb993-565">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bb993-565">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bb993-566">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) söz dizimi de kullanılabilir, daha küçük kod sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="bb993-566">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="bb993-567">İlişkili nesne, örneği `ArrayExample`, yapılandırmasından dizisi verileri alır.</span><span class="sxs-lookup"><span data-stu-id="bb993-567">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="bb993-568">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="bb993-568">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="bb993-569">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-569">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="bb993-570">0</span><span class="sxs-lookup"><span data-stu-id="bb993-570">0</span></span>                            | <span data-ttu-id="bb993-571">value0</span><span class="sxs-lookup"><span data-stu-id="bb993-571">value0</span></span>                       |
| <span data-ttu-id="bb993-572">1.</span><span class="sxs-lookup"><span data-stu-id="bb993-572">1</span></span>                            | <span data-ttu-id="bb993-573">Değer1</span><span class="sxs-lookup"><span data-stu-id="bb993-573">value1</span></span>                       |
| <span data-ttu-id="bb993-574">2</span><span class="sxs-lookup"><span data-stu-id="bb993-574">2</span></span>                            | <span data-ttu-id="bb993-575">Value2</span><span class="sxs-lookup"><span data-stu-id="bb993-575">value2</span></span>                       |
| <span data-ttu-id="bb993-576">3</span><span class="sxs-lookup"><span data-stu-id="bb993-576">3</span></span>                            | <span data-ttu-id="bb993-577">Değer4</span><span class="sxs-lookup"><span data-stu-id="bb993-577">value4</span></span>                       |
| <span data-ttu-id="bb993-578">4</span><span class="sxs-lookup"><span data-stu-id="bb993-578">4</span></span>                            | <span data-ttu-id="bb993-579">Değeri5</span><span class="sxs-lookup"><span data-stu-id="bb993-579">value5</span></span>                       |

<span data-ttu-id="bb993-580">Dizin &num;3'te ilişkili nesne için yapılandırma verilerini tutan `array:4` yapılandırma anahtarı ve değeri `value4`.</span><span class="sxs-lookup"><span data-stu-id="bb993-580">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="bb993-581">Bir diziyi içeren yapılandırma verilerini bağlandığında, dizi dizinleri yapılandırma anahtarları yalnızca nesne oluşturma sırasında yapılandırma verileri yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-581">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="bb993-582">Yapılandırma verilerinde bir null değer tutulamıyor ve yapılandırma anahtarları bir dizide bir veya daha fazla dizinlerini atladığında null değerli bir girişi bir bağımlı nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bb993-582">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="bb993-583">Eksik yapılandırma öğesi için dizin &num;bağlama önce 3 sağlanabilir `ArrayExample` örneği üretir yapılandırma doğru anahtar-değer çifti herhangi bir yapılandırma sağlayıcısı tarafından.</span><span class="sxs-lookup"><span data-stu-id="bb993-583">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="bb993-584">Ek bir JSON yapılandırma sağlayıcısı eksik anahtar-değer çifti ile örneği varsa `ArrayExample.Entries` yapılandırmanın tamamı dizisi ile eşleşen:</span><span class="sxs-lookup"><span data-stu-id="bb993-584">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="bb993-585">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="bb993-585">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="bb993-586">İçinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bb993-586">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="bb993-587">Tabloda belirtilen anahtar-değer çifti yapılandırma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="bb993-587">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="bb993-588">Anahtar</span><span class="sxs-lookup"><span data-stu-id="bb993-588">Key</span></span>             | <span data-ttu-id="bb993-589">Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-589">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="bb993-590">dizi: girişler: 3</span><span class="sxs-lookup"><span data-stu-id="bb993-590">array:entries:3</span></span> | <span data-ttu-id="bb993-591">Değeri3</span><span class="sxs-lookup"><span data-stu-id="bb993-591">value3</span></span> |

<span data-ttu-id="bb993-592">Varsa `ArrayExample` sınıf örneği bağlı dizin için giriş JSON yapılandırma sağlayıcısı içerir sonra &num;3 `ArrayExample.Entries` dizi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="bb993-592">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="bb993-593">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="bb993-593">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="bb993-594">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-594">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="bb993-595">0</span><span class="sxs-lookup"><span data-stu-id="bb993-595">0</span></span>                            | <span data-ttu-id="bb993-596">value0</span><span class="sxs-lookup"><span data-stu-id="bb993-596">value0</span></span>                       |
| <span data-ttu-id="bb993-597">1.</span><span class="sxs-lookup"><span data-stu-id="bb993-597">1</span></span>                            | <span data-ttu-id="bb993-598">Değer1</span><span class="sxs-lookup"><span data-stu-id="bb993-598">value1</span></span>                       |
| <span data-ttu-id="bb993-599">2</span><span class="sxs-lookup"><span data-stu-id="bb993-599">2</span></span>                            | <span data-ttu-id="bb993-600">Value2</span><span class="sxs-lookup"><span data-stu-id="bb993-600">value2</span></span>                       |
| <span data-ttu-id="bb993-601">3</span><span class="sxs-lookup"><span data-stu-id="bb993-601">3</span></span>                            | <span data-ttu-id="bb993-602">Değeri3</span><span class="sxs-lookup"><span data-stu-id="bb993-602">value3</span></span>                       |
| <span data-ttu-id="bb993-603">4</span><span class="sxs-lookup"><span data-stu-id="bb993-603">4</span></span>                            | <span data-ttu-id="bb993-604">Değer4</span><span class="sxs-lookup"><span data-stu-id="bb993-604">value4</span></span>                       |
| <span data-ttu-id="bb993-605">5</span><span class="sxs-lookup"><span data-stu-id="bb993-605">5</span></span>                            | <span data-ttu-id="bb993-606">Değeri5</span><span class="sxs-lookup"><span data-stu-id="bb993-606">value5</span></span>                       |

<span data-ttu-id="bb993-607">**JSON dizisi işleme**</span><span class="sxs-lookup"><span data-stu-id="bb993-607">**JSON array processing**</span></span>

<span data-ttu-id="bb993-608">Bir JSON dosyası bir dizi varsa, dizi öğeleri bölümünde sıfır tabanlı dizine sahip için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bb993-608">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="bb993-609">Aşağıdaki yapılandırma dosyasında `subsection` dizisi:</span><span class="sxs-lookup"><span data-stu-id="bb993-609">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="bb993-610">JSON yapılandırma sağlayıcısı, aşağıdaki anahtar-değer çiftlerine yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="bb993-610">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="bb993-611">Anahtar</span><span class="sxs-lookup"><span data-stu-id="bb993-611">Key</span></span>                     | <span data-ttu-id="bb993-612">Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-612">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="bb993-613">json_array:key</span><span class="sxs-lookup"><span data-stu-id="bb993-613">json_array:key</span></span>          | <span data-ttu-id="bb993-614">Değera</span><span class="sxs-lookup"><span data-stu-id="bb993-614">valueA</span></span> |
| <span data-ttu-id="bb993-615">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="bb993-615">json_array:subsection:0</span></span> | <span data-ttu-id="bb993-616">Değerb</span><span class="sxs-lookup"><span data-stu-id="bb993-616">valueB</span></span> |
| <span data-ttu-id="bb993-617">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="bb993-617">json_array:subsection:1</span></span> | <span data-ttu-id="bb993-618">valueC</span><span class="sxs-lookup"><span data-stu-id="bb993-618">valueC</span></span> |
| <span data-ttu-id="bb993-619">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="bb993-619">json_array:subsection:2</span></span> | <span data-ttu-id="bb993-620">Değerli</span><span class="sxs-lookup"><span data-stu-id="bb993-620">valueD</span></span> |

<span data-ttu-id="bb993-621">Örnek uygulamada, aşağıdaki POCO sınıfı yapılandırma anahtar-değer çiftleri bağlamak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="bb993-621">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="bb993-622">Bağlama sonra `JsonArrayExample.Key` değerine `valueA`.</span><span class="sxs-lookup"><span data-stu-id="bb993-622">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="bb993-623">Alt değerleri POCO dizi özelliğinde depolanır `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="bb993-623">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="bb993-624">`JsonArrayExample.Subsection` Dizin</span><span class="sxs-lookup"><span data-stu-id="bb993-624">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="bb993-625">`JsonArrayExample.Subsection` Değer</span><span class="sxs-lookup"><span data-stu-id="bb993-625">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="bb993-626">0</span><span class="sxs-lookup"><span data-stu-id="bb993-626">0</span></span>                                   | <span data-ttu-id="bb993-627">Değerb</span><span class="sxs-lookup"><span data-stu-id="bb993-627">valueB</span></span>                              |
| <span data-ttu-id="bb993-628">1.</span><span class="sxs-lookup"><span data-stu-id="bb993-628">1</span></span>                                   | <span data-ttu-id="bb993-629">valueC</span><span class="sxs-lookup"><span data-stu-id="bb993-629">valueC</span></span>                              |
| <span data-ttu-id="bb993-630">2</span><span class="sxs-lookup"><span data-stu-id="bb993-630">2</span></span>                                   | <span data-ttu-id="bb993-631">Değerli</span><span class="sxs-lookup"><span data-stu-id="bb993-631">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="bb993-632">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="bb993-632">Custom configuration provider</span></span>

<span data-ttu-id="bb993-633">Örnek uygulamayı yapılandırma anahtar-değer çiftleri kullanarak bir veritabanını okuyan bir temel yapılandırma sağlayıcısı oluşturma gösterilmektedir [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="bb993-633">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="bb993-634">Sağlayıcı, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="bb993-634">The provider has the following characteristics:</span></span>

* <span data-ttu-id="bb993-635">EF bellek içi veritabanına tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bb993-635">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="bb993-636">Bir bağlantı dizesi gerektiren bir veritabanı kullanmak için ikincil uygulama `ConfigurationBuilder` başka bir yapılandırma sağlayıcısı bağlantı dizesinden sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="bb993-636">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="bb993-637">Sağlayıcı bir veritabanı tablosu, başlangıç yapılandırmasını içine okur.</span><span class="sxs-lookup"><span data-stu-id="bb993-637">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="bb993-638">Sağlayıcı anahtarı başına temelinde veritabanını sorgulamak değil.</span><span class="sxs-lookup"><span data-stu-id="bb993-638">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="bb993-639">Uygulama başlatılır sahip olduktan sonra uygulamanın yapılandırma üzerinde hiçbir etkisi kadar veritabanını güncelleme yeniden üzerinde değişiklik uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="bb993-639">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="bb993-640">Tanımlayan bir `EFConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık.</span><span class="sxs-lookup"><span data-stu-id="bb993-640">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="bb993-641">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb993-641">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="bb993-642">Ekleme bir `EFConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="bb993-642">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="bb993-643">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb993-643">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="bb993-644">Uygulayan bir sınıf oluşturma <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="bb993-644">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="bb993-645">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb993-645">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="bb993-646">Özel yapılandırma sağlayıcısını devralarak oluşturma <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="bb993-646">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="bb993-647">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır.</span><span class="sxs-lookup"><span data-stu-id="bb993-647">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="bb993-648">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb993-648">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="bb993-649">Bir `AddEFConfiguration` genişletme yöntemi izin veren yapılandırması kaynağına ekleme bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bb993-649">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="bb993-650">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb993-650">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="bb993-651">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigurationProvider` içinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb993-651">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="bb993-652">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bb993-652">Access configuration during startup</span></span>

<span data-ttu-id="bb993-653">Ekleme `IConfiguration` içine `Startup` oluşturucuya erişim yapılandırma değerlerini `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bb993-653">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bb993-654">Erişim yapılandırmaya `Startup.Configure`, ya da ekleme `IConfiguration` doğrudan yöntem veya oluşturucu örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="bb993-654">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="bb993-655">Başlangıç kullanışlı yöntemler kullanarak yapılandırma erişme ilişkin bir örnek için bkz [uygulama başlatma: Yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="bb993-655">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="bb993-656">Erişim yapılandırmasında bir Razor sayfaları sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="bb993-656">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="bb993-657">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](xref:Microsoft.Extensions.Configuration) ve ekleme <xref:Microsoft.Extensions.Configuration.IConfiguration> sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="bb993-657">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="bb993-658">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="bb993-658">In a Razor Pages page:</span></span>

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

<span data-ttu-id="bb993-659">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="bb993-659">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="bb993-660">Dış bütünleştirilmiş koddan Yapılandırması Ekle</span><span class="sxs-lookup"><span data-stu-id="bb993-660">Add configuration from an external assembly</span></span>

<span data-ttu-id="bb993-661">Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bb993-661">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="bb993-662">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="bb993-662">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb993-663">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bb993-663">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="bb993-664">Microsoft yapılandırma hakkında ayrıntılı bir inceleme</span><span class="sxs-lookup"><span data-stu-id="bb993-664">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
