---
title: ASP.NET core'da yapılandırma
author: guardrex
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 3351ab743ce38b78b1c5857e52020fdeda12cbe7
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855825"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="a2b76-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a2b76-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="a2b76-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a2b76-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a2b76-105">ASP.NET core'da uygulama yapılandırması tarafından kurulan anahtar-değer çiftleri temel *yapılandırma sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="a2b76-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="a2b76-106">Yapılandırma sağlayıcıları, yapılandırma kaynaklarını çeşitli anahtar-değer çiftlerine yapılandırma verilerini okuyun:</span><span class="sxs-lookup"><span data-stu-id="a2b76-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="a2b76-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a2b76-107">Azure Key Vault</span></span>
* <span data-ttu-id="a2b76-108">Azure uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a2b76-108">Azure App Configuration</span></span>
* <span data-ttu-id="a2b76-109">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-109">Command-line arguments</span></span>
* <span data-ttu-id="a2b76-110">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="a2b76-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="a2b76-111">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="a2b76-111">Directory files</span></span>
* <span data-ttu-id="a2b76-112">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-112">Environment variables</span></span>
* <span data-ttu-id="a2b76-113">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-113">In-memory .NET objects</span></span>
* <span data-ttu-id="a2b76-114">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="a2b76-114">Settings files</span></span>

<span data-ttu-id="a2b76-115">Yapılandırma paketlerini ortak yapılandırma sağlayıcısı senaryoları dahil edilecek [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-115">Configuration packages for common configuration provider scenarios are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="a2b76-116">Kodu izleyen ve örnek uygulama kullanma örnekleri <xref:Microsoft.Extensions.Configuration> ad alanı:</span><span class="sxs-lookup"><span data-stu-id="a2b76-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="a2b76-117">*Seçenekleri deseni* bu konuda açıklanan yapılandırma kavramları bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="a2b76-118">Seçenekler, ilgili ayar gruplarını temsil etmek için sınıflar kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-118">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a2b76-119">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-119">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="a2b76-120">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2b76-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="a2b76-121">Uygulama yapılandırması barındırın</span><span class="sxs-lookup"><span data-stu-id="a2b76-121">Host versus app configuration</span></span>

<span data-ttu-id="a2b76-122">Uygulama yapılandırılmış ve başlatıldı, önce bir *konak* başlatılan ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="a2b76-123">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a2b76-124">Bu konuda açıklanan yapılandırma sağlayıcıları kullanarak, hem uygulama hem de konak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="a2b76-125">Ana bilgisayar yapılandırma anahtar-değer çiftleri uygulamanın genel yapılandırmasının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-125">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="a2b76-126">Yapılandırma sağlayıcıları konak oluşturulduğunda kullanılan yapılandırma ve yapılandırma kaynaklarını nasıl etkileyeceğini nasıl barındırmak daha fazla bilgi için bkz: [konak](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="a2b76-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="a2b76-127">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a2b76-127">Default configuration</span></span>

<span data-ttu-id="a2b76-128">Web uygulamaları üzerinde ASP.NET Core tabanlı [yeni dotnet](/dotnet/core/tools/dotnet-new) şablonları çağrı <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="a2b76-128">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="a2b76-129"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aşağıdaki sırayla uygulaması için varsayılan yapılandırması sağlar:</span><span class="sxs-lookup"><span data-stu-id="a2b76-129"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="a2b76-130">Ana bilgisayar yapılandırma öğesinden sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-130">Host configuration is provided from:</span></span>
  * <span data-ttu-id="a2b76-131">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a2b76-131">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="a2b76-132">Önek (`ASPNETCORE_`) yapılandırma anahtar-değer çiftleri yüklendiğinde çıkartılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-132">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="a2b76-133">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a2b76-133">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="a2b76-134">Uygulama yapılandırma öğesinden sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-134">App configuration is provided from:</span></span>
  * <span data-ttu-id="a2b76-135">*appSettings.JSON* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a2b76-135">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a2b76-136">*appSettings. {Ortamı} .json* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a2b76-136">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a2b76-137">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-137">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a2b76-138">Ortam değişkenlerini kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a2b76-138">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="a2b76-139">Özel bir önek kullanılıyorsa (örneğin, `PREFIX_` ile `.AddEnvironmentVariables(prefix: "PREFIX_")`), yapılandırma anahtar-değer çiftleri yüklendiğinde önek yapılandırıldıktan.</span><span class="sxs-lookup"><span data-stu-id="a2b76-139">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="a2b76-140">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a2b76-140">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="a2b76-141">Yapılandırma sağlayıcıları, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-141">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="a2b76-142">Konak hakkında daha fazla bilgi ve <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, bkz: <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-142">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="a2b76-143">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="a2b76-143">Security</span></span>

<span data-ttu-id="a2b76-144">Aşağıdaki en iyi benimseme:</span><span class="sxs-lookup"><span data-stu-id="a2b76-144">Adopt the following best practices:</span></span>

* <span data-ttu-id="a2b76-145">Hiçbir zaman parolaları ve diğer hassas verileri yapılandırma sağlayıcısı kodda veya düz metin yapılandırma dosyalarında depolayın.</span><span class="sxs-lookup"><span data-stu-id="a2b76-145">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="a2b76-146">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a2b76-146">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="a2b76-147">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="a2b76-147">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="a2b76-148">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [gizli dizi Yöneticisi ile geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) (depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. hassas verileri).</span><span class="sxs-lookup"><span data-stu-id="a2b76-148">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="a2b76-149">Gizli dizi Yöneticisi'ni dosya yapılandırma sağlayıcısı bir JSON dosyası yerel sistemdeki kullanıcı gizli dizileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-149">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="a2b76-150">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-150">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="a2b76-151">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) uygulama gizli anahtarlarının güvenli bir şekilde depolanması için bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="a2b76-151">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="a2b76-152">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-152">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="a2b76-153">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="a2b76-153">Hierarchical configuration data</span></span>

<span data-ttu-id="a2b76-154">Yapılandırma API configuration anahtarlarında bir sınırlayıcı kullanarak hiyerarşik veri düzleştirme tarafından hiyerarşik yapılandırma verileri koruma özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="a2b76-154">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="a2b76-155">Aşağıdaki JSON dosyasında iki bölüm yapılandırılmış bir hiyerarşide dört anahtarları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="a2b76-155">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="a2b76-156">Dosya yapılandırma okuduğunuzda benzersiz anahtarlar özgün hiyerarşik veri yapısını yapılandırma kaynağı korumak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-156">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="a2b76-157">Bölümler ve anahtarlar ile bir iki nokta üst üste kullanımını düzleştirilir (`:`) özgün yapıyı korumak için:</span><span class="sxs-lookup"><span data-stu-id="a2b76-157">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="a2b76-158">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-158">section0:key0</span></span>
* <span data-ttu-id="a2b76-159">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-159">section0:key1</span></span>
* <span data-ttu-id="a2b76-160">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-160">section1:key0</span></span>
* <span data-ttu-id="a2b76-161">section1:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-161">section1:key1</span></span>

<span data-ttu-id="a2b76-162"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemlerdir bölümler ve yapılandırma verilerini bir bölümde alt yalıtmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-162"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="a2b76-163">Bu yöntem daha sonra açıklanmıştır [GetSection GetChildren ve Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="a2b76-163">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="a2b76-164">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-164">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="a2b76-165">Kurallar</span><span class="sxs-lookup"><span data-stu-id="a2b76-165">Conventions</span></span>

<span data-ttu-id="a2b76-166">Uygulama başlangıcında, yapılandırma kaynaklarını yapılandırma sağlayıcıları belirttiğiniz sırayla okunur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-166">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="a2b76-167">Değişiklik algılama uygulayan yapılandırma sağlayıcıları temel alınan bir ayar değiştiğinde yapılandırmayı yeniden yükle seçeneğine sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2b76-167">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="a2b76-168">Örneğin, dosya yapılandırma (Bu konunun ilerleyen bölümlerinde açıklanmıştır) sağlayıcısı ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılama uygulayın.</span><span class="sxs-lookup"><span data-stu-id="a2b76-168">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="a2b76-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> uygulamanın kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a2b76-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> bir Razor sayfaları yerleştirilebilir <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı yapılandırmasını almak için:</span><span class="sxs-lookup"><span data-stu-id="a2b76-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="a2b76-171">Bunlar ana bilgisayar tarafından kurduktan olduğunda değil olarak yapılandırma sağlayıcıları DI, faydalanamaz.</span><span class="sxs-lookup"><span data-stu-id="a2b76-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="a2b76-172">Yapılandırma anahtarları, aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="a2b76-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="a2b76-173">Anahtarlar büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-173">Keys are case-insensitive.</span></span> <span data-ttu-id="a2b76-174">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="a2b76-175">Aynı veya farklı yapılandırma sağlayıcıları tarafından aynı anahtar için bir değer ayarlarsanız, anahtarda ayarlanan son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="a2b76-176">Hiyerarşik anahtarları</span><span class="sxs-lookup"><span data-stu-id="a2b76-176">Hierarchical keys</span></span>
  * <span data-ttu-id="a2b76-177">Yapılandırma API'sinin, iki nokta üst üste ayırıcı (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="a2b76-178">Ortam değişkenleri, iki nokta üst üste ayırıcı tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="a2b76-179">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve bir iki nokta üst üste dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="a2b76-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="a2b76-180">Azure anahtar Kasası'nda hiyerarşik tuşlarını `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="a2b76-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="a2b76-181">Gizli dizileri uygulama yapılandırma yüklendiğinde tireler iki nokta üst üste ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="a2b76-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="a2b76-183">Dizi bağlama açıklanan [bir dizi bir sınıfa Bağla](#bind-an-array-to-a-class) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a2b76-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="a2b76-184">Yapılandırma değerleri aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="a2b76-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="a2b76-185">Dizeleri değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-185">Values are strings.</span></span>
* <span data-ttu-id="a2b76-186">Null değerler yapılandırmasında depolanmış veya nesnelere bağlı olamaz.</span><span class="sxs-lookup"><span data-stu-id="a2b76-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="a2b76-187">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="a2b76-187">Providers</span></span>

<span data-ttu-id="a2b76-188">Aşağıdaki tabloda, ASP.NET Core uygulamaları için kullanılabilir yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="a2b76-189">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a2b76-189">Provider</span></span> | <span data-ttu-id="a2b76-190">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="a2b76-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="a2b76-191">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="a2b76-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="a2b76-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a2b76-192">Azure Key Vault</span></span> |
| <span data-ttu-id="a2b76-193">[Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri)</span><span class="sxs-lookup"><span data-stu-id="a2b76-193">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="a2b76-194">Azure uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a2b76-194">Azure App Configuration</span></span> |
| [<span data-ttu-id="a2b76-195">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-195">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="a2b76-196">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-196">Command-line parameters</span></span> |
| [<span data-ttu-id="a2b76-197">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-197">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="a2b76-198">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="a2b76-198">Custom source</span></span> |
| [<span data-ttu-id="a2b76-199">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-199">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="a2b76-200">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-200">Environment variables</span></span> |
| [<span data-ttu-id="a2b76-201">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-201">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="a2b76-202">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="a2b76-202">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="a2b76-203">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-203">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="a2b76-204">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="a2b76-204">Directory files</span></span> |
| [<span data-ttu-id="a2b76-205">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-205">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="a2b76-206">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="a2b76-206">In-memory collections</span></span> |
| <span data-ttu-id="a2b76-207">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="a2b76-207">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="a2b76-208">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="a2b76-208">File in the user profile directory</span></span> |

<span data-ttu-id="a2b76-209">Yapılandırma sağlayıcıları başlatma sırasında belirttiğiniz sırayla yapılandırma kaynaklarını okunur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-209">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="a2b76-210">Bu konuda açıklanan yapılandırma sağlayıcıları açıklanan alfabetik sırada, bunları kodunuzu düzenleme sırasını değil.</span><span class="sxs-lookup"><span data-stu-id="a2b76-210">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="a2b76-211">Temel yapılandırma kaynakları için önceliklerinizden uyacak şekilde yapılandırma sağlayıcıları kodunuzu sırası.</span><span class="sxs-lookup"><span data-stu-id="a2b76-211">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="a2b76-212">Yapılandırma sağlayıcıları, tipik bir dizisidir:</span><span class="sxs-lookup"><span data-stu-id="a2b76-212">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="a2b76-213">Dosyaları (*appsettings.json*, *appsettings. { Ortam} .json*burada `{Environment}` uygulamanın geçerli barındırma ortamı)</span><span class="sxs-lookup"><span data-stu-id="a2b76-213">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="a2b76-214">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="a2b76-214">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="a2b76-215">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki yalnızca)</span><span class="sxs-lookup"><span data-stu-id="a2b76-215">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="a2b76-216">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-216">Environment variables</span></span>
1. <span data-ttu-id="a2b76-217">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-217">Command-line arguments</span></span>

<span data-ttu-id="a2b76-218">Komut satırı yapılandırma sağlayıcısı yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılmak komut satırı bağımsız değişkenlerine izin vermek için sağlayıcıları serisinin son konumlandırmak için yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-218">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="a2b76-219">Yeni bir başlattığınızda bu sağlayıcıları dizi yerine konur <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-219">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="a2b76-220">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="a2b76-220">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="a2b76-221">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="a2b76-221">ConfigureAppConfiguration</span></span>

<span data-ttu-id="a2b76-222">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> zaman oluşturmaya ek olarak, uygulamanın yapılandırma sağlayıcıları belirtmek için bir konak eklediğiniz tarafından otomatik olarak <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="a2b76-222">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

<span data-ttu-id="a2b76-223">Uygulama için sağlanan yapılandırma <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın başlatma sırasında kullanılabilir dahil olmak üzere `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-223">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a2b76-224">Daha fazla bilgi için [erişim yapılandırması başlatılırken](#access-configuration-during-startup) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a2b76-224">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="a2b76-225">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-225">Command-line Configuration Provider</span></span>

<span data-ttu-id="a2b76-226"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Yapılandırma komut satırı bağımsız değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-226">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="a2b76-227">Komut satırı yapılandırmasını etkinleştirmek için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> genişletme yöntemi bir örneğinde çağrıldığında <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-227">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a2b76-228">`AddCommandLine` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-228">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="a2b76-229">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="a2b76-229">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="a2b76-230">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="a2b76-230">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a2b76-231">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="a2b76-231">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="a2b76-232">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="a2b76-232">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="a2b76-233">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a2b76-233">Environment variables.</span></span>

<span data-ttu-id="a2b76-234">`CreateDefaultBuilder` Son komut satırı yapılandırma sağlayıcısı ekler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-234">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="a2b76-235">Çalışma zamanında geçirilen komut satırı bağımsız değişkenleri yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="a2b76-235">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="a2b76-236">`CreateDefaultBuilder` konak oluşturulduğunda işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="a2b76-236">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="a2b76-237">Bu nedenle, komut satırı yapılandırma etkinleştirildi tarafından `CreateDefaultBuilder` konak nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-237">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="a2b76-238">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="a2b76-238">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="a2b76-239">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-239">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a2b76-240">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="a2b76-240">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="a2b76-241">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="a2b76-241">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="a2b76-242">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="a2b76-242">**Example**</span></span>

<span data-ttu-id="a2b76-243">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-243">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="a2b76-244">Proje dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="a2b76-244">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="a2b76-245">Bir komut satırı bağımsız değişken `dotnet run` komutu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-245">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="a2b76-246">Uygulama çalıştıktan sonra bir uygulamaya tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-246">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a2b76-247">Çıkış için sağlanan yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çifti içeren gözlemleyin `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-247">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="a2b76-248">Arguments</span><span class="sxs-lookup"><span data-stu-id="a2b76-248">Arguments</span></span>

<span data-ttu-id="a2b76-249">Değeri bir eşittir işareti gelmelidir (`=`), veya bir önek anahtarı olmalıdır (`--` veya `/`) zaman değeri bir boşluk izler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-249">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="a2b76-250">Değer bir eşittir işareti kullanılıyorsa, null olabilir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="a2b76-250">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="a2b76-251">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="a2b76-251">Key prefix</span></span>               | <span data-ttu-id="a2b76-252">Örnek</span><span class="sxs-lookup"><span data-stu-id="a2b76-252">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="a2b76-253">Önek yok</span><span class="sxs-lookup"><span data-stu-id="a2b76-253">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="a2b76-254">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="a2b76-254">Two dashes (`--`)</span></span>        | <span data-ttu-id="a2b76-255">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="a2b76-255">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="a2b76-256">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="a2b76-256">Forward slash (`/`)</span></span>      | <span data-ttu-id="a2b76-257">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="a2b76-257">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="a2b76-258">Aynı komut içinde komut satırı bağımsız değişkeni bir eşittir işareti ile bir alanı kullanan anahtar-değer çiftleri kullanan anahtar-değer çiftleri karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="a2b76-258">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="a2b76-259">Örnek komutları:</span><span class="sxs-lookup"><span data-stu-id="a2b76-259">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="a2b76-260">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="a2b76-260">Switch mappings</span></span>

<span data-ttu-id="a2b76-261">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-261">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="a2b76-262">El ile yapı kurarken yapılandırmayla bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, anahtar değişiklik için bir sözlük sağlayabilir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a2b76-262">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="a2b76-263">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-263">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="a2b76-264">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) anahtar-değer çifti uygulamanın yapılandırmasını döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-264">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="a2b76-265">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="a2b76-265">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="a2b76-266">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="a2b76-266">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="a2b76-267">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="a2b76-267">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="a2b76-268">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-268">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="a2b76-269">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="a2b76-269">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="a2b76-270">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-270">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="a2b76-271">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-271">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a2b76-272">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-272">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="a2b76-273">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a2b76-273">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="a2b76-274">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-274">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="a2b76-275">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a2b76-275">Key</span></span>       | <span data-ttu-id="a2b76-276">Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-276">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="a2b76-277">Anahtar eşlemeli anahtarları uygulamayı başlatırken kullandıysanız, yapılandırma sözlüğü tarafından sağlanan anahtardaki yapılandırma değeri alır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-277">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="a2b76-278">Önceki komutu çalıştırdıktan sonra yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-278">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="a2b76-279">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a2b76-279">Key</span></span>               | <span data-ttu-id="a2b76-280">Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-280">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="a2b76-281">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-281">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="a2b76-282"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Yapılandırma ortam değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-282">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="a2b76-283">Ortam değişkenlerini yapılandırma etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-283">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="a2b76-284">[Azure App Service](https://azure.microsoft.com/services/app-service/) ortam değişkenlerini yapılandırma Sağlayıcısı'nı kullanarak uygulama yapılandırması geçersiz kılabilirsiniz Azure portalında ortam değişkenlerini ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-284">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="a2b76-285">Daha fazla bilgi için [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="a2b76-285">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="a2b76-286">`AddEnvironmentVariables` ortam değişkenlerini ön ekine sahip yüklemek için kullanılan `ASPNETCORE_` için [ana bilgisayar yapılandırması](#host-versus-app-configuration) yeni bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-286">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is initialized.</span></span> <span data-ttu-id="a2b76-287">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="a2b76-287">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="a2b76-288">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="a2b76-288">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a2b76-289">Uygulama yapılandırması çağırarak unprefixed ortam değişkenlerinden `AddEnvironmentVariables` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="a2b76-289">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="a2b76-290">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="a2b76-290">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="a2b76-291">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="a2b76-291">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="a2b76-292">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a2b76-292">Command-line arguments.</span></span>

<span data-ttu-id="a2b76-293">Yapılandırma kullanıcı parolalarının kurulduktan sonra ortam değişkenlerini yapılandırma sağlayıcısı denir ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="a2b76-293">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="a2b76-294">Bu konumda sağlayıcıya çağrı ortam değişkenlerini okuma yapılandırması tarafından kullanıcı parolalarını ayarlanmış geçersiz kılmak için çalışma zamanında sağlar ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="a2b76-294">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="a2b76-295">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="a2b76-295">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="a2b76-296">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="a2b76-296">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="a2b76-297">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="a2b76-297">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="a2b76-298">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="a2b76-298">**Example**</span></span>

<span data-ttu-id="a2b76-299">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-299">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="a2b76-300">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2b76-300">Run the sample app.</span></span> <span data-ttu-id="a2b76-301">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-301">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a2b76-302">Çıkış ortam değişkeni için anahtar-değer çifti içeren gözlemleyin `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-302">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="a2b76-303">Değeri, uygulamanın çalıştığı, genellikle ortamın yansıtır `Development` yerel olarak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="a2b76-303">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="a2b76-304">Ortam değişkenlerini kısa uygulama tarafından işlenen listesini tutmak için aşağıdaki ile başlayan bu ortam değişkenleri uygulama filtreleri:</span><span class="sxs-lookup"><span data-stu-id="a2b76-304">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="a2b76-305">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="a2b76-305">ASPNETCORE_</span></span>
* <span data-ttu-id="a2b76-306">URL'leri</span><span class="sxs-lookup"><span data-stu-id="a2b76-306">urls</span></span>
* <span data-ttu-id="a2b76-307">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="a2b76-307">Logging</span></span>
* <span data-ttu-id="a2b76-308">ORTAM</span><span class="sxs-lookup"><span data-stu-id="a2b76-308">ENVIRONMENT</span></span>
* <span data-ttu-id="a2b76-309">contentRoot</span><span class="sxs-lookup"><span data-stu-id="a2b76-309">contentRoot</span></span>
* <span data-ttu-id="a2b76-310">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="a2b76-310">AllowedHosts</span></span>
* <span data-ttu-id="a2b76-311">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="a2b76-311">applicationName</span></span>
* <span data-ttu-id="a2b76-312">komut satırı</span><span class="sxs-lookup"><span data-stu-id="a2b76-312">CommandLine</span></span>

<span data-ttu-id="a2b76-313">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Pages/Index.cshtml.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="a2b76-313">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="a2b76-314">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="a2b76-314">Prefixes</span></span>

<span data-ttu-id="a2b76-315">Uygulamanın yapılandırma yüklendi ortam değişkenleri, bir ön ek sağladığında filtrelenir `AddEnvironmentVariables` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a2b76-315">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="a2b76-316">Örneğin, ortam değişkenlerini önek filtresi için `CUSTOM_`, yapılandırma sağlayıcısı için önek sağlayın:</span><span class="sxs-lookup"><span data-stu-id="a2b76-316">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="a2b76-317">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek yapılandırıldıktan devre dışı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-317">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="a2b76-318">Statik kolaylık yöntemi `CreateDefaultBuilder` oluşturur bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> uygulamanın ana bilgisayar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="a2b76-318">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="a2b76-319">Zaman `WebHostBuilder` olan oluşturulan, kendi ana bilgisayar yapılandırması ön ekine sahip ortam değişkenleri içindeki bulduğu `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-319">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="a2b76-320">**Bağlantı dizesi ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="a2b76-320">**Connection string prefixes**</span></span>

<span data-ttu-id="a2b76-321">Yapılandırma API'si, dört bağlantı dizesi ortam değişkenleri için app ortamı için Azure bağlantı dizelerini yapılandırılmasıyla ilgili özel işleme kurallarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-321">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="a2b76-322">Önek yok belirtilirse tabloda gösterilen ön ekine sahip ortam değişkenleri uygulamaya yüklenir `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-322">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="a2b76-323">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="a2b76-323">Connection string prefix</span></span> | <span data-ttu-id="a2b76-324">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a2b76-324">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="a2b76-325">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a2b76-325">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="a2b76-326">MySQL</span><span class="sxs-lookup"><span data-stu-id="a2b76-326">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="a2b76-327">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="a2b76-327">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="a2b76-328">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a2b76-328">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="a2b76-329">Ne zaman bir ortam değişkeni bulunur ve herhangi bir tabloda gösterilen dört öneklerini yapılandırmasını yüklendi:</span><span class="sxs-lookup"><span data-stu-id="a2b76-329">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="a2b76-330">Yapılandırma anahtarı ortam değişkeni ön eki kaldırma ve yapılandırma anahtar bölümünü ekleyerek oluşturulur (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="a2b76-330">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="a2b76-331">Veritabanı bağlantı sağlayıcısı temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (dışında `CUSTOMCONNSTR_`, belirtilen sağlayıcı yok sahiptir).</span><span class="sxs-lookup"><span data-stu-id="a2b76-331">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="a2b76-332">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="a2b76-332">Environment variable key</span></span> | <span data-ttu-id="a2b76-333">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="a2b76-333">Converted configuration key</span></span> | <span data-ttu-id="a2b76-334">Sağlayıcı Yapılandırması girdisi</span><span class="sxs-lookup"><span data-stu-id="a2b76-334">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a2b76-335">Yapılandırma girişi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-335">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a2b76-336">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a2b76-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a2b76-337">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="a2b76-337">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a2b76-338">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a2b76-338">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a2b76-339">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a2b76-339">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a2b76-340">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a2b76-340">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a2b76-341">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a2b76-341">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="a2b76-342">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-342">File Configuration Provider</span></span>

<span data-ttu-id="a2b76-343"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> yapılandırma dosya sisteminden yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-343"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="a2b76-344">Aşağıdaki yapılandırma sağlayıcıları için belirli dosya türleri ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-344">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="a2b76-345">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-345">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="a2b76-346">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-346">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="a2b76-347">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-347">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="a2b76-348">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-348">INI Configuration Provider</span></span>

<span data-ttu-id="a2b76-349"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Yapılandırma ını dosyası anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-349">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="a2b76-350">INI dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-350">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a2b76-351">İki nokta üst üste INI dosya yapılandırması bölüm ayırıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-351">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="a2b76-352">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="a2b76-352">Overloads permit specifying:</span></span>

* <span data-ttu-id="a2b76-353">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-353">Whether the file is optional.</span></span>
* <span data-ttu-id="a2b76-354">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="a2b76-354">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a2b76-355"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-355">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a2b76-356">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="a2b76-356">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="a2b76-357">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-357">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="a2b76-358">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-358">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-359">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="a2b76-359">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="a2b76-360">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-360">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="a2b76-361">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-361">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-362">Genel bir INI yapılandırma dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="a2b76-362">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="a2b76-363">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="a2b76-363">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a2b76-364">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-364">section0:key0</span></span>
* <span data-ttu-id="a2b76-365">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-365">section0:key1</span></span>
* <span data-ttu-id="a2b76-366">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="a2b76-366">section1:subsection:key</span></span>
* <span data-ttu-id="a2b76-367">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="a2b76-367">section2:subsection0:key</span></span>
* <span data-ttu-id="a2b76-368">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="a2b76-368">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="a2b76-369">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-369">JSON Configuration Provider</span></span>

<span data-ttu-id="a2b76-370"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Yapılandırma JSON dosyası anahtar-değer çiftlerinden çalışma zamanı sırasında yükler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-370">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="a2b76-371">JSON dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-371">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a2b76-372">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="a2b76-372">Overloads permit specifying:</span></span>

* <span data-ttu-id="a2b76-373">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-373">Whether the file is optional.</span></span>
* <span data-ttu-id="a2b76-374">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="a2b76-374">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a2b76-375"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-375">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a2b76-376">`AddJsonFile` Yeni bir başlattığınızda iki kez otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-376">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="a2b76-377">Yöntem yapılandırmasından yüklenemedi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-377">The method is called to load configuration from:</span></span>

* <span data-ttu-id="a2b76-378">*appSettings.JSON* &ndash; bu dosyayı ilk okuyun.</span><span class="sxs-lookup"><span data-stu-id="a2b76-378">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="a2b76-379">Dosyanın ortam sürümü tarafından sağlanan değerleri geçersiz kılabilir *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2b76-379">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="a2b76-380">*appSettings. {Ortamı} .json* &ndash; dosyanın ortam sürümünü temel alınarak yüklenir [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="a2b76-380">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="a2b76-381">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="a2b76-381">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="a2b76-382">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="a2b76-382">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a2b76-383">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a2b76-383">Environment variables.</span></span>
* <span data-ttu-id="a2b76-384">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="a2b76-384">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="a2b76-385">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a2b76-385">Command-line arguments.</span></span>

<span data-ttu-id="a2b76-386">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-386">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="a2b76-387">Bu nedenle, yapılandırma kümesi kullanıcı parolaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri geçersiz kılma *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="a2b76-387">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="a2b76-388">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırma dosyaları için dışında belirtmek için konak oluştururken *appsettings.json* ve *appsettings. { Ortam} .json*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-388">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="a2b76-389">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-389">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="a2b76-390">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-390">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-391">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="a2b76-391">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="a2b76-392">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-392">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="a2b76-393">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-393">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-394">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="a2b76-394">**Example**</span></span>

<span data-ttu-id="a2b76-395">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` iki çağrıları içeren ana bilgisayar oluşturmak için `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-395">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="a2b76-396">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="a2b76-396">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="a2b76-397">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2b76-397">Run the sample app.</span></span> <span data-ttu-id="a2b76-398">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-398">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a2b76-399">Çıkış ortamına bağlı olarak tabloda gösterilen yapılandırması için anahtar-değer çiftleri içeren gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a2b76-399">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="a2b76-400">Günlük kaydı yapılandırması tuşlarını iki nokta üst üste (`:`) hiyerarşik ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="a2b76-400">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="a2b76-401">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a2b76-401">Key</span></span>                        | <span data-ttu-id="a2b76-402">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="a2b76-402">Development Value</span></span> | <span data-ttu-id="a2b76-403">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="a2b76-403">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="a2b76-404">Günlüğe kaydetme: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="a2b76-404">Logging:LogLevel:System</span></span>    | <span data-ttu-id="a2b76-405">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a2b76-405">Information</span></span>       | <span data-ttu-id="a2b76-406">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a2b76-406">Information</span></span>      |
| <span data-ttu-id="a2b76-407">Günlüğe kaydetme: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="a2b76-407">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="a2b76-408">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a2b76-408">Information</span></span>       | <span data-ttu-id="a2b76-409">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a2b76-409">Information</span></span>      |
| <span data-ttu-id="a2b76-410">Günlüğe kaydetme: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="a2b76-410">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="a2b76-411">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a2b76-411">Debug</span></span>             | <span data-ttu-id="a2b76-412">Hata</span><span class="sxs-lookup"><span data-stu-id="a2b76-412">Error</span></span>            |
| <span data-ttu-id="a2b76-413">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="a2b76-413">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="a2b76-414">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-414">XML Configuration Provider</span></span>

<span data-ttu-id="a2b76-415"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Yapılandırma XML dosyası anahtar-değer çiftlerinin zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-415">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="a2b76-416">XML dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-416">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a2b76-417">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="a2b76-417">Overloads permit specifying:</span></span>

* <span data-ttu-id="a2b76-418">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-418">Whether the file is optional.</span></span>
* <span data-ttu-id="a2b76-419">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="a2b76-419">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a2b76-420"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-420">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a2b76-421">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayıldı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-421">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="a2b76-422">Bir belge türü tanımı (DTD'nin) veya ad alanı dosyasında belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="a2b76-422">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="a2b76-423">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="a2b76-423">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="a2b76-424">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-424">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="a2b76-425">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-425">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-426">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="a2b76-426">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="a2b76-427">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-427">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="a2b76-428">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-428">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-429">XML yapılandırma dosyalarını, yinelenen bölümler için ayrı bir öğe adları kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a2b76-429">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="a2b76-430">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="a2b76-430">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a2b76-431">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-431">section0:key0</span></span>
* <span data-ttu-id="a2b76-432">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-432">section0:key1</span></span>
* <span data-ttu-id="a2b76-433">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-433">section1:key0</span></span>
* <span data-ttu-id="a2b76-434">section1:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-434">section1:key1</span></span>

<span data-ttu-id="a2b76-435">İş öğesi adının aynısını kullanın öğeleri, yinelenen `name` özniteliği öğeleri ayırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-435">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="a2b76-436">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="a2b76-436">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a2b76-437">Bölüm: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-437">section:section0:key:key0</span></span>
* <span data-ttu-id="a2b76-438">Bölüm: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-438">section:section0:key:key1</span></span>
* <span data-ttu-id="a2b76-439">Bölüm: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-439">section:section1:key:key0</span></span>
* <span data-ttu-id="a2b76-440">Bölüm: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-440">section:section1:key:key1</span></span>

<span data-ttu-id="a2b76-441">Öznitelik değerlerini sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a2b76-441">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="a2b76-442">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="a2b76-442">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a2b76-443">Anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="a2b76-443">key:attribute</span></span>
* <span data-ttu-id="a2b76-444">Bölüm: anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="a2b76-444">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="a2b76-445">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-445">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="a2b76-446"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bir dizin dosyalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-446">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="a2b76-447">Anahtar dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-447">The key is the file name.</span></span> <span data-ttu-id="a2b76-448">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-448">The value contains the file's contents.</span></span> <span data-ttu-id="a2b76-449">Dosya başına anahtar yapılandırma sağlayıcısı Docker'da barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-449">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="a2b76-450">Dosya başına anahtar yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-450">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="a2b76-451">`directoryPath` Dosyalar için mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-451">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="a2b76-452">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="a2b76-452">Overloads permit specifying:</span></span>

* <span data-ttu-id="a2b76-453">Bir `Action<KeyPerFileConfigurationSource>` kaynağını yapılandırır temsilci.</span><span class="sxs-lookup"><span data-stu-id="a2b76-453">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="a2b76-454">Dizin isteğe bağlı olup olmadığını ve dizinin yolu.</span><span class="sxs-lookup"><span data-stu-id="a2b76-454">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="a2b76-455">Çift alt çizgi (`__`) dosya adları içinde yapılandırma anahtar sınırlayıcı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-455">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="a2b76-456">Örneğin, dosya adı `Logging__LogLevel__System` üretir yapılandırma anahtarı `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-456">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="a2b76-457">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="a2b76-457">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="a2b76-458">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-458">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="a2b76-459">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-459">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-460">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="a2b76-460">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="a2b76-461">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-461">Memory Configuration Provider</span></span>

<span data-ttu-id="a2b76-462"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Bir bellek içi koleksiyonu yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-462">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="a2b76-463">Bellek içi toplama yapılandırması etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-463">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a2b76-464">Yapılandırma sağlayıcısı ile başlatılabilir bir `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-464">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="a2b76-465">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="a2b76-465">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="a2b76-466">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="a2b76-466">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="a2b76-467">GetValue</span><span class="sxs-lookup"><span data-stu-id="a2b76-467">GetValue</span></span>

<span data-ttu-id="a2b76-468">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) bir değeri belirtilen bir anahtarla yapılandırmasından ayıklar ve onu belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a2b76-468">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="a2b76-469">Aşırı yükleme anahtarı bulunmazsa, varsayılan bir değer sağlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-469">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="a2b76-470">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="a2b76-470">The following example:</span></span>

* <span data-ttu-id="a2b76-471">Dize değeri yapılandırmasından anahtarıyla ayıklar `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-471">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="a2b76-472">Varsa `NumberKey` yapılandırma anahtarları, varsayılan değeri bulunamadığında `99` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-472">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="a2b76-473">Değer olarak türleri bir `int`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-473">Types the value as an `int`.</span></span>
* <span data-ttu-id="a2b76-474">İçinde bir değer depolar `NumberConfig` özellik sayfası tarafından kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="a2b76-474">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="a2b76-475">GetSection, GetChildren ve var.</span><span class="sxs-lookup"><span data-stu-id="a2b76-475">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="a2b76-476">Aşağıdaki örneklerde için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="a2b76-476">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="a2b76-477">Dört anahtarları içeren bir çift alt bölümleri içerir, iki bölümlerde bulunur:</span><span class="sxs-lookup"><span data-stu-id="a2b76-477">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="a2b76-478">Dosya yapılandırma okuduğunuzda aşağıdaki benzersiz hiyerarşik anahtarları yapılandırma değerleri tutmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a2b76-478">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="a2b76-479">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-479">section0:key0</span></span>
* <span data-ttu-id="a2b76-480">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-480">section0:key1</span></span>
* <span data-ttu-id="a2b76-481">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-481">section1:key0</span></span>
* <span data-ttu-id="a2b76-482">section1:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-482">section1:key1</span></span>
* <span data-ttu-id="a2b76-483">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-483">section2:subsection0:key0</span></span>
* <span data-ttu-id="a2b76-484">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-484">section2:subsection0:key1</span></span>
* <span data-ttu-id="a2b76-485">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="a2b76-485">section2:subsection1:key0</span></span>
* <span data-ttu-id="a2b76-486">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="a2b76-486">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="a2b76-487">GetSection</span><span class="sxs-lookup"><span data-stu-id="a2b76-487">GetSection</span></span>

<span data-ttu-id="a2b76-488">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) yapılandırma bölümüne belirtilen alt anahtarını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="a2b76-488">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="a2b76-489">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-489">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-490">Döndürülecek bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> yalnızca anahtar-değer çiftlerini içeren `section1`, çağrı `GetSection` ve bölüm adı verin:</span><span class="sxs-lookup"><span data-stu-id="a2b76-490">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="a2b76-491">`configSection` Bir değeri, yalnızca bir anahtar ve bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-491">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="a2b76-492">Benzer şekilde, anahtarları değerlerini almak için `section2:subsection0`, çağrı `GetSection` ve bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="a2b76-492">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="a2b76-493">`GetSection` hiç dönmüyor `null`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-493">`GetSection` never returns `null`.</span></span> <span data-ttu-id="a2b76-494">Eşleşen bir bölümü olmadığından bulunamazsa, boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a2b76-494">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="a2b76-495">Zaman `GetSection` eşleşen bir bölümü döndürür <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuş değil.</span><span class="sxs-lookup"><span data-stu-id="a2b76-495">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="a2b76-496">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> bölümü varsa, döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a2b76-496">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="a2b76-497">GetChildren</span><span class="sxs-lookup"><span data-stu-id="a2b76-497">GetChildren</span></span>

<span data-ttu-id="a2b76-498">Bir çağrı [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) üzerinde `section2` alır bir `IEnumerable<IConfigurationSection>` içeren:</span><span class="sxs-lookup"><span data-stu-id="a2b76-498">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="a2b76-499">Var</span><span class="sxs-lookup"><span data-stu-id="a2b76-499">Exists</span></span>

<span data-ttu-id="a2b76-500">Kullanım [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) yapılandırma bölümü olup olmadığını belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="a2b76-500">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="a2b76-501">Belirtilen örnek veri `sectionExists` olduğu `false` olmadığından bir `section2:subsection2` yapılandırma verilerini bir bölümde.</span><span class="sxs-lookup"><span data-stu-id="a2b76-501">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="a2b76-502">Bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="a2b76-502">Bind to a class</span></span>

<span data-ttu-id="a2b76-503">Yapılandırma kullanarak ilgili ayar gruplarını temsil eden sınıflar için bağlanabilir *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="a2b76-503">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="a2b76-504">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-504">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="a2b76-505">Yapılandırma değerleri döndürülür dizeler olarak çağıran <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> oluşumu sağlayan [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesneleri.</span><span class="sxs-lookup"><span data-stu-id="a2b76-505">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="a2b76-506">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-506">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-507">Örnek uygulamayı içeren bir `Starship` modeli (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2b76-507">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="a2b76-508">`starship` Bölümünü *starship.json* örnek uygulamayı yapılandırma yüklemek için JSON yapılandırma sağlayıcısı kullandığında yapılandırma dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a2b76-508">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="a2b76-509">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a2b76-509">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="a2b76-510">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a2b76-510">Key</span></span>                   | <span data-ttu-id="a2b76-511">Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-511">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="a2b76-512">starship: adı</span><span class="sxs-lookup"><span data-stu-id="a2b76-512">starship:name</span></span>         | <span data-ttu-id="a2b76-513">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="a2b76-513">USS Enterprise</span></span>                                    |
| <span data-ttu-id="a2b76-514">starship:registry</span><span class="sxs-lookup"><span data-stu-id="a2b76-514">starship:registry</span></span>     | <span data-ttu-id="a2b76-515">NCC 1701</span><span class="sxs-lookup"><span data-stu-id="a2b76-515">NCC-1701</span></span>                                          |
| <span data-ttu-id="a2b76-516">starship:class</span><span class="sxs-lookup"><span data-stu-id="a2b76-516">starship:class</span></span>        | <span data-ttu-id="a2b76-517">Anayasa</span><span class="sxs-lookup"><span data-stu-id="a2b76-517">Constitution</span></span>                                      |
| <span data-ttu-id="a2b76-518">starship:length</span><span class="sxs-lookup"><span data-stu-id="a2b76-518">starship:length</span></span>       | <span data-ttu-id="a2b76-519">304.8</span><span class="sxs-lookup"><span data-stu-id="a2b76-519">304.8</span></span>                                             |
| <span data-ttu-id="a2b76-520">starship: yetkilendirilen</span><span class="sxs-lookup"><span data-stu-id="a2b76-520">starship:commissioned</span></span> | <span data-ttu-id="a2b76-521">False</span><span class="sxs-lookup"><span data-stu-id="a2b76-521">False</span></span>                                             |
| <span data-ttu-id="a2b76-522">Ticari marka</span><span class="sxs-lookup"><span data-stu-id="a2b76-522">trademark</span></span>             | <span data-ttu-id="a2b76-523">Paramount resimleri Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="a2b76-523">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="a2b76-524">Örnek Uygulama çağrıları `GetSection` ile `starship` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-524">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="a2b76-525">`starship` Anahtar-değer çiftleridir yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="a2b76-525">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="a2b76-526">`Bind` Bir örneğini geçirerek Altbölüm yöntemi çağrıldığında `Starship` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-526">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="a2b76-527">Örnek değerleri bağlandıktan sonra işleme için bir özellik için örneği atanır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-527">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="a2b76-528">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-528">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="a2b76-529">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="a2b76-529">Bind to an object graph</span></span>

<span data-ttu-id="a2b76-530"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Tüm POCO Nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-530"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="a2b76-531">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-531">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-532">Örnek içeren bir `TvShow` olan nesne grafiğini içeren model `Metadata` ve `Actors` sınıfları (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2b76-532">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="a2b76-533">Örnek uygulamanın bir *tvshow.xml* yapılandırma verilerini içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="a2b76-533">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="a2b76-534">Yapılandırma tüm bağlı `TvShow` Nesne grafiği ile `Bind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a2b76-534">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="a2b76-535">İlişkili örneği, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-535">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="a2b76-536">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="a2b76-536">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="a2b76-537">`Get<T>` kullanmaktan daha kullanışlı olan `Bind`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-537">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="a2b76-538">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle birlikte işlemek için kullanılan özellik doğrudan atanan bağlı örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="a2b76-538">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="a2b76-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="a2b76-540">`Get<T>` ASP.NET Core 1.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-540">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="a2b76-541">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-541">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="a2b76-542">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="a2b76-542">Bind an array to a class</span></span>

<span data-ttu-id="a2b76-543">*Örnek uygulama, bu bölümde açıklanan kavramları göstermektedir.*</span><span class="sxs-lookup"><span data-stu-id="a2b76-543">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="a2b76-544"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="a2b76-544">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="a2b76-545">Sayısal bir anahtar kesimi sunan herhangi bir dizi biçimi (`:0:`, `:1:`, &hellip; `:{n}:`) dizisi bağlama POCO sınıfı dizisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-545">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="a2b76-546">' Bağlama '' bulunduğu [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-546">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="a2b76-547">Bağlama, kural olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-547">Binding is provided by convention.</span></span> <span data-ttu-id="a2b76-548">Özel yapılandırma sağlayıcıları dizi bağlama uygulamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-548">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="a2b76-549">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="a2b76-549">**In-memory array processing**</span></span>

<span data-ttu-id="a2b76-550">Yapılandırma anahtarları ve değerleri aşağıdaki tabloda gösterilen göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="a2b76-550">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="a2b76-551">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a2b76-551">Key</span></span>             | <span data-ttu-id="a2b76-552">Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-552">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="a2b76-553">dizi: girişler: 0</span><span class="sxs-lookup"><span data-stu-id="a2b76-553">array:entries:0</span></span> | <span data-ttu-id="a2b76-554">value0</span><span class="sxs-lookup"><span data-stu-id="a2b76-554">value0</span></span> |
| <span data-ttu-id="a2b76-555">dizi: girişler: 1</span><span class="sxs-lookup"><span data-stu-id="a2b76-555">array:entries:1</span></span> | <span data-ttu-id="a2b76-556">Değer1</span><span class="sxs-lookup"><span data-stu-id="a2b76-556">value1</span></span> |
| <span data-ttu-id="a2b76-557">dizi: girişler: 2</span><span class="sxs-lookup"><span data-stu-id="a2b76-557">array:entries:2</span></span> | <span data-ttu-id="a2b76-558">Value2</span><span class="sxs-lookup"><span data-stu-id="a2b76-558">value2</span></span> |
| <span data-ttu-id="a2b76-559">dizi: girişler: 4</span><span class="sxs-lookup"><span data-stu-id="a2b76-559">array:entries:4</span></span> | <span data-ttu-id="a2b76-560">Değer4</span><span class="sxs-lookup"><span data-stu-id="a2b76-560">value4</span></span> |
| <span data-ttu-id="a2b76-561">dizi: girişler: 5</span><span class="sxs-lookup"><span data-stu-id="a2b76-561">array:entries:5</span></span> | <span data-ttu-id="a2b76-562">Değeri5</span><span class="sxs-lookup"><span data-stu-id="a2b76-562">value5</span></span> |

<span data-ttu-id="a2b76-563">Bellek yapılandırma sağlayıcısı kullanan örnek uygulamasında, bu anahtarların ve değerlerin yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a2b76-563">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

<span data-ttu-id="a2b76-564">Dizi dizini için bir değer atlar &num;3.</span><span class="sxs-lookup"><span data-stu-id="a2b76-564">The array skips a value for index &num;3.</span></span> <span data-ttu-id="a2b76-565">Yapılandırma bağlayıcı temizleyin, bu dizi için bir nesne bağlamanın sonucunu gösterilmiştir birazdan haline geldikten bağlama null değerler veya null girişler bağımlı nesneleri oluşturma yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-565">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="a2b76-566">Örnek uygulamada, ilişkili yapılandırma verilerini tutmak bir POCO sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a2b76-566">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="a2b76-567">Yapılandırma verilerini nesnesine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-567">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="a2b76-568">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a2b76-568">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a2b76-569">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) söz dizimi de kullanılabilir, daha küçük kod sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="a2b76-569">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="a2b76-570">İlişkili nesne, örneği `ArrayExample`, yapılandırmasından dizisi verileri alır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-570">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="a2b76-571">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="a2b76-571">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="a2b76-572">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-572">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="a2b76-573">0</span><span class="sxs-lookup"><span data-stu-id="a2b76-573">0</span></span>                            | <span data-ttu-id="a2b76-574">value0</span><span class="sxs-lookup"><span data-stu-id="a2b76-574">value0</span></span>                       |
| <span data-ttu-id="a2b76-575">1\.</span><span class="sxs-lookup"><span data-stu-id="a2b76-575">1</span></span>                            | <span data-ttu-id="a2b76-576">Değer1</span><span class="sxs-lookup"><span data-stu-id="a2b76-576">value1</span></span>                       |
| <span data-ttu-id="a2b76-577">2</span><span class="sxs-lookup"><span data-stu-id="a2b76-577">2</span></span>                            | <span data-ttu-id="a2b76-578">Value2</span><span class="sxs-lookup"><span data-stu-id="a2b76-578">value2</span></span>                       |
| <span data-ttu-id="a2b76-579">3</span><span class="sxs-lookup"><span data-stu-id="a2b76-579">3</span></span>                            | <span data-ttu-id="a2b76-580">Değer4</span><span class="sxs-lookup"><span data-stu-id="a2b76-580">value4</span></span>                       |
| <span data-ttu-id="a2b76-581">4</span><span class="sxs-lookup"><span data-stu-id="a2b76-581">4</span></span>                            | <span data-ttu-id="a2b76-582">Değeri5</span><span class="sxs-lookup"><span data-stu-id="a2b76-582">value5</span></span>                       |

<span data-ttu-id="a2b76-583">Dizin &num;3'te ilişkili nesne için yapılandırma verilerini tutan `array:4` yapılandırma anahtarı ve değeri `value4`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-583">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="a2b76-584">Bir diziyi içeren yapılandırma verilerini bağlandığında, dizi dizinleri yapılandırma anahtarları yalnızca nesne oluşturma sırasında yapılandırma verileri yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-584">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="a2b76-585">Yapılandırma verilerinde bir null değer tutulamıyor ve yapılandırma anahtarları bir dizide bir veya daha fazla dizinlerini atladığında null değerli bir girişi bir bağımlı nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="a2b76-585">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="a2b76-586">Eksik yapılandırma öğesi için dizin &num;bağlama önce 3 sağlanabilir `ArrayExample` örneği üretir yapılandırma doğru anahtar-değer çifti herhangi bir yapılandırma sağlayıcısı tarafından.</span><span class="sxs-lookup"><span data-stu-id="a2b76-586">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="a2b76-587">Ek bir JSON yapılandırma sağlayıcısı eksik anahtar-değer çifti ile örneği varsa `ArrayExample.Entries` yapılandırmanın tamamı dizisi ile eşleşen:</span><span class="sxs-lookup"><span data-stu-id="a2b76-587">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="a2b76-588">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-588">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="a2b76-589">İçinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="a2b76-589">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="a2b76-590">Tabloda belirtilen anahtar-değer çifti yapılandırma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-590">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="a2b76-591">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a2b76-591">Key</span></span>             | <span data-ttu-id="a2b76-592">Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-592">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="a2b76-593">dizi: girişler: 3</span><span class="sxs-lookup"><span data-stu-id="a2b76-593">array:entries:3</span></span> | <span data-ttu-id="a2b76-594">Değeri3</span><span class="sxs-lookup"><span data-stu-id="a2b76-594">value3</span></span> |

<span data-ttu-id="a2b76-595">Varsa `ArrayExample` sınıf örneği bağlı dizin için giriş JSON yapılandırma sağlayıcısı içerir sonra &num;3 `ArrayExample.Entries` dizi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="a2b76-595">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="a2b76-596">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="a2b76-596">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="a2b76-597">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-597">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="a2b76-598">0</span><span class="sxs-lookup"><span data-stu-id="a2b76-598">0</span></span>                            | <span data-ttu-id="a2b76-599">value0</span><span class="sxs-lookup"><span data-stu-id="a2b76-599">value0</span></span>                       |
| <span data-ttu-id="a2b76-600">1\.</span><span class="sxs-lookup"><span data-stu-id="a2b76-600">1</span></span>                            | <span data-ttu-id="a2b76-601">Değer1</span><span class="sxs-lookup"><span data-stu-id="a2b76-601">value1</span></span>                       |
| <span data-ttu-id="a2b76-602">2</span><span class="sxs-lookup"><span data-stu-id="a2b76-602">2</span></span>                            | <span data-ttu-id="a2b76-603">Value2</span><span class="sxs-lookup"><span data-stu-id="a2b76-603">value2</span></span>                       |
| <span data-ttu-id="a2b76-604">3</span><span class="sxs-lookup"><span data-stu-id="a2b76-604">3</span></span>                            | <span data-ttu-id="a2b76-605">Değeri3</span><span class="sxs-lookup"><span data-stu-id="a2b76-605">value3</span></span>                       |
| <span data-ttu-id="a2b76-606">4</span><span class="sxs-lookup"><span data-stu-id="a2b76-606">4</span></span>                            | <span data-ttu-id="a2b76-607">Değer4</span><span class="sxs-lookup"><span data-stu-id="a2b76-607">value4</span></span>                       |
| <span data-ttu-id="a2b76-608">5</span><span class="sxs-lookup"><span data-stu-id="a2b76-608">5</span></span>                            | <span data-ttu-id="a2b76-609">Değeri5</span><span class="sxs-lookup"><span data-stu-id="a2b76-609">value5</span></span>                       |

<span data-ttu-id="a2b76-610">**JSON dizisi işleme**</span><span class="sxs-lookup"><span data-stu-id="a2b76-610">**JSON array processing**</span></span>

<span data-ttu-id="a2b76-611">Bir JSON dosyası bir dizi varsa, dizi öğeleri bölümünde sıfır tabanlı dizine sahip için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-611">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="a2b76-612">Aşağıdaki yapılandırma dosyasında `subsection` dizisi:</span><span class="sxs-lookup"><span data-stu-id="a2b76-612">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="a2b76-613">JSON yapılandırma sağlayıcısı, aşağıdaki anahtar-değer çiftlerine yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="a2b76-613">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="a2b76-614">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a2b76-614">Key</span></span>                     | <span data-ttu-id="a2b76-615">Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-615">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="a2b76-616">json_array:key</span><span class="sxs-lookup"><span data-stu-id="a2b76-616">json_array:key</span></span>          | <span data-ttu-id="a2b76-617">Değera</span><span class="sxs-lookup"><span data-stu-id="a2b76-617">valueA</span></span> |
| <span data-ttu-id="a2b76-618">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="a2b76-618">json_array:subsection:0</span></span> | <span data-ttu-id="a2b76-619">Değerb</span><span class="sxs-lookup"><span data-stu-id="a2b76-619">valueB</span></span> |
| <span data-ttu-id="a2b76-620">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="a2b76-620">json_array:subsection:1</span></span> | <span data-ttu-id="a2b76-621">valueC</span><span class="sxs-lookup"><span data-stu-id="a2b76-621">valueC</span></span> |
| <span data-ttu-id="a2b76-622">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="a2b76-622">json_array:subsection:2</span></span> | <span data-ttu-id="a2b76-623">Değerli</span><span class="sxs-lookup"><span data-stu-id="a2b76-623">valueD</span></span> |

<span data-ttu-id="a2b76-624">Örnek uygulamada, aşağıdaki POCO sınıfı yapılandırma anahtar-değer çiftleri bağlamak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a2b76-624">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="a2b76-625">Bağlama sonra `JsonArrayExample.Key` değerine `valueA`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-625">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="a2b76-626">Alt değerleri POCO dizi özelliğinde depolanır `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-626">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="a2b76-627">`JsonArrayExample.Subsection` Dizin</span><span class="sxs-lookup"><span data-stu-id="a2b76-627">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="a2b76-628">`JsonArrayExample.Subsection` Değer</span><span class="sxs-lookup"><span data-stu-id="a2b76-628">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="a2b76-629">0</span><span class="sxs-lookup"><span data-stu-id="a2b76-629">0</span></span>                                   | <span data-ttu-id="a2b76-630">Değerb</span><span class="sxs-lookup"><span data-stu-id="a2b76-630">valueB</span></span>                              |
| <span data-ttu-id="a2b76-631">1\.</span><span class="sxs-lookup"><span data-stu-id="a2b76-631">1</span></span>                                   | <span data-ttu-id="a2b76-632">valueC</span><span class="sxs-lookup"><span data-stu-id="a2b76-632">valueC</span></span>                              |
| <span data-ttu-id="a2b76-633">2</span><span class="sxs-lookup"><span data-stu-id="a2b76-633">2</span></span>                                   | <span data-ttu-id="a2b76-634">Değerli</span><span class="sxs-lookup"><span data-stu-id="a2b76-634">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="a2b76-635">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a2b76-635">Custom configuration provider</span></span>

<span data-ttu-id="a2b76-636">Örnek uygulamayı yapılandırma anahtar-değer çiftleri kullanarak bir veritabanını okuyan bir temel yapılandırma sağlayıcısı oluşturma gösterilmektedir [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="a2b76-636">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="a2b76-637">Sağlayıcı, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a2b76-637">The provider has the following characteristics:</span></span>

* <span data-ttu-id="a2b76-638">EF bellek içi veritabanına tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-638">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="a2b76-639">Bir bağlantı dizesi gerektiren bir veritabanı kullanmak için ikincil uygulama `ConfigurationBuilder` başka bir yapılandırma sağlayıcısı bağlantı dizesinden sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="a2b76-639">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="a2b76-640">Sağlayıcı bir veritabanı tablosu, başlangıç yapılandırmasını içine okur.</span><span class="sxs-lookup"><span data-stu-id="a2b76-640">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="a2b76-641">Sağlayıcı anahtarı başına temelinde veritabanını sorgulamak değil.</span><span class="sxs-lookup"><span data-stu-id="a2b76-641">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="a2b76-642">Uygulama başlatılır sahip olduktan sonra uygulamanın yapılandırma üzerinde hiçbir etkisi kadar veritabanını güncelleme yeniden üzerinde değişiklik uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-642">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="a2b76-643">Tanımlayan bir `EFConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık.</span><span class="sxs-lookup"><span data-stu-id="a2b76-643">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="a2b76-644">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-644">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="a2b76-645">Ekleme bir `EFConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="a2b76-645">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="a2b76-646">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-646">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a2b76-647">Uygulayan bir sınıf oluşturma <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-647">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="a2b76-648">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-648">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="a2b76-649">Özel yapılandırma sağlayıcısını devralarak oluşturma <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-649">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="a2b76-650">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır.</span><span class="sxs-lookup"><span data-stu-id="a2b76-650">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="a2b76-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="a2b76-652">Bir `AddEFConfiguration` genişletme yöntemi izin veren yapılandırması kaynağına ekleme bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-652">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="a2b76-653">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-653">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="a2b76-654">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigurationProvider` içinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2b76-654">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="a2b76-655">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a2b76-655">Access configuration during startup</span></span>

<span data-ttu-id="a2b76-656">Ekleme `IConfiguration` içine `Startup` oluşturucuya erişim yapılandırma değerlerini `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a2b76-656">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a2b76-657">Erişim yapılandırmaya `Startup.Configure`, ya da ekleme `IConfiguration` doğrudan yöntem veya oluşturucu örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2b76-657">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="a2b76-658">Başlangıç kullanışlı yöntemler kullanarak yapılandırma erişme ilişkin bir örnek için bkz [uygulama başlatma: Yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="a2b76-658">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="a2b76-659">Erişim yapılandırmasında bir Razor sayfaları sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="a2b76-659">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="a2b76-660">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](xref:Microsoft.Extensions.Configuration) ve ekleme <xref:Microsoft.Extensions.Configuration.IConfiguration> sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="a2b76-660">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="a2b76-661">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="a2b76-661">In a Razor Pages page:</span></span>

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

<span data-ttu-id="a2b76-662">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="a2b76-662">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="a2b76-663">Dış bütünleştirilmiş koddan Yapılandırması Ekle</span><span class="sxs-lookup"><span data-stu-id="a2b76-663">Add configuration from an external assembly</span></span>

<span data-ttu-id="a2b76-664">Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a2b76-664">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="a2b76-665">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a2b76-665">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2b76-666">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a2b76-666">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="a2b76-667">Microsoft yapılandırma hakkında ayrıntılı bir inceleme</span><span class="sxs-lookup"><span data-stu-id="a2b76-667">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
