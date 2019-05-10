---
title: ASP.NET core'da yapılandırma
author: guardrex
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 63a876c09f952537d790f2a5df4b8672df49d015
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517028"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="d1d78-103">ASP.NET core'da yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d1d78-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="d1d78-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d1d78-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d1d78-105">ASP.NET core'da uygulama yapılandırması tarafından kurulan anahtar-değer çiftleri temel *yapılandırma sağlayıcıları*.</span><span class="sxs-lookup"><span data-stu-id="d1d78-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="d1d78-106">Yapılandırma sağlayıcıları, yapılandırma kaynaklarını çeşitli anahtar-değer çiftlerine yapılandırma verilerini okuyun:</span><span class="sxs-lookup"><span data-stu-id="d1d78-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="d1d78-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d1d78-107">Azure Key Vault</span></span>
* <span data-ttu-id="d1d78-108">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-108">Command-line arguments</span></span>
* <span data-ttu-id="d1d78-109">(Yüklü veya oluşturulan) özel sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="d1d78-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="d1d78-110">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="d1d78-110">Directory files</span></span>
* <span data-ttu-id="d1d78-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-111">Environment variables</span></span>
* <span data-ttu-id="d1d78-112">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-112">In-memory .NET objects</span></span>
* <span data-ttu-id="d1d78-113">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="d1d78-113">Settings files</span></span>

<span data-ttu-id="d1d78-114">*Seçenekleri deseni* bu konuda açıklanan yapılandırma kavramları bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="d1d78-115">Seçenekler, ilgili ayar gruplarını temsil etmek için sınıflar kullanır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="d1d78-116">Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d1d78-117">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d1d78-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d1d78-118">Bu üç paketi içinde yer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="d1d78-119">Uygulama yapılandırması barındırın</span><span class="sxs-lookup"><span data-stu-id="d1d78-119">Host vs. app configuration</span></span>

<span data-ttu-id="d1d78-120">Uygulama yapılandırılmış ve başlatıldı, önce bir *konak* başlatılan ve yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="d1d78-121">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d1d78-122">Bu konuda açıklanan yapılandırma sağlayıcıları kullanarak, hem uygulama hem de konak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="d1d78-123">Ana bilgisayar yapılandırma anahtar-değer çiftleri uygulamanın genel yapılandırmasının bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="d1d78-124">Yapılandırma sağlayıcıları konak oluşturulduğunda kullanılan yapılandırma ve yapılandırma kaynaklarını nasıl etkileyeceğini nasıl barındırmak daha fazla bilgi için bkz: [konak](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="d1d78-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="d1d78-125">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d1d78-125">Default configuration</span></span>

<span data-ttu-id="d1d78-126">Web uygulamaları üzerinde ASP.NET Core tabanlı [yeni dotnet](/dotnet/core/tools/dotnet-new) şablonları çağrı <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="d1d78-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="d1d78-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aşağıdaki sırayla uygulaması için varsayılan yapılandırması sağlar:</span><span class="sxs-lookup"><span data-stu-id="d1d78-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="d1d78-128">Ana bilgisayar yapılandırma öğesinden sağlanır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="d1d78-129">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d1d78-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="d1d78-130">Önek (`ASPNETCORE_`) yapılandırma anahtar-değer çiftleri yüklendiğinde çıkartılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="d1d78-131">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d1d78-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="d1d78-132">Uygulama yapılandırma öğesinden sağlanır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="d1d78-133">*appSettings.JSON* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d1d78-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d1d78-134">*appSettings. {Ortamı} .json* kullanarak [dosya yapılandırma sağlayıcısı](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d1d78-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d1d78-135">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d1d78-136">Ortam değişkenlerini kullanarak [ortam değişkenlerini yapılandırma sağlayıcısı](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d1d78-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="d1d78-137">Özel bir önek kullanılıyorsa (örneğin, `PREFIX_` ile `.AddEnvironmentVariables(prefix: "PREFIX_")`), yapılandırma anahtar-değer çiftleri yüklendiğinde önek yapılandırıldıktan.</span><span class="sxs-lookup"><span data-stu-id="d1d78-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="d1d78-138">Komut satırı bağımsız değişkenleri kullanarak [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d1d78-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="d1d78-139">Yapılandırma sağlayıcıları, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="d1d78-140">Konak hakkında daha fazla bilgi ve <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, bkz: <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="d1d78-141">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="d1d78-141">Security</span></span>

<span data-ttu-id="d1d78-142">Aşağıdaki en iyi benimseme:</span><span class="sxs-lookup"><span data-stu-id="d1d78-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="d1d78-143">Hiçbir zaman parolaları ve diğer hassas verileri yapılandırma sağlayıcısı kodda veya düz metin yapılandırma dosyalarında depolayın.</span><span class="sxs-lookup"><span data-stu-id="d1d78-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="d1d78-144">Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="d1d78-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="d1d78-145">Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin.</span><span class="sxs-lookup"><span data-stu-id="d1d78-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="d1d78-146">Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [gizli dizi Yöneticisi ile geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) (depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. hassas verileri).</span><span class="sxs-lookup"><span data-stu-id="d1d78-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="d1d78-147">Gizli dizi Yöneticisi'ni dosya yapılandırma sağlayıcısı bir JSON dosyası yerel sistemdeki kullanıcı gizli dizileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="d1d78-148">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="d1d78-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) uygulama gizli anahtarlarının güvenli bir şekilde depolanması için bir seçenek.</span><span class="sxs-lookup"><span data-stu-id="d1d78-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="d1d78-150">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="d1d78-151">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="d1d78-151">Hierarchical configuration data</span></span>

<span data-ttu-id="d1d78-152">Yapılandırma API configuration anahtarlarında bir sınırlayıcı kullanarak hiyerarşik veri düzleştirme tarafından hiyerarşik yapılandırma verileri koruma özelliğine sahip.</span><span class="sxs-lookup"><span data-stu-id="d1d78-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="d1d78-153">Aşağıdaki JSON dosyasında iki bölüm yapılandırılmış bir hiyerarşide dört anahtarları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="d1d78-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="d1d78-154">Dosya yapılandırma okuduğunuzda benzersiz anahtarlar özgün hiyerarşik veri yapısını yapılandırma kaynağı korumak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="d1d78-155">Bölümler ve anahtarlar ile bir iki nokta üst üste kullanımını düzleştirilir (`:`) özgün yapıyı korumak için:</span><span class="sxs-lookup"><span data-stu-id="d1d78-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="d1d78-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-156">section0:key0</span></span>
* <span data-ttu-id="d1d78-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-157">section0:key1</span></span>
* <span data-ttu-id="d1d78-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-158">section1:key0</span></span>
* <span data-ttu-id="d1d78-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-159">section1:key1</span></span>

<span data-ttu-id="d1d78-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemlerdir bölümler ve yapılandırma verilerini bir bölümde alt yalıtmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="d1d78-161">Bu yöntem daha sonra açıklanmıştır [GetSection GetChildren ve Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="d1d78-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="d1d78-162">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="d1d78-163">Kurallar</span><span class="sxs-lookup"><span data-stu-id="d1d78-163">Conventions</span></span>

<span data-ttu-id="d1d78-164">Uygulama başlangıcında, yapılandırma kaynaklarını yapılandırma sağlayıcıları belirttiğiniz sırayla okunur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="d1d78-165">Değişiklik algılama uygulayan yapılandırma sağlayıcıları temel alınan bir ayar değiştiğinde yapılandırmayı yeniden yükle seçeneğine sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="d1d78-165">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="d1d78-166">Örneğin, dosya yapılandırma (Bu konunun ilerleyen bölümlerinde açıklanmıştır) sağlayıcısı ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılama uygulayın.</span><span class="sxs-lookup"><span data-stu-id="d1d78-166">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="d1d78-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> uygulamanın kullanılabilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d1d78-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> bir Razor sayfaları yerleştirilebilir <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı yapılandırmasını almak için:</span><span class="sxs-lookup"><span data-stu-id="d1d78-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

<span data-ttu-id="d1d78-169">Bunlar ana bilgisayar tarafından kurduktan olduğunda değil olarak yapılandırma sağlayıcıları DI, faydalanamaz.</span><span class="sxs-lookup"><span data-stu-id="d1d78-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="d1d78-170">Yapılandırma anahtarları, aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="d1d78-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="d1d78-171">Anahtarlar büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-171">Keys are case-insensitive.</span></span> <span data-ttu-id="d1d78-172">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="d1d78-173">Aynı veya farklı yapılandırma sağlayıcıları tarafından aynı anahtar için bir değer ayarlarsanız, anahtarda ayarlanan son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="d1d78-174">Hiyerarşik anahtarları</span><span class="sxs-lookup"><span data-stu-id="d1d78-174">Hierarchical keys</span></span>
  * <span data-ttu-id="d1d78-175">Yapılandırma API'sinin, iki nokta üst üste ayırıcı (`:`) tüm platformlarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="d1d78-176">Ortam değişkenleri, iki nokta üst üste ayırıcı tüm platformlarda çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="d1d78-177">Çift alt çizgi (`__`) tüm platformları tarafından desteklenir ve bir iki nokta üst üste dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="d1d78-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="d1d78-178">Azure anahtar Kasası'nda hiyerarşik tuşlarını `--` (iki kısa çizgi) ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="d1d78-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="d1d78-179">Gizli dizileri uygulama yapılandırma yüklendiğinde tireler iki nokta üst üste ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="d1d78-180"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d1d78-181">Dizi bağlama açıklanan [bir dizi bir sınıfa Bağla](#bind-an-array-to-a-class) bölümü.</span><span class="sxs-lookup"><span data-stu-id="d1d78-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="d1d78-182">Yapılandırma değerleri aşağıdaki kurallar benimseme:</span><span class="sxs-lookup"><span data-stu-id="d1d78-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="d1d78-183">Dizeleri değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-183">Values are strings.</span></span>
* <span data-ttu-id="d1d78-184">Null değerler yapılandırmasında depolanmış veya nesnelere bağlı olamaz.</span><span class="sxs-lookup"><span data-stu-id="d1d78-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="d1d78-185">sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="d1d78-185">Providers</span></span>

<span data-ttu-id="d1d78-186">Aşağıdaki tabloda, ASP.NET Core uygulamaları için kullanılabilir yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="d1d78-187">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="d1d78-187">Provider</span></span> | <span data-ttu-id="d1d78-188">Yapılandırmasından sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="d1d78-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="d1d78-189">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="d1d78-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="d1d78-190">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d1d78-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="d1d78-191">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="d1d78-192">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-192">Command-line parameters</span></span> |
| [<span data-ttu-id="d1d78-193">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="d1d78-194">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="d1d78-194">Custom source</span></span> |
| [<span data-ttu-id="d1d78-195">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="d1d78-196">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-196">Environment variables</span></span> |
| [<span data-ttu-id="d1d78-197">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="d1d78-198">Dosyaları (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="d1d78-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="d1d78-199">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="d1d78-200">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="d1d78-200">Directory files</span></span> |
| [<span data-ttu-id="d1d78-201">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="d1d78-202">Bellek içi koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="d1d78-202">In-memory collections</span></span> |
| <span data-ttu-id="d1d78-203">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="d1d78-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="d1d78-204">Kullanıcı profili dizinde dosya</span><span class="sxs-lookup"><span data-stu-id="d1d78-204">File in the user profile directory</span></span> |

<span data-ttu-id="d1d78-205">Yapılandırma sağlayıcıları başlatma sırasında belirttiğiniz sırayla yapılandırma kaynaklarını okunur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="d1d78-206">Bu konuda açıklanan yapılandırma sağlayıcıları açıklanan alfabetik sırada, bunları kodunuzu düzenleme sırasını değil.</span><span class="sxs-lookup"><span data-stu-id="d1d78-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="d1d78-207">Temel yapılandırma kaynakları için önceliklerinizden uyacak şekilde yapılandırma sağlayıcıları kodunuzu sırası.</span><span class="sxs-lookup"><span data-stu-id="d1d78-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="d1d78-208">Yapılandırma sağlayıcıları, tipik bir dizisidir:</span><span class="sxs-lookup"><span data-stu-id="d1d78-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="d1d78-209">Dosyaları (*appsettings.json*, *appsettings. { Ortam} .json*burada `{Environment}` uygulamanın geçerli barındırma ortamı)</span><span class="sxs-lookup"><span data-stu-id="d1d78-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="d1d78-210">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="d1d78-210">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="d1d78-211">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki yalnızca)</span><span class="sxs-lookup"><span data-stu-id="d1d78-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="d1d78-212">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-212">Environment variables</span></span>
1. <span data-ttu-id="d1d78-213">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-213">Command-line arguments</span></span>

<span data-ttu-id="d1d78-214">Komut satırı yapılandırma sağlayıcısı yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılmak komut satırı bağımsız değişkenlerine izin vermek için sağlayıcıları serisinin son konumlandırmak için yaygın bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="d1d78-215">Yeni bir başlattığınızda bu sağlayıcıları dizi yerine konur <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d1d78-216">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d1d78-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="d1d78-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="d1d78-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="d1d78-218">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> zaman oluşturmaya ek olarak, uygulamanın yapılandırma sağlayıcıları belirtmek için bir konak eklediğiniz tarafından otomatik olarak <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="d1d78-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="d1d78-219">Uygulama için sağlanan yapılandırma <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın başlatma sırasında kullanılabilir dahil olmak üzere `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d1d78-220">Daha fazla bilgi için [erişim yapılandırması başlatılırken](#access-configuration-during-startup) bölümü.</span><span class="sxs-lookup"><span data-stu-id="d1d78-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="d1d78-221">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="d1d78-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Yapılandırma komut satırı bağımsız değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="d1d78-223">Komut satırı yapılandırmasını etkinleştirmek için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> genişletme yöntemi bir örneğinde çağrıldığında <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d1d78-224">`AddCommandLine` Yeni bir başlattığınızda otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d1d78-225">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d1d78-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d1d78-226">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="d1d78-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d1d78-227">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="d1d78-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="d1d78-228">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="d1d78-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d1d78-229">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d1d78-229">Environment variables.</span></span>

<span data-ttu-id="d1d78-230">`CreateDefaultBuilder` Son komut satırı yapılandırma sağlayıcısı ekler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="d1d78-231">Çalışma zamanında geçirilen komut satırı bağımsız değişkenleri yapılandırması diğer sağlayıcılar tarafından ayarlanmış geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="d1d78-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="d1d78-232">`CreateDefaultBuilder` konak oluşturulduğunda işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="d1d78-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="d1d78-233">Bu nedenle, komut satırı yapılandırma etkinleştirildi tarafından `CreateDefaultBuilder` konak nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="d1d78-234">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="d1d78-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="d1d78-235">`AddCommandLine` zaten çağrıldı `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d1d78-236">Uygulama yapılandırması sağlayın ve devam edebilir, yapılandırma komut satırı bağımsız değişkenleri ile geçersiz kılmak gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddCommandLine` son.</span><span class="sxs-lookup"><span data-stu-id="d1d78-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="d1d78-237">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="d1d78-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="d1d78-238">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="d1d78-238">**Example**</span></span>

<span data-ttu-id="d1d78-239">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="d1d78-240">Proje dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="d1d78-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="d1d78-241">Bir komut satırı bağımsız değişken `dotnet run` komutu `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="d1d78-242">Uygulama çalıştıktan sonra bir uygulamaya tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d1d78-243">Çıkış için sağlanan yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çifti içeren gözlemleyin `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="d1d78-244">Arguments</span><span class="sxs-lookup"><span data-stu-id="d1d78-244">Arguments</span></span>

<span data-ttu-id="d1d78-245">Değeri bir eşittir işareti gelmelidir (`=`), veya bir önek anahtarı olmalıdır (`--` veya `/`) zaman değeri bir boşluk izler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="d1d78-246">Değer bir eşittir işareti kullanılıyorsa, null olabilir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="d1d78-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="d1d78-247">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="d1d78-247">Key prefix</span></span>               | <span data-ttu-id="d1d78-248">Örnek</span><span class="sxs-lookup"><span data-stu-id="d1d78-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="d1d78-249">Önek yok</span><span class="sxs-lookup"><span data-stu-id="d1d78-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="d1d78-250">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="d1d78-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="d1d78-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="d1d78-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="d1d78-252">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="d1d78-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="d1d78-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="d1d78-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="d1d78-254">Aynı komut içinde komut satırı bağımsız değişkeni bir eşittir işareti ile bir alanı kullanan anahtar-değer çiftleri kullanan anahtar-değer çiftleri karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="d1d78-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="d1d78-255">Örnek komutları:</span><span class="sxs-lookup"><span data-stu-id="d1d78-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="d1d78-256">Geçiş eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="d1d78-256">Switch mappings</span></span>

<span data-ttu-id="d1d78-257">Anahtar, anahtar adı değiştirme mantıksal eşlemeler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="d1d78-258">El ile yapı kurarken yapılandırmayla bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, anahtar değişiklik için bir sözlük sağlayabilir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1d78-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="d1d78-259">Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="d1d78-260">Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) anahtar-değer çifti uygulamanın yapılandırmasını döndürülmek geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="d1d78-261">Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).</span><span class="sxs-lookup"><span data-stu-id="d1d78-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="d1d78-262">Eşlemeleri sözlüğü anahtar kuralları anahtarı:</span><span class="sxs-lookup"><span data-stu-id="d1d78-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="d1d78-263">Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).</span><span class="sxs-lookup"><span data-stu-id="d1d78-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="d1d78-264">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="d1d78-265">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="d1d78-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="d1d78-266">Çağrı yukarıdaki örnekte gösterildiği gibi `CreateDefaultBuilder` anahtar eşlemeleri kullanıldığında bağımsız değişkenleri geçirmek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="d1d78-267">`CreateDefaultBuilder` yöntemin `AddCommandLine` çağrısı, eşleştirilen anahtarları içermez ve anahtar eşleme sözlüğe geçirilecek bir yolu yoktur `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d1d78-268">Bağımsız değişkenler eşlenmiş bir anahtar içerir ve geçirilen `CreateDefaultBuilder`, kendi `AddCommandLine` sağlayıcısı başarısız ile başlatmak bir <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="d1d78-269">Bağımsız değişkenler için çözüm olmayan `CreateDefaultBuilder` ancak bunun yerine izin vermek için `ConfigurationBuilder` yöntemin `AddCommandLine` hem bağımsız değişkenler hem de anahtar eşleme sözlük işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1d78-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="d1d78-270">Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="d1d78-271">Anahtar</span><span class="sxs-lookup"><span data-stu-id="d1d78-271">Key</span></span>       | <span data-ttu-id="d1d78-272">Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="d1d78-273">Anahtar eşlemeli anahtarları uygulamayı başlatırken kullandıysanız, yapılandırma sözlüğü tarafından sağlanan anahtardaki yapılandırma değeri alır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="d1d78-274">Önceki komutu çalıştırdıktan sonra yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="d1d78-275">Anahtar</span><span class="sxs-lookup"><span data-stu-id="d1d78-275">Key</span></span>               | <span data-ttu-id="d1d78-276">Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="d1d78-277">Ortam değişkenlerini yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="d1d78-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Yapılandırma ortam değişkeni anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="d1d78-279">Ortam değişkenlerini yapılandırma etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="d1d78-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) ortam değişkenlerini yapılandırma Sağlayıcısı'nı kullanarak uygulama yapılandırması geçersiz kılabilirsiniz Azure portalında ortam değişkenlerini ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="d1d78-281">Daha fazla bilgi için [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="d1d78-281">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="d1d78-282">`AddEnvironmentVariables` ortam değişkenlerini ön eki için otomatik olarak çağrılır `ASPNETCORE_` yeni başlatırken <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-282">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="d1d78-283">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d1d78-283">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d1d78-284">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="d1d78-284">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d1d78-285">Uygulama yapılandırması çağırarak unprefixed ortam değişkenlerinden `AddEnvironmentVariables` öneki olmadan.</span><span class="sxs-lookup"><span data-stu-id="d1d78-285">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="d1d78-286">İsteğe bağlı yapılandırmasından *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="d1d78-286">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="d1d78-287">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="d1d78-287">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d1d78-288">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d1d78-288">Command-line arguments.</span></span>

<span data-ttu-id="d1d78-289">Yapılandırma kullanıcı parolalarının kurulduktan sonra ortam değişkenlerini yapılandırma sağlayıcısı denir ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="d1d78-289">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="d1d78-290">Bu konumda sağlayıcıya çağrı ortam değişkenlerini okuma yapılandırması tarafından kullanıcı parolalarını ayarlanmış geçersiz kılmak için çalışma zamanında sağlar ve *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="d1d78-290">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="d1d78-291">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken.</span><span class="sxs-lookup"><span data-stu-id="d1d78-291">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="d1d78-292">Ek ortam değişkenleri uygulama yapılandırmasından sağlamanız gerekiyorsa, uygulamanın ek sağlayıcılar Çağır <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ve çağrı `AddEnvironmentVariables` ön eki.</span><span class="sxs-lookup"><span data-stu-id="d1d78-292">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="d1d78-293">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="d1d78-293">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="d1d78-294">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="d1d78-294">**Example**</span></span>

<span data-ttu-id="d1d78-295">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` bir çağrı içerdiğine ana bilgisayar oluşturmak için `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-295">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="d1d78-296">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d1d78-296">Run the sample app.</span></span> <span data-ttu-id="d1d78-297">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-297">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d1d78-298">Çıkış ortam değişkeni için anahtar-değer çifti içeren gözlemleyin `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-298">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="d1d78-299">Değeri, uygulamanın çalıştığı, genellikle ortamın yansıtır `Development` yerel olarak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="d1d78-299">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="d1d78-300">Ortam değişkenlerini kısa uygulama tarafından işlenen listesini tutmak için aşağıdaki ile başlayan bu ortam değişkenleri uygulama filtreleri:</span><span class="sxs-lookup"><span data-stu-id="d1d78-300">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="d1d78-301">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="d1d78-301">ASPNETCORE_</span></span>
* <span data-ttu-id="d1d78-302">URL'leri</span><span class="sxs-lookup"><span data-stu-id="d1d78-302">urls</span></span>
* <span data-ttu-id="d1d78-303">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="d1d78-303">Logging</span></span>
* <span data-ttu-id="d1d78-304">ORTAM</span><span class="sxs-lookup"><span data-stu-id="d1d78-304">ENVIRONMENT</span></span>
* <span data-ttu-id="d1d78-305">contentRoot</span><span class="sxs-lookup"><span data-stu-id="d1d78-305">contentRoot</span></span>
* <span data-ttu-id="d1d78-306">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="d1d78-306">AllowedHosts</span></span>
* <span data-ttu-id="d1d78-307">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="d1d78-307">applicationName</span></span>
* <span data-ttu-id="d1d78-308">komut satırı</span><span class="sxs-lookup"><span data-stu-id="d1d78-308">CommandLine</span></span>

<span data-ttu-id="d1d78-309">Tüm ortam değişkenlerinin uygulamaya kullanılabilir kullanıma sunmak isterseniz değiştirme `FilteredConfiguration` içinde *Pages/Index.cshtml.cs* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="d1d78-309">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="d1d78-310">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="d1d78-310">Prefixes</span></span>

<span data-ttu-id="d1d78-311">Uygulamanın yapılandırma yüklendi ortam değişkenleri, bir ön ek sağladığında filtrelenir `AddEnvironmentVariables` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1d78-311">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="d1d78-312">Örneğin, ortam değişkenlerini önek filtresi için `CUSTOM_`, yapılandırma sağlayıcısı için önek sağlayın:</span><span class="sxs-lookup"><span data-stu-id="d1d78-312">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="d1d78-313">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek yapılandırıldıktan devre dışı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-313">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="d1d78-314">Statik kolaylık yöntemi `CreateDefaultBuilder` oluşturur bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> uygulamanın ana bilgisayar oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="d1d78-314">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="d1d78-315">Zaman `WebHostBuilder` olan oluşturulan, kendi ana bilgisayar yapılandırması ön ekine sahip ortam değişkenleri içindeki bulduğu `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-315">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="d1d78-316">**Bağlantı dizesi ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="d1d78-316">**Connection string prefixes**</span></span>

<span data-ttu-id="d1d78-317">Yapılandırma API'si, dört bağlantı dizesi ortam değişkenleri için app ortamı için Azure bağlantı dizelerini yapılandırılmasıyla ilgili özel işleme kurallarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-317">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="d1d78-318">Önek yok belirtilirse tabloda gösterilen ön ekine sahip ortam değişkenleri uygulamaya yüklenir `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-318">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="d1d78-319">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="d1d78-319">Connection string prefix</span></span> | <span data-ttu-id="d1d78-320">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="d1d78-320">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="d1d78-321">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="d1d78-321">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="d1d78-322">MySQL</span><span class="sxs-lookup"><span data-stu-id="d1d78-322">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="d1d78-323">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="d1d78-323">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="d1d78-324">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d1d78-324">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="d1d78-325">Ne zaman bir ortam değişkeni bulunur ve herhangi bir tabloda gösterilen dört öneklerini yapılandırmasını yüklendi:</span><span class="sxs-lookup"><span data-stu-id="d1d78-325">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="d1d78-326">Yapılandırma anahtarı ortam değişkeni ön eki kaldırma ve yapılandırma anahtar bölümünü ekleyerek oluşturulur (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="d1d78-326">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="d1d78-327">Veritabanı bağlantı sağlayıcısı temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (dışında `CUSTOMCONNSTR_`, belirtilen sağlayıcı yok sahiptir).</span><span class="sxs-lookup"><span data-stu-id="d1d78-327">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="d1d78-328">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="d1d78-328">Environment variable key</span></span> | <span data-ttu-id="d1d78-329">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="d1d78-329">Converted configuration key</span></span> | <span data-ttu-id="d1d78-330">Sağlayıcı Yapılandırması girdisi</span><span class="sxs-lookup"><span data-stu-id="d1d78-330">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d1d78-331">Yapılandırma girişi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-331">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d1d78-332">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d1d78-332">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d1d78-333">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="d1d78-333">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d1d78-334">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d1d78-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d1d78-335">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d1d78-335">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="d1d78-336">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d1d78-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="d1d78-337">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d1d78-337">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="d1d78-338">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-338">File Configuration Provider</span></span>

<span data-ttu-id="d1d78-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> yapılandırma dosya sisteminden yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="d1d78-340">Aşağıdaki yapılandırma sağlayıcıları için belirli dosya türleri ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-340">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="d1d78-341">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-341">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="d1d78-342">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-342">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="d1d78-343">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-343">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="d1d78-344">INI yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-344">INI Configuration Provider</span></span>

<span data-ttu-id="d1d78-345"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Yapılandırma ını dosyası anahtar-değer çiftleri zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-345">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="d1d78-346">INI dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-346">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d1d78-347">İki nokta üst üste INI dosya yapılandırması bölüm ayırıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-347">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="d1d78-348">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="d1d78-348">Overloads permit specifying:</span></span>

* <span data-ttu-id="d1d78-349">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-349">Whether the file is optional.</span></span>
* <span data-ttu-id="d1d78-350">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="d1d78-350">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d1d78-351"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-351">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d1d78-352">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="d1d78-352">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="d1d78-353">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-353">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d1d78-354">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-354">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-355">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="d1d78-355">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="d1d78-356">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-356">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d1d78-357">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-357">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-358">Genel bir INI yapılandırma dosyası örneği:</span><span class="sxs-lookup"><span data-stu-id="d1d78-358">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="d1d78-359">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="d1d78-359">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d1d78-360">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-360">section0:key0</span></span>
* <span data-ttu-id="d1d78-361">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-361">section0:key1</span></span>
* <span data-ttu-id="d1d78-362">section1:subsection:Key</span><span class="sxs-lookup"><span data-stu-id="d1d78-362">section1:subsection:key</span></span>
* <span data-ttu-id="d1d78-363">section2:subsection0:Key</span><span class="sxs-lookup"><span data-stu-id="d1d78-363">section2:subsection0:key</span></span>
* <span data-ttu-id="d1d78-364">section2:subsection1:Key</span><span class="sxs-lookup"><span data-stu-id="d1d78-364">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="d1d78-365">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-365">JSON Configuration Provider</span></span>

<span data-ttu-id="d1d78-366"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Yapılandırma JSON dosyası anahtar-değer çiftlerinden çalışma zamanı sırasında yükler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-366">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="d1d78-367">JSON dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-367">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d1d78-368">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="d1d78-368">Overloads permit specifying:</span></span>

* <span data-ttu-id="d1d78-369">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-369">Whether the file is optional.</span></span>
* <span data-ttu-id="d1d78-370">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="d1d78-370">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d1d78-371"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-371">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d1d78-372">`AddJsonFile` Yeni bir başlattığınızda iki kez otomatik olarak çağrılır <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> ile <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-372">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="d1d78-373">Yöntem yapılandırmasından yüklenemedi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-373">The method is called to load configuration from:</span></span>

* <span data-ttu-id="d1d78-374">*appSettings.JSON* &ndash; bu dosyayı ilk okuyun.</span><span class="sxs-lookup"><span data-stu-id="d1d78-374">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="d1d78-375">Dosyanın ortam sürümü tarafından sağlanan değerleri geçersiz kılabilir *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d1d78-375">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="d1d78-376">*appSettings. {Ortamı} .json* &ndash; dosyanın ortam sürümünü temel alınarak yüklenir [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="d1d78-376">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="d1d78-377">Daha fazla bilgi için [Web ana bilgisayarı: Bir konak ayarlamanız](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="d1d78-377">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="d1d78-378">`CreateDefaultBuilder` Ayrıca yükler:</span><span class="sxs-lookup"><span data-stu-id="d1d78-378">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d1d78-379">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d1d78-379">Environment variables.</span></span>
* <span data-ttu-id="d1d78-380">[Kullanıcı parolaları (gizli dizi Yöneticisi)](xref:security/app-secrets) (geliştirme ortamındaki).</span><span class="sxs-lookup"><span data-stu-id="d1d78-380">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="d1d78-381">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d1d78-381">Command-line arguments.</span></span>

<span data-ttu-id="d1d78-382">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-382">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="d1d78-383">Bu nedenle, yapılandırma kümesi kullanıcı parolaları, ortam değişkenleri ve komut satırı bağımsız değişkenleri geçersiz kılma *appsettings* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="d1d78-383">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="d1d78-384">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırma dosyaları için dışında belirtmek için konak oluştururken *appsettings.json* ve *appsettings. { Ortam} .json*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-384">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="d1d78-385">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-385">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d1d78-386">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-386">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-387">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="d1d78-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="d1d78-388">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-388">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d1d78-389">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-389">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-390">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="d1d78-390">**Example**</span></span>

<span data-ttu-id="d1d78-391">Örnek uygulamayı statik kolaylık yöntemi yararlanır `CreateDefaultBuilder` iki çağrıları içeren ana bilgisayar oluşturmak için `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-391">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="d1d78-392">Yapılandırma yüklenir *appsettings.json* ve *appsettings. { Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="d1d78-392">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="d1d78-393">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d1d78-393">Run the sample app.</span></span> <span data-ttu-id="d1d78-394">Uygulamaya bir tarayıcıda `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-394">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d1d78-395">Çıkış ortamına bağlı olarak tabloda gösterilen yapılandırması için anahtar-değer çiftleri içeren gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="d1d78-395">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="d1d78-396">Günlük kaydı yapılandırması tuşlarını iki nokta üst üste (`:`) hiyerarşik ayırıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="d1d78-396">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="d1d78-397">Anahtar</span><span class="sxs-lookup"><span data-stu-id="d1d78-397">Key</span></span>                        | <span data-ttu-id="d1d78-398">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="d1d78-398">Development Value</span></span> | <span data-ttu-id="d1d78-399">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="d1d78-399">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="d1d78-400">Günlüğe kaydetme: LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="d1d78-400">Logging:LogLevel:System</span></span>    | <span data-ttu-id="d1d78-401">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="d1d78-401">Information</span></span>       | <span data-ttu-id="d1d78-402">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="d1d78-402">Information</span></span>      |
| <span data-ttu-id="d1d78-403">Günlüğe kaydetme: LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="d1d78-403">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="d1d78-404">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="d1d78-404">Information</span></span>       | <span data-ttu-id="d1d78-405">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="d1d78-405">Information</span></span>      |
| <span data-ttu-id="d1d78-406">Günlüğe kaydetme: LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="d1d78-406">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="d1d78-407">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="d1d78-407">Debug</span></span>             | <span data-ttu-id="d1d78-408">Hata</span><span class="sxs-lookup"><span data-stu-id="d1d78-408">Error</span></span>            |
| <span data-ttu-id="d1d78-409">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="d1d78-409">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="d1d78-410">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-410">XML Configuration Provider</span></span>

<span data-ttu-id="d1d78-411"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Yapılandırma XML dosyası anahtar-değer çiftlerinin zamanında yükler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-411">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="d1d78-412">XML dosyası yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-412">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d1d78-413">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="d1d78-413">Overloads permit specifying:</span></span>

* <span data-ttu-id="d1d78-414">Dosya isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-414">Whether the file is optional.</span></span>
* <span data-ttu-id="d1d78-415">Yoksa dosyayı değiştirirse yapılandırma yeniden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="d1d78-415">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d1d78-416"><xref:Microsoft.Extensions.FileProviders.IFileProvider> Dosyaya erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-416">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d1d78-417">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayıldı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-417">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="d1d78-418">Bir belge türü tanımı (DTD'nin) veya ad alanı dosyasında belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="d1d78-418">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="d1d78-419">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="d1d78-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="d1d78-420">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d1d78-421">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-422">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="d1d78-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="d1d78-423">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-423">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d1d78-424">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-424">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-425">XML yapılandırma dosyalarını, yinelenen bölümler için ayrı bir öğe adları kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d1d78-425">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="d1d78-426">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="d1d78-426">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d1d78-427">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-427">section0:key0</span></span>
* <span data-ttu-id="d1d78-428">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-428">section0:key1</span></span>
* <span data-ttu-id="d1d78-429">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-429">section1:key0</span></span>
* <span data-ttu-id="d1d78-430">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-430">section1:key1</span></span>

<span data-ttu-id="d1d78-431">İş öğesi adının aynısını kullanın öğeleri, yinelenen `name` özniteliği öğeleri ayırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-431">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="d1d78-432">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="d1d78-432">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d1d78-433">Bölüm: section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-433">section:section0:key:key0</span></span>
* <span data-ttu-id="d1d78-434">Bölüm: section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-434">section:section0:key:key1</span></span>
* <span data-ttu-id="d1d78-435">Bölüm: section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-435">section:section1:key:key0</span></span>
* <span data-ttu-id="d1d78-436">Bölüm: section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-436">section:section1:key:key1</span></span>

<span data-ttu-id="d1d78-437">Öznitelik değerlerini sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d1d78-437">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="d1d78-438">Önceki yapılandırma dosyasını aşağıdaki anahtarları yükler `value`:</span><span class="sxs-lookup"><span data-stu-id="d1d78-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d1d78-439">Anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="d1d78-439">key:attribute</span></span>
* <span data-ttu-id="d1d78-440">Bölüm: anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="d1d78-440">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="d1d78-441">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-441">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="d1d78-442"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bir dizin dosyalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-442">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="d1d78-443">Anahtar dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-443">The key is the file name.</span></span> <span data-ttu-id="d1d78-444">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-444">The value contains the file's contents.</span></span> <span data-ttu-id="d1d78-445">Dosya başına anahtar yapılandırma sağlayıcısı Docker'da barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-445">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="d1d78-446">Dosya başına anahtar yapılandırmasını etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-446">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="d1d78-447">`directoryPath` Dosyalar için mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-447">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="d1d78-448">Aşırı yüklemeler belirtme izin ver:</span><span class="sxs-lookup"><span data-stu-id="d1d78-448">Overloads permit specifying:</span></span>

* <span data-ttu-id="d1d78-449">Bir `Action<KeyPerFileConfigurationSource>` kaynağını yapılandırır temsilci.</span><span class="sxs-lookup"><span data-stu-id="d1d78-449">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="d1d78-450">Dizin isteğe bağlı olup olmadığını ve dizinin yolu.</span><span class="sxs-lookup"><span data-stu-id="d1d78-450">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="d1d78-451">Çift alt çizgi (`__`) dosya adları içinde yapılandırma anahtar sınırlayıcı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-451">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="d1d78-452">Örneğin, dosya adı `Logging__LogLevel__System` üretir yapılandırma anahtarı `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-452">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="d1d78-453">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="d1d78-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="d1d78-454">Temel yol ile ayarlanır <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="d1d78-455">`SetBasePath` içinde [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-456">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="d1d78-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="d1d78-457">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-457">Memory Configuration Provider</span></span>

<span data-ttu-id="d1d78-458"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Bir bellek içi koleksiyonu yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-458">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="d1d78-459">Bellek içi toplama yapılandırması etkinleştirmek için çağrı <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> örneği genişletme yöntemini <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-459">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d1d78-460">Yapılandırma sağlayıcısı ile başlatılabilir bir `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-460">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="d1d78-461">Çağrı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uygulamanın yapılandırmasını belirlemek için konak oluştururken:</span><span class="sxs-lookup"><span data-stu-id="d1d78-461">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="d1d78-462">Oluştururken bir <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> doğrudan çağırmak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> yapılandırmayla:</span><span class="sxs-lookup"><span data-stu-id="d1d78-462">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="d1d78-463">GetValue</span><span class="sxs-lookup"><span data-stu-id="d1d78-463">GetValue</span></span>

<span data-ttu-id="d1d78-464">[ConfigurationBinder.GetValue&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) bir değeri belirtilen bir anahtarla yapılandırmasından ayıklar ve onu belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d1d78-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="d1d78-465">Aşırı yükleme anahtarı bulunmazsa, varsayılan bir değer sağlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-465">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="d1d78-466">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d1d78-466">The following example:</span></span>

* <span data-ttu-id="d1d78-467">Dize değeri yapılandırmasından anahtarıyla ayıklar `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-467">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="d1d78-468">Varsa `NumberKey` yapılandırma anahtarları, varsayılan değeri bulunamadığında `99` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-468">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="d1d78-469">Değer olarak türleri bir `int`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-469">Types the value as an `int`.</span></span>
* <span data-ttu-id="d1d78-470">İçinde bir değer depolar `NumberConfig` özellik sayfası tarafından kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="d1d78-470">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="d1d78-471">GetSection, GetChildren ve var.</span><span class="sxs-lookup"><span data-stu-id="d1d78-471">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="d1d78-472">Aşağıdaki örneklerde için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="d1d78-472">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="d1d78-473">Dört anahtarları içeren bir çift alt bölümleri içerir, iki bölümlerde bulunur:</span><span class="sxs-lookup"><span data-stu-id="d1d78-473">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="d1d78-474">Dosya yapılandırma okuduğunuzda aşağıdaki benzersiz hiyerarşik anahtarları yapılandırma değerleri tutmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="d1d78-474">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="d1d78-475">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-475">section0:key0</span></span>
* <span data-ttu-id="d1d78-476">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-476">section0:key1</span></span>
* <span data-ttu-id="d1d78-477">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-477">section1:key0</span></span>
* <span data-ttu-id="d1d78-478">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-478">section1:key1</span></span>
* <span data-ttu-id="d1d78-479">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-479">section2:subsection0:key0</span></span>
* <span data-ttu-id="d1d78-480">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-480">section2:subsection0:key1</span></span>
* <span data-ttu-id="d1d78-481">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="d1d78-481">section2:subsection1:key0</span></span>
* <span data-ttu-id="d1d78-482">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="d1d78-482">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="d1d78-483">GetSection</span><span class="sxs-lookup"><span data-stu-id="d1d78-483">GetSection</span></span>

<span data-ttu-id="d1d78-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) yapılandırma bölümüne belirtilen alt anahtarını ayıklar.</span><span class="sxs-lookup"><span data-stu-id="d1d78-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="d1d78-485">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-485">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-486">Döndürülecek bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> yalnızca anahtar-değer çiftlerini içeren `section1`, çağrı `GetSection` ve bölüm adı verin:</span><span class="sxs-lookup"><span data-stu-id="d1d78-486">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="d1d78-487">`configSection` Bir değeri, yalnızca bir anahtar ve bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-487">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="d1d78-488">Benzer şekilde, anahtarları değerlerini almak için `section2:subsection0`, çağrı `GetSection` ve bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="d1d78-488">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="d1d78-489">`GetSection` hiç dönmüyor `null`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-489">`GetSection` never returns `null`.</span></span> <span data-ttu-id="d1d78-490">Eşleşen bir bölümü olmadığından bulunamazsa, boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d1d78-490">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="d1d78-491">Zaman `GetSection` eşleşen bir bölümü döndürür <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuş değil.</span><span class="sxs-lookup"><span data-stu-id="d1d78-491">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="d1d78-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> bölümü varsa, döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d1d78-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="d1d78-493">GetChildren</span><span class="sxs-lookup"><span data-stu-id="d1d78-493">GetChildren</span></span>

<span data-ttu-id="d1d78-494">Bir çağrı [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) üzerinde `section2` alır bir `IEnumerable<IConfigurationSection>` içeren:</span><span class="sxs-lookup"><span data-stu-id="d1d78-494">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="d1d78-495">Var</span><span class="sxs-lookup"><span data-stu-id="d1d78-495">Exists</span></span>

<span data-ttu-id="d1d78-496">Kullanım [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) yapılandırma bölümü olup olmadığını belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="d1d78-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="d1d78-497">Belirtilen örnek veri `sectionExists` olduğu `false` olmadığından bir `section2:subsection2` yapılandırma verilerini bir bölümde.</span><span class="sxs-lookup"><span data-stu-id="d1d78-497">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="d1d78-498">Bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="d1d78-498">Bind to a class</span></span>

<span data-ttu-id="d1d78-499">Yapılandırma kullanarak ilgili ayar gruplarını temsil eden sınıflar için bağlanabilir *seçenekleri deseni*.</span><span class="sxs-lookup"><span data-stu-id="d1d78-499">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="d1d78-500">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-500">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d1d78-501">Yapılandırma değerleri döndürülür dizeler olarak çağıran <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> oluşumu sağlayan [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesneleri.</span><span class="sxs-lookup"><span data-stu-id="d1d78-501">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="d1d78-502">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-502">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-503">Örnek uygulamayı içeren bir `Starship` modeli (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="d1d78-503">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="d1d78-504">`starship` Bölümünü *starship.json* örnek uygulamayı yapılandırma yüklemek için JSON yapılandırma sağlayıcısı kullandığında yapılandırma dosyası oluşturur:</span><span class="sxs-lookup"><span data-stu-id="d1d78-504">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="d1d78-505">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="d1d78-505">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="d1d78-506">Anahtar</span><span class="sxs-lookup"><span data-stu-id="d1d78-506">Key</span></span>                   | <span data-ttu-id="d1d78-507">Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-507">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="d1d78-508">starship: adı</span><span class="sxs-lookup"><span data-stu-id="d1d78-508">starship:name</span></span>         | <span data-ttu-id="d1d78-509">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="d1d78-509">USS Enterprise</span></span>                                    |
| <span data-ttu-id="d1d78-510">starship:registry</span><span class="sxs-lookup"><span data-stu-id="d1d78-510">starship:registry</span></span>     | <span data-ttu-id="d1d78-511">NCC 1701</span><span class="sxs-lookup"><span data-stu-id="d1d78-511">NCC-1701</span></span>                                          |
| <span data-ttu-id="d1d78-512">starship:class</span><span class="sxs-lookup"><span data-stu-id="d1d78-512">starship:class</span></span>        | <span data-ttu-id="d1d78-513">Anayasa</span><span class="sxs-lookup"><span data-stu-id="d1d78-513">Constitution</span></span>                                      |
| <span data-ttu-id="d1d78-514">starship:length</span><span class="sxs-lookup"><span data-stu-id="d1d78-514">starship:length</span></span>       | <span data-ttu-id="d1d78-515">304.8</span><span class="sxs-lookup"><span data-stu-id="d1d78-515">304.8</span></span>                                             |
| <span data-ttu-id="d1d78-516">starship: yetkilendirilen</span><span class="sxs-lookup"><span data-stu-id="d1d78-516">starship:commissioned</span></span> | <span data-ttu-id="d1d78-517">False</span><span class="sxs-lookup"><span data-stu-id="d1d78-517">False</span></span>                                             |
| <span data-ttu-id="d1d78-518">Ticari marka</span><span class="sxs-lookup"><span data-stu-id="d1d78-518">trademark</span></span>             | <span data-ttu-id="d1d78-519">Paramount resimleri Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="d1d78-519">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="d1d78-520">Örnek Uygulama çağrıları `GetSection` ile `starship` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-520">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="d1d78-521">`starship` Anahtar-değer çiftleridir yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="d1d78-521">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="d1d78-522">`Bind` Bir örneğini geçirerek Altbölüm yöntemi çağrıldığında `Starship` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-522">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="d1d78-523">Örnek değerleri bağlandıktan sonra işleme için bir özellik için örneği atanır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-523">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="d1d78-524">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-524">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="d1d78-525">Bir nesne grafiği için bağlama</span><span class="sxs-lookup"><span data-stu-id="d1d78-525">Bind to an object graph</span></span>

<span data-ttu-id="d1d78-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Tüm POCO Nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="d1d78-527">`Bind` içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-527">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-528">Örnek içeren bir `TvShow` olan nesne grafiğini içeren model `Metadata` ve `Actors` sınıfları (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="d1d78-528">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="d1d78-529">Örnek uygulamanın bir *tvshow.xml* yapılandırma verilerini içeren dosya:</span><span class="sxs-lookup"><span data-stu-id="d1d78-529">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="d1d78-530">Yapılandırma tüm bağlı `TvShow` Nesne grafiği ile `Bind` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d1d78-530">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="d1d78-531">İlişkili örneği, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-531">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="d1d78-532">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="d1d78-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="d1d78-533">`Get<T>` kullanmaktan daha kullanışlı olan `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-533">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="d1d78-534">Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle birlikte işlemek için kullanılan özellik doğrudan atanan bağlı örnek sağlar:</span><span class="sxs-lookup"><span data-stu-id="d1d78-534">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="d1d78-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> içinde [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="d1d78-536">`Get<T>` ASP.NET Core 1.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-536">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="d1d78-537">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-537">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="d1d78-538">Bir dizi bir sınıfa Bağla</span><span class="sxs-lookup"><span data-stu-id="d1d78-538">Bind an array to a class</span></span>

<span data-ttu-id="d1d78-539">*Örnek uygulama, bu bölümde açıklanan kavramları göstermektedir.*</span><span class="sxs-lookup"><span data-stu-id="d1d78-539">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="d1d78-540"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Dizi dizinleri yapılandırma anahtarlarını kullanarak nesnelere bağlama dizilerini destekler.</span><span class="sxs-lookup"><span data-stu-id="d1d78-540">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d1d78-541">Sayısal bir anahtar kesimi sunan herhangi bir dizi biçimi (`:0:`, `:1:`, &hellip; `:{n}:`) dizisi bağlama POCO sınıfı dizisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-541">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="d1d78-542">' Bağlama '' bulunduğu [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-542">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="d1d78-543">Bağlama, kural olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-543">Binding is provided by convention.</span></span> <span data-ttu-id="d1d78-544">Özel yapılandırma sağlayıcıları dizi bağlama uygulamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-544">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="d1d78-545">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="d1d78-545">**In-memory array processing**</span></span>

<span data-ttu-id="d1d78-546">Yapılandırma anahtarları ve değerleri aşağıdaki tabloda gösterilen göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="d1d78-546">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="d1d78-547">Anahtar</span><span class="sxs-lookup"><span data-stu-id="d1d78-547">Key</span></span>             | <span data-ttu-id="d1d78-548">Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-548">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d1d78-549">dizi: girişler: 0</span><span class="sxs-lookup"><span data-stu-id="d1d78-549">array:entries:0</span></span> | <span data-ttu-id="d1d78-550">value0</span><span class="sxs-lookup"><span data-stu-id="d1d78-550">value0</span></span> |
| <span data-ttu-id="d1d78-551">dizi: girişler: 1</span><span class="sxs-lookup"><span data-stu-id="d1d78-551">array:entries:1</span></span> | <span data-ttu-id="d1d78-552">Değer1</span><span class="sxs-lookup"><span data-stu-id="d1d78-552">value1</span></span> |
| <span data-ttu-id="d1d78-553">dizi: girişler: 2</span><span class="sxs-lookup"><span data-stu-id="d1d78-553">array:entries:2</span></span> | <span data-ttu-id="d1d78-554">Value2</span><span class="sxs-lookup"><span data-stu-id="d1d78-554">value2</span></span> |
| <span data-ttu-id="d1d78-555">dizi: girişler: 4</span><span class="sxs-lookup"><span data-stu-id="d1d78-555">array:entries:4</span></span> | <span data-ttu-id="d1d78-556">Değer4</span><span class="sxs-lookup"><span data-stu-id="d1d78-556">value4</span></span> |
| <span data-ttu-id="d1d78-557">dizi: girişler: 5</span><span class="sxs-lookup"><span data-stu-id="d1d78-557">array:entries:5</span></span> | <span data-ttu-id="d1d78-558">Değeri5</span><span class="sxs-lookup"><span data-stu-id="d1d78-558">value5</span></span> |

<span data-ttu-id="d1d78-559">Bellek yapılandırma sağlayıcısı kullanan örnek uygulamasında, bu anahtarların ve değerlerin yüklenir:</span><span class="sxs-lookup"><span data-stu-id="d1d78-559">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="d1d78-560">Dizi dizini için bir değer atlar &num;3.</span><span class="sxs-lookup"><span data-stu-id="d1d78-560">The array skips a value for index &num;3.</span></span> <span data-ttu-id="d1d78-561">Yapılandırma bağlayıcı temizleyin, bu dizi için bir nesne bağlamanın sonucunu gösterilmiştir birazdan haline geldikten bağlama null değerler veya null girişler bağımlı nesneleri oluşturma yeteneğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-561">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="d1d78-562">Örnek uygulamada, ilişkili yapılandırma verilerini tutmak bir POCO sınıf kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d1d78-562">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="d1d78-563">Yapılandırma verilerini nesnesine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-563">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="d1d78-564">`GetSection` içinde [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d1d78-564">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d1d78-565">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) söz dizimi de kullanılabilir, daha küçük kod sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="d1d78-565">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="d1d78-566">İlişkili nesne, örneği `ArrayExample`, yapılandırmasından dizisi verileri alır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-566">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="d1d78-567">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="d1d78-567">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d1d78-568">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-568">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d1d78-569">0</span><span class="sxs-lookup"><span data-stu-id="d1d78-569">0</span></span>                            | <span data-ttu-id="d1d78-570">value0</span><span class="sxs-lookup"><span data-stu-id="d1d78-570">value0</span></span>                       |
| <span data-ttu-id="d1d78-571">1.</span><span class="sxs-lookup"><span data-stu-id="d1d78-571">1</span></span>                            | <span data-ttu-id="d1d78-572">Değer1</span><span class="sxs-lookup"><span data-stu-id="d1d78-572">value1</span></span>                       |
| <span data-ttu-id="d1d78-573">2</span><span class="sxs-lookup"><span data-stu-id="d1d78-573">2</span></span>                            | <span data-ttu-id="d1d78-574">Value2</span><span class="sxs-lookup"><span data-stu-id="d1d78-574">value2</span></span>                       |
| <span data-ttu-id="d1d78-575">3</span><span class="sxs-lookup"><span data-stu-id="d1d78-575">3</span></span>                            | <span data-ttu-id="d1d78-576">Değer4</span><span class="sxs-lookup"><span data-stu-id="d1d78-576">value4</span></span>                       |
| <span data-ttu-id="d1d78-577">4</span><span class="sxs-lookup"><span data-stu-id="d1d78-577">4</span></span>                            | <span data-ttu-id="d1d78-578">Değeri5</span><span class="sxs-lookup"><span data-stu-id="d1d78-578">value5</span></span>                       |

<span data-ttu-id="d1d78-579">Dizin &num;3'te ilişkili nesne için yapılandırma verilerini tutan `array:4` yapılandırma anahtarı ve değeri `value4`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-579">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="d1d78-580">Bir diziyi içeren yapılandırma verilerini bağlandığında, dizi dizinleri yapılandırma anahtarları yalnızca nesne oluşturma sırasında yapılandırma verileri yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-580">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="d1d78-581">Yapılandırma verilerinde bir null değer tutulamıyor ve yapılandırma anahtarları bir dizide bir veya daha fazla dizinlerini atladığında null değerli bir girişi bir bağımlı nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="d1d78-581">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="d1d78-582">Eksik yapılandırma öğesi için dizin &num;bağlama önce 3 sağlanabilir `ArrayExample` örneği üretir yapılandırma doğru anahtar-değer çifti herhangi bir yapılandırma sağlayıcısı tarafından.</span><span class="sxs-lookup"><span data-stu-id="d1d78-582">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="d1d78-583">Ek bir JSON yapılandırma sağlayıcısı eksik anahtar-değer çifti ile örneği varsa `ArrayExample.Entries` yapılandırmanın tamamı dizisi ile eşleşen:</span><span class="sxs-lookup"><span data-stu-id="d1d78-583">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="d1d78-584">*missing_value.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-584">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="d1d78-585">İçinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="d1d78-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="d1d78-586">Tabloda belirtilen anahtar-değer çifti yapılandırma yüklenir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-586">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="d1d78-587">Anahtar</span><span class="sxs-lookup"><span data-stu-id="d1d78-587">Key</span></span>             | <span data-ttu-id="d1d78-588">Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d1d78-589">dizi: girişler: 3</span><span class="sxs-lookup"><span data-stu-id="d1d78-589">array:entries:3</span></span> | <span data-ttu-id="d1d78-590">Değeri3</span><span class="sxs-lookup"><span data-stu-id="d1d78-590">value3</span></span> |

<span data-ttu-id="d1d78-591">Varsa `ArrayExample` sınıf örneği bağlı dizin için giriş JSON yapılandırma sağlayıcısı içerir sonra &num;3 `ArrayExample.Entries` dizi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="d1d78-591">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="d1d78-592">`ArrayExample.Entries` Dizin</span><span class="sxs-lookup"><span data-stu-id="d1d78-592">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d1d78-593">`ArrayExample.Entries` Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-593">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d1d78-594">0</span><span class="sxs-lookup"><span data-stu-id="d1d78-594">0</span></span>                            | <span data-ttu-id="d1d78-595">value0</span><span class="sxs-lookup"><span data-stu-id="d1d78-595">value0</span></span>                       |
| <span data-ttu-id="d1d78-596">1.</span><span class="sxs-lookup"><span data-stu-id="d1d78-596">1</span></span>                            | <span data-ttu-id="d1d78-597">Değer1</span><span class="sxs-lookup"><span data-stu-id="d1d78-597">value1</span></span>                       |
| <span data-ttu-id="d1d78-598">2</span><span class="sxs-lookup"><span data-stu-id="d1d78-598">2</span></span>                            | <span data-ttu-id="d1d78-599">Value2</span><span class="sxs-lookup"><span data-stu-id="d1d78-599">value2</span></span>                       |
| <span data-ttu-id="d1d78-600">3</span><span class="sxs-lookup"><span data-stu-id="d1d78-600">3</span></span>                            | <span data-ttu-id="d1d78-601">Değeri3</span><span class="sxs-lookup"><span data-stu-id="d1d78-601">value3</span></span>                       |
| <span data-ttu-id="d1d78-602">4</span><span class="sxs-lookup"><span data-stu-id="d1d78-602">4</span></span>                            | <span data-ttu-id="d1d78-603">Değer4</span><span class="sxs-lookup"><span data-stu-id="d1d78-603">value4</span></span>                       |
| <span data-ttu-id="d1d78-604">5</span><span class="sxs-lookup"><span data-stu-id="d1d78-604">5</span></span>                            | <span data-ttu-id="d1d78-605">Değeri5</span><span class="sxs-lookup"><span data-stu-id="d1d78-605">value5</span></span>                       |

<span data-ttu-id="d1d78-606">**JSON dizisi işleme**</span><span class="sxs-lookup"><span data-stu-id="d1d78-606">**JSON array processing**</span></span>

<span data-ttu-id="d1d78-607">Bir JSON dosyası bir dizi varsa, dizi öğeleri bölümünde sıfır tabanlı dizine sahip için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-607">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="d1d78-608">Aşağıdaki yapılandırma dosyasında `subsection` dizisi:</span><span class="sxs-lookup"><span data-stu-id="d1d78-608">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="d1d78-609">JSON yapılandırma sağlayıcısı, aşağıdaki anahtar-değer çiftlerine yapılandırma verilerini okur:</span><span class="sxs-lookup"><span data-stu-id="d1d78-609">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="d1d78-610">Anahtar</span><span class="sxs-lookup"><span data-stu-id="d1d78-610">Key</span></span>                     | <span data-ttu-id="d1d78-611">Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-611">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="d1d78-612">json_array:key</span><span class="sxs-lookup"><span data-stu-id="d1d78-612">json_array:key</span></span>          | <span data-ttu-id="d1d78-613">Değera</span><span class="sxs-lookup"><span data-stu-id="d1d78-613">valueA</span></span> |
| <span data-ttu-id="d1d78-614">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="d1d78-614">json_array:subsection:0</span></span> | <span data-ttu-id="d1d78-615">Değerb</span><span class="sxs-lookup"><span data-stu-id="d1d78-615">valueB</span></span> |
| <span data-ttu-id="d1d78-616">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="d1d78-616">json_array:subsection:1</span></span> | <span data-ttu-id="d1d78-617">valueC</span><span class="sxs-lookup"><span data-stu-id="d1d78-617">valueC</span></span> |
| <span data-ttu-id="d1d78-618">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="d1d78-618">json_array:subsection:2</span></span> | <span data-ttu-id="d1d78-619">Değerli</span><span class="sxs-lookup"><span data-stu-id="d1d78-619">valueD</span></span> |

<span data-ttu-id="d1d78-620">Örnek uygulamada, aşağıdaki POCO sınıfı yapılandırma anahtar-değer çiftleri bağlamak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d1d78-620">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="d1d78-621">Bağlama sonra `JsonArrayExample.Key` değerine `valueA`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-621">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="d1d78-622">Alt değerleri POCO dizi özelliğinde depolanır `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-622">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="d1d78-623">`JsonArrayExample.Subsection` Dizin</span><span class="sxs-lookup"><span data-stu-id="d1d78-623">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="d1d78-624">`JsonArrayExample.Subsection` Değer</span><span class="sxs-lookup"><span data-stu-id="d1d78-624">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="d1d78-625">0</span><span class="sxs-lookup"><span data-stu-id="d1d78-625">0</span></span>                                   | <span data-ttu-id="d1d78-626">Değerb</span><span class="sxs-lookup"><span data-stu-id="d1d78-626">valueB</span></span>                              |
| <span data-ttu-id="d1d78-627">1.</span><span class="sxs-lookup"><span data-stu-id="d1d78-627">1</span></span>                                   | <span data-ttu-id="d1d78-628">valueC</span><span class="sxs-lookup"><span data-stu-id="d1d78-628">valueC</span></span>                              |
| <span data-ttu-id="d1d78-629">2</span><span class="sxs-lookup"><span data-stu-id="d1d78-629">2</span></span>                                   | <span data-ttu-id="d1d78-630">Değerli</span><span class="sxs-lookup"><span data-stu-id="d1d78-630">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="d1d78-631">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d1d78-631">Custom configuration provider</span></span>

<span data-ttu-id="d1d78-632">Örnek uygulamayı yapılandırma anahtar-değer çiftleri kullanarak bir veritabanını okuyan bir temel yapılandırma sağlayıcısı oluşturma gösterilmektedir [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="d1d78-632">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="d1d78-633">Sağlayıcı, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="d1d78-633">The provider has the following characteristics:</span></span>

* <span data-ttu-id="d1d78-634">EF bellek içi veritabanına tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-634">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="d1d78-635">Bir bağlantı dizesi gerektiren bir veritabanı kullanmak için ikincil uygulama `ConfigurationBuilder` başka bir yapılandırma sağlayıcısı bağlantı dizesinden sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="d1d78-635">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="d1d78-636">Sağlayıcı bir veritabanı tablosu, başlangıç yapılandırmasını içine okur.</span><span class="sxs-lookup"><span data-stu-id="d1d78-636">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="d1d78-637">Sağlayıcı anahtarı başına temelinde veritabanını sorgulamak değil.</span><span class="sxs-lookup"><span data-stu-id="d1d78-637">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="d1d78-638">Uygulama başlatılır sahip olduktan sonra uygulamanın yapılandırma üzerinde hiçbir etkisi kadar veritabanını güncelleme yeniden üzerinde değişiklik uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-638">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="d1d78-639">Tanımlayan bir `EFConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık.</span><span class="sxs-lookup"><span data-stu-id="d1d78-639">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="d1d78-640">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-640">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="d1d78-641">Ekleme bir `EFConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="d1d78-641">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="d1d78-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="d1d78-643">Uygulayan bir sınıf oluşturma <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-643">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="d1d78-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="d1d78-645">Özel yapılandırma sağlayıcısını devralarak oluşturma <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-645">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="d1d78-646">Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır.</span><span class="sxs-lookup"><span data-stu-id="d1d78-646">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="d1d78-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="d1d78-648">Bir `AddEFConfiguration` genişletme yöntemi izin veren yapılandırması kaynağına ekleme bir `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-648">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="d1d78-649">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-649">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="d1d78-650">Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigurationProvider` içinde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d1d78-650">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="d1d78-651">Başlatma sırasında erişimi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d1d78-651">Access configuration during startup</span></span>

<span data-ttu-id="d1d78-652">Ekleme `IConfiguration` içine `Startup` oluşturucuya erişim yapılandırma değerlerini `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d1d78-652">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d1d78-653">Erişim yapılandırmaya `Startup.Configure`, ya da ekleme `IConfiguration` doğrudan yöntem veya oluşturucu örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="d1d78-653">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="d1d78-654">Başlangıç kullanışlı yöntemler kullanarak yapılandırma erişme ilişkin bir örnek için bkz [uygulama başlatma: Yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="d1d78-654">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="d1d78-655">Erişim yapılandırmasında bir Razor sayfaları sayfası veya MVC görünümü</span><span class="sxs-lookup"><span data-stu-id="d1d78-655">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="d1d78-656">Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](xref:Microsoft.Extensions.Configuration) ve ekleme <xref:Microsoft.Extensions.Configuration.IConfiguration> sayfası ya da görünümü.</span><span class="sxs-lookup"><span data-stu-id="d1d78-656">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="d1d78-657">Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="d1d78-657">In a Razor Pages page:</span></span>

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

<span data-ttu-id="d1d78-658">Bir MVC Görünümü'nde:</span><span class="sxs-lookup"><span data-stu-id="d1d78-658">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="d1d78-659">Dış bütünleştirilmiş koddan Yapılandırması Ekle</span><span class="sxs-lookup"><span data-stu-id="d1d78-659">Add configuration from an external assembly</span></span>

<span data-ttu-id="d1d78-660">Bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d1d78-660">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="d1d78-661">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d1d78-661">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1d78-662">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d1d78-662">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="d1d78-663">Microsoft yapılandırma hakkında ayrıntılı bir inceleme</span><span class="sxs-lookup"><span data-stu-id="d1d78-663">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
