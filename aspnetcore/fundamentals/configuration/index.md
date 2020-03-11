---
title: ASP.NET Core yapılandırma
author: guardrex
description: ASP.NET Core uygulamasını yapılandırmak için yapılandırma API 'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 3dcabae3f76d81e641057c346dbb9097c2da42c7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656333"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="b3fc4-103">ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3fc4-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="b3fc4-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="b3fc4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b3fc4-105">ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="b3fc4-106">Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="b3fc4-107">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="b3fc4-107">Azure Key Vault</span></span>
* <span data-ttu-id="b3fc4-108">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b3fc4-108">Azure App Configuration</span></span>
* <span data-ttu-id="b3fc4-109">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-109">Command-line arguments</span></span>
* <span data-ttu-id="b3fc4-110">Özel sağlayıcılar (yüklü veya oluşturulmuş)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="b3fc4-111">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="b3fc4-111">Directory files</span></span>
* <span data-ttu-id="b3fc4-112">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-112">Environment variables</span></span>
* <span data-ttu-id="b3fc4-113">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-113">In-memory .NET objects</span></span>
* <span data-ttu-id="b3fc4-114">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="b3fc4-114">Settings files</span></span>

<span data-ttu-id="b3fc4-115">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), Framework tarafından örtük olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="b3fc4-116">Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="b3fc4-117">*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="b3fc4-118">Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="b3fc4-119">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b3fc4-120">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3fc4-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="b3fc4-121">Uygulama yapılandırmasına karşı konak</span><span class="sxs-lookup"><span data-stu-id="b3fc4-121">Host versus app configuration</span></span>

<span data-ttu-id="b3fc4-122">Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="b3fc4-123">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b3fc4-124">Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="b3fc4-125">Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="b3fc4-126">Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="b3fc4-127">Diğer yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3fc4-127">Other configuration</span></span>

<span data-ttu-id="b3fc4-128">Bu konu yalnızca *uygulama yapılandırması*ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="b3fc4-129">ASP.NET Core uygulamalarını çalıştırmanın diğer yönleri, bu konuda kapsanmayan yapılandırma dosyaları kullanılarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="b3fc4-130">*Launch. json*/*launchsettings. JSON* , geliştirme ortamı için yapılandırma dosyalarını araçlar, açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="b3fc4-131"><xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="b3fc4-132">Dosyaların geliştirme senaryolarında ASP.NET Core Uygulamaları yapılandırmak için kullanıldığı belge kümesi boyunca.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="b3fc4-133">*Web. config* aşağıdaki konularda açıklanan bir sunucu yapılandırma dosyasıdır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="b3fc4-134">ASP.NET 'in önceki sürümlerinden uygulama yapılandırmasını geçirme hakkında daha fazla bilgi için bkz. <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="b3fc4-135">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3fc4-135">Default configuration</span></span>

<span data-ttu-id="b3fc4-136">ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="b3fc4-137">`CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="b3fc4-138">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="b3fc4-139">[Web konağını](xref:fundamentals/host/web-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [bu konunun ASP.NET Core 2,2 sürümüne](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="b3fc4-140">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="b3fc4-141">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `DOTNET_` (örneğin, `DOTNET_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="b3fc4-142">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`DOTNET_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="b3fc4-143">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="b3fc4-144">Web ana bilgisayar varsayılan yapılandırması oluşturuldu (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="b3fc4-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="b3fc4-145">Kestrel, Web sunucusu olarak kullanılır ve uygulamanın yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="b3fc4-146">Konak filtreleme ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="b3fc4-147">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` ortam değişkeni `true`olarak ayarlandıysa, Iletilen üstbilgiler ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="b3fc4-148">IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-148">Enable IIS integration.</span></span>
* <span data-ttu-id="b3fc4-149">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="b3fc4-150">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b3fc4-151">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b3fc4-152">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="b3fc4-153">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="b3fc4-154">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="b3fc4-155">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="b3fc4-155">Security</span></span>

<span data-ttu-id="b3fc4-156">Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="b3fc4-157">Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="b3fc4-158">Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="b3fc4-159">Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="b3fc4-160">Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="b3fc4-161"><xref:security/app-secrets> &ndash;, hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler Içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="b3fc4-162">Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="b3fc4-163">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="b3fc4-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="b3fc4-165">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="b3fc4-166">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-166">Hierarchical configuration data</span></span>

<span data-ttu-id="b3fc4-167">Yapılandırma API 'SI, hiyerarşik verileri yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini muhafaza ediyor.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="b3fc4-168">Aşağıdaki JSON dosyasında, iki bölümün yapılandırılmış hiyerarşisinde dört anahtar mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="b3fc4-169">Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="b3fc4-170">Bölüm ve anahtarlar, özgün yapıyı sürdürmek için iki nokta üst üste (`:`) kullanımıyla düzleştirilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="b3fc4-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-171">section0:key0</span></span>
* <span data-ttu-id="b3fc4-172">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-172">section0:key1</span></span>
* <span data-ttu-id="b3fc4-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-173">section1:key0</span></span>
* <span data-ttu-id="b3fc4-174">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-174">section1:key1</span></span>

<span data-ttu-id="b3fc4-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="b3fc4-176">Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="b3fc4-177">Kurallar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="b3fc4-178">Kaynaklar ve sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-178">Sources and providers</span></span>

<span data-ttu-id="b3fc4-179">Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="b3fc4-180">Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="b3fc4-181">Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="b3fc4-182"><xref:Microsoft.Extensions.Configuration.IConfiguration>, uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b3fc4-183"><xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfın yapılandırmasını elde etmek için bir Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> veya MVC <xref:Microsoft.AspNetCore.Mvc.Controller> eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="b3fc4-184">Aşağıdaki örneklerde `_config` alanı yapılandırma değerlerine erişmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="b3fc4-185">Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="b3fc4-186">Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-186">Keys</span></span>

<span data-ttu-id="b3fc4-187">Yapılandırma anahtarları aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="b3fc4-188">Anahtarlar büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-188">Keys are case-insensitive.</span></span> <span data-ttu-id="b3fc4-189">Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="b3fc4-190">Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="b3fc4-191">Hiyerarşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-191">Hierarchical keys</span></span>
  * <span data-ttu-id="b3fc4-192">Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="b3fc4-193">Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="b3fc4-194">Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="b3fc4-195">Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="b3fc4-196">Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tireleri bir iki nokta ile değiştirmek için kod yazın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="b3fc4-197"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b3fc4-198">Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="b3fc4-199">Değerler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-199">Values</span></span>

<span data-ttu-id="b3fc4-200">Yapılandırma değerleri aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="b3fc4-201">Değerler dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-201">Values are strings.</span></span>
* <span data-ttu-id="b3fc4-202">Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="b3fc4-203">Sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-203">Providers</span></span>

<span data-ttu-id="b3fc4-204">Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="b3fc4-205">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-205">Provider</span></span> | <span data-ttu-id="b3fc4-206">&hellip; yapılandırma sağlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="b3fc4-207">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="b3fc4-208">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="b3fc4-208">Azure Key Vault</span></span> |
| <span data-ttu-id="b3fc4-209">[Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="b3fc4-210">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b3fc4-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="b3fc4-211">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="b3fc4-212">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-212">Command-line parameters</span></span> |
| [<span data-ttu-id="b3fc4-213">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="b3fc4-214">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="b3fc4-214">Custom source</span></span> |
| [<span data-ttu-id="b3fc4-215">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="b3fc4-216">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-216">Environment variables</span></span> |
| [<span data-ttu-id="b3fc4-217">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="b3fc4-218">Dosyalar (ıNı, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="b3fc4-219">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="b3fc4-220">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="b3fc4-220">Directory files</span></span> |
| [<span data-ttu-id="b3fc4-221">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="b3fc4-222">Bellek içi Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-222">In-memory collections</span></span> |
| <span data-ttu-id="b3fc4-223">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="b3fc4-224">Kullanıcı profili dizinindeki dosya</span><span class="sxs-lookup"><span data-stu-id="b3fc4-224">File in the user profile directory</span></span> |

<span data-ttu-id="b3fc4-225">Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="b3fc4-226">Bu konu başlığı altında açıklanan yapılandırma sağlayıcıları, kodun onları düzenler sırasına göre değil alfabetik sırayla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="b3fc4-227">Koddaki yapılandırma sağlayıcılarını, uygulamanın gerektirdiği temel yapılandırma kaynakları için önceliklere uyacak şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="b3fc4-228">Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="b3fc4-229">Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="b3fc4-230">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-230">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="b3fc4-231">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (yalnızca geliştirme ortamı)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="b3fc4-232">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-232">Environment variables</span></span>
1. <span data-ttu-id="b3fc4-233">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-233">Command-line arguments</span></span>

<span data-ttu-id="b3fc4-234">Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="b3fc4-235">Yeni bir ana bilgisayar Oluşturucusu `CreateDefaultBuilder`ile başlatıldığında yukarıdaki sağlayıcılar dizisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-236">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="b3fc4-237">Konak oluşturucuyu ConfigureHostConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3fc4-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="b3fc4-238">Konak oluşturucuyu yapılandırmak için <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> çağırın ve yapılandırmayı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="b3fc4-239">`ConfigureHostConfiguration`, derleme sürecinde daha sonra kullanmak üzere <xref:Microsoft.Extensions.Hosting.IHostEnvironment> başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="b3fc4-240">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

## <a name="configureappconfiguration"></a><span data-ttu-id="b3fc4-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="b3fc4-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="b3fc4-242">`CreateDefaultBuilder`tarafından otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirlemek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="b3fc4-243">Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="b3fc4-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="b3fc4-244">Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için `AddCommandLine` son ' u çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="b3fc4-245">CreateDefaultBuilder tarafından eklenen sağlayıcıları kaldır</span><span class="sxs-lookup"><span data-stu-id="b3fc4-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="b3fc4-246">`CreateDefaultBuilder`tarafından eklenen sağlayıcıları kaldırmak için, önce [ılisteationbuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) üzerinde [clear](/dotnet/api/system.collections.generic.icollection-1.clear) öğesini çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="b3fc4-247">Uygulama başlatma sırasında yapılandırmayı kullan</span><span class="sxs-lookup"><span data-stu-id="b3fc4-247">Consume configuration during app startup</span></span>

<span data-ttu-id="b3fc4-248">`ConfigureAppConfiguration` içinde uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında `Startup.ConfigureServices`dahil olmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b3fc4-249">Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="b3fc4-250">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="b3fc4-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, çalışma zamanında komut satırı bağımsız değişkeni anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-252">Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> uzantısı yöntemi bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-253">`AddCommandLine`, `CreateDefaultBuilder(string [])` çağrıldığında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="b3fc4-254">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-255">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b3fc4-256">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="b3fc4-257">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="b3fc4-258">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-258">Environment variables.</span></span>

<span data-ttu-id="b3fc4-259">`CreateDefaultBuilder`, komut satırı yapılandırma sağlayıcısını en son ekler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="b3fc4-260">Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="b3fc4-261">`CreateDefaultBuilder` ana bilgisayar oluşturulduğunda davranır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="b3fc4-262">Bu nedenle, `CreateDefaultBuilder` tarafından etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="b3fc4-263">ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` `CreateDefaultBuilder`tarafından zaten çağırılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-264">Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için, `ConfigureAppConfiguration` 'de uygulamanın ek sağlayıcılarını çağırın ve `AddCommandLine` son ' u çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="b3fc4-265">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-265">**Example**</span></span>

<span data-ttu-id="b3fc4-266">Örnek uygulama, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="b3fc4-267">Projenin dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="b3fc4-268">`dotnet run` komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="b3fc4-269">Uygulama çalıştıktan sonra, `http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b3fc4-270">Çıktının `dotnet run`için belirtilen yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="b3fc4-271">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-271">Arguments</span></span>

<span data-ttu-id="b3fc4-272">Değer bir eşittir işareti (`=`) izlemelidir veya değer bir boşluk izleyen anahtarın bir ön eki (`--` veya `/`) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="b3fc4-273">Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="b3fc4-274">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="b3fc4-274">Key prefix</span></span>               | <span data-ttu-id="b3fc4-275">Örnek</span><span class="sxs-lookup"><span data-stu-id="b3fc4-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="b3fc4-276">Ön ek yok</span><span class="sxs-lookup"><span data-stu-id="b3fc4-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="b3fc4-277">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="b3fc4-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="b3fc4-279">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="b3fc4-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="b3fc4-281">Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="b3fc4-282">Örnek komutlar:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="b3fc4-283">Eşleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="b3fc4-283">Switch mappings</span></span>

<span data-ttu-id="b3fc4-284">Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="b3fc4-285">Bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>el ile yapılandırma oluştururken, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoduna geçiş değiştirme sözlüğü sağlar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="b3fc4-286">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="b3fc4-287">Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="b3fc4-288">Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="b3fc4-289">Anahtar eşlemeleri sözlük anahtarı kuralları:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="b3fc4-290">Anahtarlar tireyle (`-`) veya çift tireyle başlamalıdır (`--`).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="b3fc4-291">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="b3fc4-292">Anahtar eşlemeleri sözlüğü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="b3fc4-293">Aşağıdaki örnekte, iki anahtar eşlemesi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="b3fc4-294">Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne `AddCommandLine` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="b3fc4-295">Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="b3fc4-296">`CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`'e iletmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-297">Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="b3fc4-298">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra, aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="b3fc4-299">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-299">Key</span></span>       | <span data-ttu-id="b3fc4-300">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="b3fc4-301">Uygulama başlatılırken anahtar eşlenmiş anahtarlar kullanılıyorsa, yapılandırma sözlük tarafından sağlanan anahtardaki yapılandırma değerini alır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="b3fc4-302">Önceki komutu çalıştırdıktan sonra, yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="b3fc4-303">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-303">Key</span></span>               | <span data-ttu-id="b3fc4-304">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="b3fc4-305">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="b3fc4-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-307">Ortam değişkenleri yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="b3fc4-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceği ortam değişkenlerini ayarlamaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="b3fc4-309">Daha fazla bilgi için bkz. [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="b3fc4-310">`AddEnvironmentVariables`, [genel ana bilgisayarla](xref:fundamentals/host/generic-host) yeni bir konak Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `DOTNET_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="b3fc4-311">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-312">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b3fc4-313">Önek olmadan `AddEnvironmentVariables` çağırarak, ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="b3fc4-314">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="b3fc4-315">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="b3fc4-316">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-316">Command-line arguments.</span></span>

<span data-ttu-id="b3fc4-317">Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="b3fc4-318">Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="b3fc4-319">Ek ortam değişkenlerinden uygulama yapılandırması sağlamak için, uygulamanın `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle birlikte `AddEnvironmentVariables` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="b3fc4-320">Diğer sağlayıcılardan gelen değerleri geçersiz kılmak için verilen öneke sahip ortam değişkenlerine izin vermek üzere en son `AddEnvironmentVariables` çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="b3fc4-321">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-321">**Example**</span></span>

<span data-ttu-id="b3fc4-322">Örnek uygulama, `AddEnvironmentVariables`bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="b3fc4-323">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-323">Run the sample app.</span></span> <span data-ttu-id="b3fc4-324">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b3fc4-325">Çıktının `ENVIRONMENT`ortam değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="b3fc4-326">Değer, uygulamanın çalıştığı ortamı yansıtır, genellikle yerel olarak çalışırken `Development`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="b3fc4-327">Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="b3fc4-328">Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="b3fc4-329">Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek için, *Pages/Index. cshtml. cs* ' deki `FilteredConfiguration` aşağıdaki gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="b3fc4-330">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-330">Prefixes</span></span>

<span data-ttu-id="b3fc4-331">Uygulamanın yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağlanırken filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="b3fc4-332">Örneğin, önek `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="b3fc4-333">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="b3fc4-334">Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="b3fc4-335">Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-336">**Bağlantı dizesi önekleri**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-336">**Connection string prefixes**</span></span>

<span data-ttu-id="b3fc4-337">Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="b3fc4-338">`AddEnvironmentVariables`için bir önek sağlanmazsa, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="b3fc4-339">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="b3fc4-339">Connection string prefix</span></span> | <span data-ttu-id="b3fc4-340">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="b3fc4-341">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="b3fc4-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="b3fc4-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="b3fc4-343">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="b3fc4-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b3fc4-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="b3fc4-345">Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="b3fc4-346">Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="b3fc4-347">Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="b3fc4-348">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-348">Environment variable key</span></span> | <span data-ttu-id="b3fc4-349">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-349">Converted configuration key</span></span> | <span data-ttu-id="b3fc4-350">Sağlayıcı yapılandırma girişi</span><span class="sxs-lookup"><span data-stu-id="b3fc4-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-351">Yapılandırma girişi oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-352">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="b3fc4-353">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-354">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="b3fc4-355">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-356">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="b3fc4-357">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="b3fc4-358">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-358">**Example**</span></span>

<span data-ttu-id="b3fc4-359">Sunucuda özel bir bağlantı dizesi ortam değişkeni oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="b3fc4-360">Ad &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="b3fc4-361">Değer &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="b3fc4-362">`IConfiguration` eklenen ve `_config`adlı bir alana atanmışsa, şu değeri okuyun:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="b3fc4-363">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-363">File Configuration Provider</span></span>

<span data-ttu-id="b3fc4-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="b3fc4-365">Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="b3fc4-366">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="b3fc4-367">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="b3fc4-368">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="b3fc4-369">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-369">INI Configuration Provider</span></span>

<span data-ttu-id="b3fc4-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-371">INI dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-372">İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="b3fc4-373">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-374">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-374">Whether the file is optional.</span></span>
* <span data-ttu-id="b3fc4-375">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b3fc4-376">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b3fc4-377">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="b3fc4-378">Bir ıNı yapılandırma dosyasına genel bir örnek:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-378">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="b3fc4-379">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-380">section0:key0</span></span>
* <span data-ttu-id="b3fc4-381">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-381">section0:key1</span></span>
* <span data-ttu-id="b3fc4-382">Section1: alt bölüm: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-382">section1:subsection:key</span></span>
* <span data-ttu-id="b3fc4-383">section2: subsection0: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-383">section2:subsection0:key</span></span>
* <span data-ttu-id="b3fc4-384">section2: subsection1: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="b3fc4-385">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-385">JSON Configuration Provider</span></span>

<span data-ttu-id="b3fc4-386"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="b3fc4-387">JSON dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-388">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-389">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-389">Whether the file is optional.</span></span>
* <span data-ttu-id="b3fc4-390">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b3fc4-391">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b3fc4-392">`CreateDefaultBuilder`ile yeni bir ana bilgisayar Oluşturucu başlatıldığında `AddJsonFile` otomatik olarak iki kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-393">Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="b3fc4-394">*appSettings. json* &ndash; bu dosya ilk kez okundu.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="b3fc4-395">Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="b3fc4-396">*appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="b3fc4-397">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-398">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b3fc4-399">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-399">Environment variables.</span></span>
* <span data-ttu-id="b3fc4-400">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="b3fc4-401">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-401">Command-line arguments.</span></span>

<span data-ttu-id="b3fc4-402">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="b3fc4-403">Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="b3fc4-404">Ana bilgisayarı derlerken, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtecek `ConfigureAppConfiguration` çağrısı yapın *. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="b3fc4-405">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-405">**Example**</span></span>

<span data-ttu-id="b3fc4-406">Örnek uygulama, `AddJsonFile`iki çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="b3fc4-407">`AddJsonFile` ilk çağrısı *appSettings. JSON*' dan yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="b3fc4-408">`AddJsonFile` ikinci çağrısı, appSettings 'ten yapılandırma yükler *. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="b3fc4-409">*AppSettings için. Geliştirme. JSON* örnek uygulamada aşağıdaki dosya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="b3fc4-410">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-410">Run the sample app.</span></span> <span data-ttu-id="b3fc4-411">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b3fc4-412">Çıktı, uygulamanın ortamına göre yapılandırma için anahtar-değer çiftleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="b3fc4-413">Uygulama geliştirme ortamında çalıştırılırken anahtar `Logging:LogLevel:Default` günlük düzeyi `Debug`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="b3fc4-414">Örnek uygulamayı üretim ortamında yeniden çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="b3fc4-415">*Properties/launchSettings. JSON* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="b3fc4-416">`ConfigurationSample` profilinde, `ASPNETCORE_ENVIRONMENT` ortam değişkeninin değerini `Production`olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="b3fc4-417">Dosyayı kaydedin ve bir komut kabuğunda `dotnet run` uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="b3fc4-418">AppSettings içindeki ayarlar *. Development. JSON* artık *appSettings. JSON*içindeki ayarları geçersiz kılmaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="b3fc4-419">Anahtar `Logging:LogLevel:Default` için günlük düzeyi `Information`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="b3fc4-420">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-420">XML Configuration Provider</span></span>

<span data-ttu-id="b3fc4-421"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-422">XML dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-423">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-424">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-424">Whether the file is optional.</span></span>
* <span data-ttu-id="b3fc4-425">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b3fc4-426">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b3fc4-427">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="b3fc4-428">Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="b3fc4-429">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="b3fc4-430">XML yapılandırma dosyaları, yinelenen bölümler için farklı öğe adları kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-430">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="b3fc4-431">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-432">section0:key0</span></span>
* <span data-ttu-id="b3fc4-433">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-433">section0:key1</span></span>
* <span data-ttu-id="b3fc4-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-434">section1:key0</span></span>
* <span data-ttu-id="b3fc4-435">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-435">section1:key1</span></span>

<span data-ttu-id="b3fc4-436">Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="b3fc4-437">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-438">Bölüm: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-438">section:section0:key:key0</span></span>
* <span data-ttu-id="b3fc4-439">Bölüm: section0: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-439">section:section0:key:key1</span></span>
* <span data-ttu-id="b3fc4-440">Bölüm: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-440">section:section1:key:key0</span></span>
* <span data-ttu-id="b3fc4-441">Bölüm: Section1: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-441">section:section1:key:key1</span></span>

<span data-ttu-id="b3fc4-442">Öznitelikler, değerler sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="b3fc4-443">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-444">anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="b3fc4-444">key:attribute</span></span>
* <span data-ttu-id="b3fc4-445">Section: Key: özniteliği</span><span class="sxs-lookup"><span data-stu-id="b3fc4-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="b3fc4-446">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="b3fc4-447"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="b3fc4-448">Anahtar, dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-448">The key is the file name.</span></span> <span data-ttu-id="b3fc4-449">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-449">The value contains the file's contents.</span></span> <span data-ttu-id="b3fc4-450">Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="b3fc4-451">Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="b3fc4-452">Dosyaların `directoryPath` mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="b3fc4-453">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-454">Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="b3fc4-455">Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="b3fc4-456">Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="b3fc4-457">Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="b3fc4-458">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="b3fc4-459">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-459">Memory Configuration Provider</span></span>

<span data-ttu-id="b3fc4-460"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="b3fc4-461">Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-462">Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>`başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="b3fc4-463">Uygulamanın yapılandırmasını belirtmek için Konağı derlerken `ConfigureAppConfiguration` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b3fc4-464">Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="b3fc4-465">Sözlük, yapılandırmayı sağlamak için bir `AddInMemoryCollection` çağrısıyla kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="b3fc4-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="b3fc4-466">GetValue</span></span>

<span data-ttu-id="b3fc4-467">[Configurationciltçi. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen koleksiyon olmayan türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="b3fc4-468">Aşırı yükleme varsayılan bir değeri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-468">An overload accepts a default value.</span></span>

<span data-ttu-id="b3fc4-469">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-469">The following example:</span></span>

* <span data-ttu-id="b3fc4-470">Anahtar `NumberKey`, yapılandırmadan dize değerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="b3fc4-471">Yapılandırma anahtarlarında `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="b3fc4-472">Değeri bir `int`olarak türler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="b3fc4-473">Değeri, sayfanın kullanımı için `NumberConfig` özelliği içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="b3fc4-474">GetSection, GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="b3fc4-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="b3fc4-475">İzleyen örnekler için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="b3fc4-476">İki bölüm arasında dört anahtar bulunur ve bunlardan biri alt bölümleri çifti içerir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="b3fc4-477">Dosya yapılandırmaya okunduğu zaman yapılandırma değerlerini tutmak için aşağıdaki benzersiz hiyerarşik anahtarlar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="b3fc4-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-478">section0:key0</span></span>
* <span data-ttu-id="b3fc4-479">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-479">section0:key1</span></span>
* <span data-ttu-id="b3fc4-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-480">section1:key0</span></span>
* <span data-ttu-id="b3fc4-481">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-481">section1:key1</span></span>
* <span data-ttu-id="b3fc4-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="b3fc4-483">section2: subsection0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="b3fc4-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="b3fc4-485">section2: subsection1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="b3fc4-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="b3fc4-486">GetSection</span></span>

<span data-ttu-id="b3fc4-487">[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü ayıklar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="b3fc4-488">`section1`yalnızca anahtar-değer çiftlerini içeren bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> döndürmek için `GetSection` çağırın ve bölüm adını sağlayın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="b3fc4-489">`configSection` bir değer, yalnızca bir anahtar ve yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="b3fc4-490">Benzer şekilde, `section2:subsection0`anahtarlar için değerleri almak için, `GetSection` çağırın ve Bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="b3fc4-491">`GetSection` hiçbir şekilde `null`döndürmez.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="b3fc4-492">Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="b3fc4-493">`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="b3fc4-494">Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="b3fc4-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="b3fc4-495">GetChildren</span></span>

<span data-ttu-id="b3fc4-496">`section2` üzerinde [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı, şunları içeren bir `IEnumerable<IConfigurationSection>` edinir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="b3fc4-497">Var</span><span class="sxs-lookup"><span data-stu-id="b3fc4-497">Exists</span></span>

<span data-ttu-id="b3fc4-498">Bir yapılandırma bölümünün mevcut olup olmadığını anlamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) kullanın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="b3fc4-499">Örnek veriler verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölümü olmadığından `sectionExists` `false`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="b3fc4-500">Bir sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="b3fc4-500">Bind to a class</span></span>

<span data-ttu-id="b3fc4-501">Yapılandırma, *Seçenekler modelini*kullanarak ilgili ayarların gruplarını temsil eden sınıflara bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="b3fc4-502">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b3fc4-503">Yapılandırma değerleri dizeler olarak döndürülür, ancak <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> çağrısı [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesnelerinin oluşturulmasını mümkün yapar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="b3fc4-504">Ciltçi, değerleri, belirtilen türün tüm genel okuma/yazma özelliklerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="b3fc4-505">Alanlar bağlanmadı **.**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-505">Fields are **not** bound.</span></span>

<span data-ttu-id="b3fc4-506">Örnek uygulama bir `Starship` modeli içerir (*modeller/Başlangıçgönder. cs*):</span><span class="sxs-lookup"><span data-stu-id="b3fc4-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="b3fc4-507">*Starsevk. JSON* dosyasının `starship` bölümü, örnek uygulama yapılandırmayı yüklemek Için JSON yapılandırma sağlayıcısını kullandığında yapılandırmayı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="b3fc4-508">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="b3fc4-509">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-509">Key</span></span>                   | <span data-ttu-id="b3fc4-510">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="b3fc4-511">starsevk: ad</span><span class="sxs-lookup"><span data-stu-id="b3fc4-511">starship:name</span></span>         | <span data-ttu-id="b3fc4-512">USS kurumsal</span><span class="sxs-lookup"><span data-stu-id="b3fc4-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="b3fc4-513">starsevk: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-513">starship:registry</span></span>     | <span data-ttu-id="b3fc4-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="b3fc4-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="b3fc4-515">starsevk: sınıfı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-515">starship:class</span></span>        | <span data-ttu-id="b3fc4-516">Anaytion</span><span class="sxs-lookup"><span data-stu-id="b3fc4-516">Constitution</span></span>                                      |
| <span data-ttu-id="b3fc4-517">başlangıçgönder: Uzunluk</span><span class="sxs-lookup"><span data-stu-id="b3fc4-517">starship:length</span></span>       | <span data-ttu-id="b3fc4-518">304,8</span><span class="sxs-lookup"><span data-stu-id="b3fc4-518">304.8</span></span>                                             |
| <span data-ttu-id="b3fc4-519">starsevkiyat: Commissioned</span><span class="sxs-lookup"><span data-stu-id="b3fc4-519">starship:commissioned</span></span> | <span data-ttu-id="b3fc4-520">False</span><span class="sxs-lookup"><span data-stu-id="b3fc4-520">False</span></span>                                             |
| <span data-ttu-id="b3fc4-521">dır</span><span class="sxs-lookup"><span data-stu-id="b3fc4-521">trademark</span></span>             | <span data-ttu-id="b3fc4-522">Paramount resimleri Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="b3fc4-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="b3fc4-523">Örnek uygulama, `starship` anahtarıyla `GetSection` çağırır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="b3fc4-524">`starship` anahtar-değer çiftleri yalıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="b3fc4-525">`Bind` yöntemi, `Starship` sınıfının bir örneğinde geçen alt bölümde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="b3fc4-526">Örnek değerlerini bağladıktan sonra örnek, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="b3fc4-527">Bir nesne grafiğine bağlama</span><span class="sxs-lookup"><span data-stu-id="b3fc4-527">Bind to an object graph</span></span>

<span data-ttu-id="b3fc4-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, tüm POCO nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="b3fc4-529">Basit bir nesne bağlamakla birlikte yalnızca genel okuma/yazma özellikleri bağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="b3fc4-530">Örnek, nesne grafı `Metadata` ve `Actors` sınıfları (*modeller/TvShow. cs*) içeren bir `TvShow` modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="b3fc4-531">Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="b3fc4-532">Yapılandırma, `Bind` yöntemi ile `TvShow` nesne grafiğinin tamamına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="b3fc4-533">Bağlantılı örnek, işleme için bir özelliğe atandı:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="b3fc4-534">[Configurationciltçi.\<t > bağlamalar alın](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) ve belirtilen türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="b3fc4-535">`Get<T>`, `Bind`kullanmaktan daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="b3fc4-536">Aşağıdaki kod, ilişkili örneğin işleme için kullanılan özelliğe doğrudan atanmasını sağlayan önceki örnekle `Get<T>` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="b3fc4-537">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="b3fc4-537">Bind an array to a class</span></span>

<span data-ttu-id="b3fc4-538">*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*</span><span class="sxs-lookup"><span data-stu-id="b3fc4-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="b3fc4-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b3fc4-540">Sayısal anahtar segmentini (`:0:`, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir POCO sınıf dizisine dizi bağlama özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="b3fc4-541">Bağlama, kural tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-541">Binding is provided by convention.</span></span> <span data-ttu-id="b3fc4-542">Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="b3fc4-543">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-543">**In-memory array processing**</span></span>

<span data-ttu-id="b3fc4-544">Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="b3fc4-545">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-545">Key</span></span>             | <span data-ttu-id="b3fc4-546">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b3fc4-547">dizi: girdiler: 0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-547">array:entries:0</span></span> | <span data-ttu-id="b3fc4-548">value0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-548">value0</span></span> |
| <span data-ttu-id="b3fc4-549">dizi: girdiler: 1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-549">array:entries:1</span></span> | <span data-ttu-id="b3fc4-550">Value1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-550">value1</span></span> |
| <span data-ttu-id="b3fc4-551">dizi: girdiler: 2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-551">array:entries:2</span></span> | <span data-ttu-id="b3fc4-552">Value2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-552">value2</span></span> |
| <span data-ttu-id="b3fc4-553">dizi: girdiler: 4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-553">array:entries:4</span></span> | <span data-ttu-id="b3fc4-554">value4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-554">value4</span></span> |
| <span data-ttu-id="b3fc4-555">dizi: girdiler: 5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-555">array:entries:5</span></span> | <span data-ttu-id="b3fc4-556">value5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-556">value5</span></span> |

<span data-ttu-id="b3fc4-557">Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="b3fc4-558">Dizi, Dizin &num;3 için bir değer atlıyor.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="b3fc4-559">Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="b3fc4-560">Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="b3fc4-561">Yapılandırma verileri nesnesine bağlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="b3fc4-562">[Configurationciltçi. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="b3fc4-563">`ArrayExample`bir örneği olan bağlantılı nesne, yapılandırmadan dizi verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="b3fc4-564">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="b3fc4-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b3fc4-565">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b3fc4-566">0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-566">0</span></span>                            | <span data-ttu-id="b3fc4-567">value0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-567">value0</span></span>                       |
| <span data-ttu-id="b3fc4-568">1\.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-568">1</span></span>                            | <span data-ttu-id="b3fc4-569">Value1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-569">value1</span></span>                       |
| <span data-ttu-id="b3fc4-570">2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-570">2</span></span>                            | <span data-ttu-id="b3fc4-571">Value2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-571">value2</span></span>                       |
| <span data-ttu-id="b3fc4-572">3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-572">3</span></span>                            | <span data-ttu-id="b3fc4-573">value4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-573">value4</span></span>                       |
| <span data-ttu-id="b3fc4-574">4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-574">4</span></span>                            | <span data-ttu-id="b3fc4-575">value5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-575">value5</span></span>                       |

<span data-ttu-id="b3fc4-576">İlişkili nesnede &num;3 dizini, `array:4` yapılandırma anahtarı ve `value4`değeri için yapılandırma verilerini tutar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="b3fc4-577">Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="b3fc4-578">Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="b3fc4-579">Yapılandırmada doğru anahtar-değer çiftini üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce, &num;3 dizini için eksik yapılandırma öğesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="b3fc4-580">Örnek, eksik anahtar-değer çiftine sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, `ArrayExample.Entries` tam yapılandırma dizisiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="b3fc4-581">*missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="b3fc4-582">`ConfigureAppConfiguration` içinde:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="b3fc4-583">Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="b3fc4-584">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-584">Key</span></span>             | <span data-ttu-id="b3fc4-585">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b3fc4-586">dizi: girdiler: 3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-586">array:entries:3</span></span> | <span data-ttu-id="b3fc4-587">value3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-587">value3</span></span> |

<span data-ttu-id="b3fc4-588">`ArrayExample` sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ün girdisini içeriyorsa, `ArrayExample.Entries` dizisi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="b3fc4-589">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="b3fc4-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b3fc4-590">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b3fc4-591">0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-591">0</span></span>                            | <span data-ttu-id="b3fc4-592">value0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-592">value0</span></span>                       |
| <span data-ttu-id="b3fc4-593">1\.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-593">1</span></span>                            | <span data-ttu-id="b3fc4-594">Value1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-594">value1</span></span>                       |
| <span data-ttu-id="b3fc4-595">2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-595">2</span></span>                            | <span data-ttu-id="b3fc4-596">Value2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-596">value2</span></span>                       |
| <span data-ttu-id="b3fc4-597">3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-597">3</span></span>                            | <span data-ttu-id="b3fc4-598">value3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-598">value3</span></span>                       |
| <span data-ttu-id="b3fc4-599">4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-599">4</span></span>                            | <span data-ttu-id="b3fc4-600">value4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-600">value4</span></span>                       |
| <span data-ttu-id="b3fc4-601">5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-601">5</span></span>                            | <span data-ttu-id="b3fc4-602">value5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-602">value5</span></span>                       |

<span data-ttu-id="b3fc4-603">**JSON dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-603">**JSON array processing**</span></span>

<span data-ttu-id="b3fc4-604">JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="b3fc4-605">Aşağıdaki yapılandırma dosyasında, `subsection` bir dizidir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="b3fc4-606">JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="b3fc4-607">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-607">Key</span></span>                     | <span data-ttu-id="b3fc4-608">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="b3fc4-609">json_array: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-609">json_array:key</span></span>          | <span data-ttu-id="b3fc4-610">değer EA</span><span class="sxs-lookup"><span data-stu-id="b3fc4-610">valueA</span></span> |
| <span data-ttu-id="b3fc4-611">json_array: alt bölüm: 0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-611">json_array:subsection:0</span></span> | <span data-ttu-id="b3fc4-612">valueB</span><span class="sxs-lookup"><span data-stu-id="b3fc4-612">valueB</span></span> |
| <span data-ttu-id="b3fc4-613">json_array: alt bölüm: 1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-613">json_array:subsection:1</span></span> | <span data-ttu-id="b3fc4-614">değer EC</span><span class="sxs-lookup"><span data-stu-id="b3fc4-614">valueC</span></span> |
| <span data-ttu-id="b3fc4-615">json_array: alt bölüm: 2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-615">json_array:subsection:2</span></span> | <span data-ttu-id="b3fc4-616">Değerler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-616">valueD</span></span> |

<span data-ttu-id="b3fc4-617">Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="b3fc4-618">Bağlamadan sonra, `JsonArrayExample.Key` `valueA`değerini tutar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="b3fc4-619">Alt bölüm değerleri, `Subsection`POCO dizisi özelliğinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="b3fc4-620">`JsonArrayExample.Subsection` dizini</span><span class="sxs-lookup"><span data-stu-id="b3fc4-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="b3fc4-621">`JsonArrayExample.Subsection` değeri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="b3fc4-622">0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-622">0</span></span>                                   | <span data-ttu-id="b3fc4-623">valueB</span><span class="sxs-lookup"><span data-stu-id="b3fc4-623">valueB</span></span>                              |
| <span data-ttu-id="b3fc4-624">1\.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-624">1</span></span>                                   | <span data-ttu-id="b3fc4-625">değer EC</span><span class="sxs-lookup"><span data-stu-id="b3fc4-625">valueC</span></span>                              |
| <span data-ttu-id="b3fc4-626">2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-626">2</span></span>                                   | <span data-ttu-id="b3fc4-627">Değerler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="b3fc4-628">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-628">Custom configuration provider</span></span>

<span data-ttu-id="b3fc4-629">Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="b3fc4-630">Sağlayıcı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="b3fc4-631">EF bellek içi veritabanı, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="b3fc4-632">Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="b3fc4-633">Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="b3fc4-634">Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="b3fc4-635">Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="b3fc4-636">Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="b3fc4-637">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="b3fc4-638">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="b3fc4-639">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="b3fc4-640"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="b3fc4-641">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="b3fc4-642"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="b3fc4-643">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="b3fc4-644">[Yapılandırma anahtarları büyük/küçük harfe duyarsız](#keys)olduğundan, veritabanını başlatmak için kullanılan sözlük, büyük/küçük harf duyarsız karşılaştırıcı ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)) ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="b3fc4-645">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="b3fc4-646">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="b3fc4-647">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="b3fc4-648">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="b3fc4-649">Başlangıç sırasında yapılandırmaya erişim</span><span class="sxs-lookup"><span data-stu-id="b3fc4-649">Access configuration during startup</span></span>

<span data-ttu-id="b3fc4-650">`Startup.ConfigureServices`yapılandırma değerlerine erişmek için `Startup` oluşturucusuna `IConfiguration` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b3fc4-651">`Startup.Configure`yapılandırmaya erişmek için `IConfiguration` doğrudan yönteme ekleyin ya da oluşturucuyu kullanarak örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="b3fc4-652">Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. [uygulama başlatma: kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="b3fc4-653">Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b3fc4-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="b3fc4-654">Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfa ya da görünüme <xref:Microsoft.Extensions.Configuration.IConfiguration> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="b3fc4-655">Razor Pages sayfasında:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-655">In a Razor Pages page:</span></span>

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

<span data-ttu-id="b3fc4-656">MVC görünümünde:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-656">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="b3fc4-657">Bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="b3fc4-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="b3fc4-658"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="b3fc4-659">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b3fc4-660">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b3fc4-661">ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="b3fc4-662">Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="b3fc4-663">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="b3fc4-663">Azure Key Vault</span></span>
* <span data-ttu-id="b3fc4-664">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b3fc4-664">Azure App Configuration</span></span>
* <span data-ttu-id="b3fc4-665">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-665">Command-line arguments</span></span>
* <span data-ttu-id="b3fc4-666">Özel sağlayıcılar (yüklü veya oluşturulmuş)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="b3fc4-667">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="b3fc4-667">Directory files</span></span>
* <span data-ttu-id="b3fc4-668">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-668">Environment variables</span></span>
* <span data-ttu-id="b3fc4-669">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-669">In-memory .NET objects</span></span>
* <span data-ttu-id="b3fc4-670">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="b3fc4-670">Settings files</span></span>

<span data-ttu-id="b3fc4-671">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b3fc4-672">Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="b3fc4-673">*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="b3fc4-674">Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="b3fc4-675">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b3fc4-676">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3fc4-676">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="b3fc4-677">Uygulama yapılandırmasına karşı konak</span><span class="sxs-lookup"><span data-stu-id="b3fc4-677">Host versus app configuration</span></span>

<span data-ttu-id="b3fc4-678">Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="b3fc4-679">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b3fc4-680">Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="b3fc4-681">Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="b3fc4-682">Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="b3fc4-683">Diğer yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3fc4-683">Other configuration</span></span>

<span data-ttu-id="b3fc4-684">Bu konu yalnızca *uygulama yapılandırması*ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="b3fc4-685">ASP.NET Core uygulamalarını çalıştırmanın diğer yönleri, bu konuda kapsanmayan yapılandırma dosyaları kullanılarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="b3fc4-686">*Launch. json*/*launchsettings. JSON* , geliştirme ortamı için yapılandırma dosyalarını araçlar, açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="b3fc4-687"><xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="b3fc4-688">Dosyaların geliştirme senaryolarında ASP.NET Core Uygulamaları yapılandırmak için kullanıldığı belge kümesi boyunca.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="b3fc4-689">*Web. config* aşağıdaki konularda açıklanan bir sunucu yapılandırma dosyasıdır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="b3fc4-690">ASP.NET 'in önceki sürümlerinden uygulama yapılandırmasını geçirme hakkında daha fazla bilgi için bkz. <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="b3fc4-691">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3fc4-691">Default configuration</span></span>

<span data-ttu-id="b3fc4-692">ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="b3fc4-693">`CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="b3fc4-694">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="b3fc4-695">[Genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [Bu konunun en son sürümüne](xref:fundamentals/configuration/index)bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="b3fc4-696">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="b3fc4-697">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="b3fc4-698">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`ASPNETCORE_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="b3fc4-699">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="b3fc4-700">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="b3fc4-701">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b3fc4-702">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b3fc4-703">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="b3fc4-704">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="b3fc4-705">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="b3fc4-706">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="b3fc4-706">Security</span></span>

<span data-ttu-id="b3fc4-707">Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="b3fc4-708">Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="b3fc4-709">Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="b3fc4-710">Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="b3fc4-711">Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="b3fc4-712"><xref:security/app-secrets> &ndash;, hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler Içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="b3fc4-713">Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="b3fc4-714">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="b3fc4-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="b3fc4-716">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="b3fc4-717">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-717">Hierarchical configuration data</span></span>

<span data-ttu-id="b3fc4-718">Yapılandırma API 'SI, hiyerarşik verileri yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini muhafaza ediyor.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="b3fc4-719">Aşağıdaki JSON dosyasında, iki bölümün yapılandırılmış hiyerarşisinde dört anahtar mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="b3fc4-720">Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="b3fc4-721">Bölüm ve anahtarlar, özgün yapıyı sürdürmek için iki nokta üst üste (`:`) kullanımıyla düzleştirilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="b3fc4-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-722">section0:key0</span></span>
* <span data-ttu-id="b3fc4-723">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-723">section0:key1</span></span>
* <span data-ttu-id="b3fc4-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-724">section1:key0</span></span>
* <span data-ttu-id="b3fc4-725">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-725">section1:key1</span></span>

<span data-ttu-id="b3fc4-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="b3fc4-727">Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="b3fc4-728">Kurallar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="b3fc4-729">Kaynaklar ve sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-729">Sources and providers</span></span>

<span data-ttu-id="b3fc4-730">Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="b3fc4-731">Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="b3fc4-732">Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="b3fc4-733"><xref:Microsoft.Extensions.Configuration.IConfiguration>, uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b3fc4-734"><xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfın yapılandırmasını elde etmek için bir Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> veya MVC <xref:Microsoft.AspNetCore.Mvc.Controller> eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="b3fc4-735">Aşağıdaki örneklerde `_config` alanı yapılandırma değerlerine erişmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="b3fc4-736">Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="b3fc4-737">Anahtarlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-737">Keys</span></span>

<span data-ttu-id="b3fc4-738">Yapılandırma anahtarları aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="b3fc4-739">Anahtarlar büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-739">Keys are case-insensitive.</span></span> <span data-ttu-id="b3fc4-740">Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="b3fc4-741">Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="b3fc4-742">Hiyerarşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-742">Hierarchical keys</span></span>
  * <span data-ttu-id="b3fc4-743">Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="b3fc4-744">Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="b3fc4-745">Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="b3fc4-746">Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="b3fc4-747">Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tireleri bir iki nokta ile değiştirmek için kod yazın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="b3fc4-748"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b3fc4-749">Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="b3fc4-750">Değerler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-750">Values</span></span>

<span data-ttu-id="b3fc4-751">Yapılandırma değerleri aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="b3fc4-752">Değerler dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-752">Values are strings.</span></span>
* <span data-ttu-id="b3fc4-753">Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="b3fc4-754">Sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-754">Providers</span></span>

<span data-ttu-id="b3fc4-755">Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="b3fc4-756">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-756">Provider</span></span> | <span data-ttu-id="b3fc4-757">&hellip; yapılandırma sağlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="b3fc4-758">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="b3fc4-759">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="b3fc4-759">Azure Key Vault</span></span> |
| <span data-ttu-id="b3fc4-760">[Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="b3fc4-761">Azure Uygulama Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b3fc4-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="b3fc4-762">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="b3fc4-763">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-763">Command-line parameters</span></span> |
| [<span data-ttu-id="b3fc4-764">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="b3fc4-765">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="b3fc4-765">Custom source</span></span> |
| [<span data-ttu-id="b3fc4-766">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="b3fc4-767">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-767">Environment variables</span></span> |
| [<span data-ttu-id="b3fc4-768">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="b3fc4-769">Dosyalar (ıNı, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="b3fc4-770">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="b3fc4-771">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="b3fc4-771">Directory files</span></span> |
| [<span data-ttu-id="b3fc4-772">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="b3fc4-773">Bellek içi Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-773">In-memory collections</span></span> |
| <span data-ttu-id="b3fc4-774">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="b3fc4-775">Kullanıcı profili dizinindeki dosya</span><span class="sxs-lookup"><span data-stu-id="b3fc4-775">File in the user profile directory</span></span> |

<span data-ttu-id="b3fc4-776">Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="b3fc4-777">Bu konu başlığı altında açıklanan yapılandırma sağlayıcıları, kodun onları düzenler sırasına göre değil alfabetik sırayla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="b3fc4-778">Koddaki yapılandırma sağlayıcılarını, uygulamanın gerektirdiği temel yapılandırma kaynakları için önceliklere uyacak şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="b3fc4-779">Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="b3fc4-780">Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="b3fc4-781">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-781">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="b3fc4-782">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (yalnızca geliştirme ortamı)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="b3fc4-783">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-783">Environment variables</span></span>
1. <span data-ttu-id="b3fc4-784">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-784">Command-line arguments</span></span>

<span data-ttu-id="b3fc4-785">Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="b3fc4-786">Yeni bir ana bilgisayar Oluşturucusu `CreateDefaultBuilder`ile başlatıldığında yukarıdaki sağlayıcılar dizisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-787">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="b3fc4-788">Konak oluşturucuyu UseConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3fc4-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="b3fc4-789">Konak oluşturucuyu yapılandırmak için, yapılandırma ile konak Oluşturucu üzerinde <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="b3fc4-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="b3fc4-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="b3fc4-791">`CreateDefaultBuilder`tarafından otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirlemek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="b3fc4-792">Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="b3fc4-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="b3fc4-793">Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için `AddCommandLine` son ' u çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="b3fc4-794">CreateDefaultBuilder tarafından eklenen sağlayıcıları kaldır</span><span class="sxs-lookup"><span data-stu-id="b3fc4-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="b3fc4-795">`CreateDefaultBuilder`tarafından eklenen sağlayıcıları kaldırmak için, önce [ılisteationbuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) üzerinde [clear](/dotnet/api/system.collections.generic.icollection-1.clear) öğesini çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="b3fc4-796">Uygulama başlatma sırasında yapılandırmayı kullan</span><span class="sxs-lookup"><span data-stu-id="b3fc4-796">Consume configuration during app startup</span></span>

<span data-ttu-id="b3fc4-797">`ConfigureAppConfiguration` içinde uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında `Startup.ConfigureServices`dahil olmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b3fc4-798">Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="b3fc4-799">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="b3fc4-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, çalışma zamanında komut satırı bağımsız değişkeni anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-801">Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> uzantısı yöntemi bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-802">`AddCommandLine`, `CreateDefaultBuilder(string [])` çağrıldığında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="b3fc4-803">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-804">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b3fc4-805">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="b3fc4-806">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="b3fc4-807">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-807">Environment variables.</span></span>

<span data-ttu-id="b3fc4-808">`CreateDefaultBuilder`, komut satırı yapılandırma sağlayıcısını en son ekler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="b3fc4-809">Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="b3fc4-810">`CreateDefaultBuilder` ana bilgisayar oluşturulduğunda davranır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="b3fc4-811">Bu nedenle, `CreateDefaultBuilder` tarafından etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="b3fc4-812">ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` `CreateDefaultBuilder`tarafından zaten çağırılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-813">Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için, `ConfigureAppConfiguration` 'de uygulamanın ek sağlayıcılarını çağırın ve `AddCommandLine` son ' u çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="b3fc4-814">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-814">**Example**</span></span>

<span data-ttu-id="b3fc4-815">Örnek uygulama, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="b3fc4-816">Projenin dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="b3fc4-817">`dotnet run` komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="b3fc4-818">Uygulama çalıştıktan sonra, `http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b3fc4-819">Çıktının `dotnet run`için belirtilen yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="b3fc4-820">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-820">Arguments</span></span>

<span data-ttu-id="b3fc4-821">Değer bir eşittir işareti (`=`) izlemelidir veya değer bir boşluk izleyen anahtarın bir ön eki (`--` veya `/`) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="b3fc4-822">Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="b3fc4-823">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="b3fc4-823">Key prefix</span></span>               | <span data-ttu-id="b3fc4-824">Örnek</span><span class="sxs-lookup"><span data-stu-id="b3fc4-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="b3fc4-825">Ön ek yok</span><span class="sxs-lookup"><span data-stu-id="b3fc4-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="b3fc4-826">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="b3fc4-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="b3fc4-828">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="b3fc4-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="b3fc4-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="b3fc4-830">Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="b3fc4-831">Örnek komutlar:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="b3fc4-832">Eşleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="b3fc4-832">Switch mappings</span></span>

<span data-ttu-id="b3fc4-833">Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="b3fc4-834">Bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>el ile yapılandırma oluştururken, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoduna geçiş değiştirme sözlüğü sağlar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="b3fc4-835">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="b3fc4-836">Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="b3fc4-837">Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="b3fc4-838">Anahtar eşlemeleri sözlük anahtarı kuralları:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="b3fc4-839">Anahtarlar tireyle (`-`) veya çift tireyle başlamalıdır (`--`).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="b3fc4-840">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="b3fc4-841">Anahtar eşlemeleri sözlüğü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="b3fc4-842">Aşağıdaki örnekte, iki anahtar eşlemesi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="b3fc4-843">Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne `AddCommandLine` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="b3fc4-844">Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="b3fc4-845">`CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`'e iletmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-846">Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="b3fc4-847">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra, aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="b3fc4-848">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-848">Key</span></span>       | <span data-ttu-id="b3fc4-849">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="b3fc4-850">Uygulama başlatılırken anahtar eşlenmiş anahtarlar kullanılıyorsa, yapılandırma sözlük tarafından sağlanan anahtardaki yapılandırma değerini alır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="b3fc4-851">Önceki komutu çalıştırdıktan sonra, yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="b3fc4-852">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-852">Key</span></span>               | <span data-ttu-id="b3fc4-853">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="b3fc4-854">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="b3fc4-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-856">Ortam değişkenleri yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="b3fc4-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceği ortam değişkenlerini ayarlamaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="b3fc4-858">Daha fazla bilgi için bkz. [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="b3fc4-859">`AddEnvironmentVariables`, [Web ana](xref:fundamentals/host/web-host) bilgisayarıyla yeni bir ana bilgisayar Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `ASPNETCORE_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="b3fc4-860">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-861">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b3fc4-862">Önek olmadan `AddEnvironmentVariables` çağırarak, ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="b3fc4-863">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="b3fc4-864">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="b3fc4-865">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-865">Command-line arguments.</span></span>

<span data-ttu-id="b3fc4-866">Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="b3fc4-867">Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="b3fc4-868">Ek ortam değişkenlerinden uygulama yapılandırması sağlamak için, uygulamanın `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle birlikte `AddEnvironmentVariables` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="b3fc4-869">Diğer sağlayıcılardan gelen değerleri geçersiz kılmak için verilen öneke sahip ortam değişkenlerine izin vermek üzere en son `AddEnvironmentVariables` çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="b3fc4-870">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-870">**Example**</span></span>

<span data-ttu-id="b3fc4-871">Örnek uygulama, `AddEnvironmentVariables`bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="b3fc4-872">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-872">Run the sample app.</span></span> <span data-ttu-id="b3fc4-873">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b3fc4-874">Çıktının `ENVIRONMENT`ortam değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="b3fc4-875">Değer, uygulamanın çalıştığı ortamı yansıtır, genellikle yerel olarak çalışırken `Development`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="b3fc4-876">Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="b3fc4-877">Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="b3fc4-878">Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek için, *Pages/Index. cshtml. cs* ' deki `FilteredConfiguration` aşağıdaki gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="b3fc4-879">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-879">Prefixes</span></span>

<span data-ttu-id="b3fc4-880">Uygulamanın yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağlanırken filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="b3fc4-881">Örneğin, önek `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="b3fc4-882">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="b3fc4-883">Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="b3fc4-884">Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-885">**Bağlantı dizesi önekleri**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-885">**Connection string prefixes**</span></span>

<span data-ttu-id="b3fc4-886">Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="b3fc4-887">`AddEnvironmentVariables`için bir önek sağlanmazsa, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="b3fc4-888">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="b3fc4-888">Connection string prefix</span></span> | <span data-ttu-id="b3fc4-889">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="b3fc4-890">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="b3fc4-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="b3fc4-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="b3fc4-892">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="b3fc4-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b3fc4-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="b3fc4-894">Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="b3fc4-895">Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="b3fc4-896">Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="b3fc4-897">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-897">Environment variable key</span></span> | <span data-ttu-id="b3fc4-898">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-898">Converted configuration key</span></span> | <span data-ttu-id="b3fc4-899">Sağlayıcı yapılandırma girişi</span><span class="sxs-lookup"><span data-stu-id="b3fc4-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-900">Yapılandırma girişi oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-901">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="b3fc4-902">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-903">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="b3fc4-904">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="b3fc4-905">Anahtar: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="b3fc4-906">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="b3fc4-907">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-907">**Example**</span></span>

<span data-ttu-id="b3fc4-908">Sunucuda özel bir bağlantı dizesi ortam değişkeni oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="b3fc4-909">Ad &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="b3fc4-910">Değer &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="b3fc4-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="b3fc4-911">`IConfiguration` eklenen ve `_config`adlı bir alana atanmışsa, şu değeri okuyun:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="b3fc4-912">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-912">File Configuration Provider</span></span>

<span data-ttu-id="b3fc4-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="b3fc4-914">Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="b3fc4-915">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="b3fc4-916">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="b3fc4-917">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="b3fc4-918">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-918">INI Configuration Provider</span></span>

<span data-ttu-id="b3fc4-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-920">INI dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-921">İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="b3fc4-922">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-923">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-923">Whether the file is optional.</span></span>
* <span data-ttu-id="b3fc4-924">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b3fc4-925">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b3fc4-926">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="b3fc4-927">Bir ıNı yapılandırma dosyasına genel bir örnek:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-927">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="b3fc4-928">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-929">section0:key0</span></span>
* <span data-ttu-id="b3fc4-930">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-930">section0:key1</span></span>
* <span data-ttu-id="b3fc4-931">Section1: alt bölüm: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-931">section1:subsection:key</span></span>
* <span data-ttu-id="b3fc4-932">section2: subsection0: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-932">section2:subsection0:key</span></span>
* <span data-ttu-id="b3fc4-933">section2: subsection1: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="b3fc4-934">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-934">JSON Configuration Provider</span></span>

<span data-ttu-id="b3fc4-935"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="b3fc4-936">JSON dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-937">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-938">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-938">Whether the file is optional.</span></span>
* <span data-ttu-id="b3fc4-939">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b3fc4-940">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b3fc4-941">`CreateDefaultBuilder`ile yeni bir ana bilgisayar Oluşturucu başlatıldığında `AddJsonFile` otomatik olarak iki kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b3fc4-942">Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="b3fc4-943">*appSettings. json* &ndash; bu dosya ilk kez okundu.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="b3fc4-944">Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="b3fc4-945">*appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="b3fc4-946">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="b3fc4-947">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b3fc4-948">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-948">Environment variables.</span></span>
* <span data-ttu-id="b3fc4-949">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b3fc4-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="b3fc4-950">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-950">Command-line arguments.</span></span>

<span data-ttu-id="b3fc4-951">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="b3fc4-952">Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="b3fc4-953">Ana bilgisayarı derlerken, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtecek `ConfigureAppConfiguration` çağrısı yapın *. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="b3fc4-954">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-954">**Example**</span></span>

<span data-ttu-id="b3fc4-955">Örnek uygulama, `AddJsonFile`iki çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="b3fc4-956">`AddJsonFile` ilk çağrısı *appSettings. JSON*' dan yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="b3fc4-957">`AddJsonFile` ikinci çağrısı, appSettings 'ten yapılandırma yükler *. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="b3fc4-958">*AppSettings için. Geliştirme. JSON* örnek uygulamada aşağıdaki dosya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="b3fc4-959">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-959">Run the sample app.</span></span> <span data-ttu-id="b3fc4-960">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b3fc4-961">Çıktı, uygulamanın ortamına göre yapılandırma için anahtar-değer çiftleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="b3fc4-962">Uygulama geliştirme ortamında çalıştırılırken anahtar `Logging:LogLevel:Default` günlük düzeyi `Debug`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="b3fc4-963">Örnek uygulamayı üretim ortamında yeniden çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="b3fc4-964">*Properties/launchSettings. JSON* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="b3fc4-965">`ConfigurationSample` profilinde, `ASPNETCORE_ENVIRONMENT` ortam değişkeninin değerini `Production`olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="b3fc4-966">Dosyayı kaydedin ve bir komut kabuğunda `dotnet run` uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="b3fc4-967">AppSettings içindeki ayarlar *. Development. JSON* artık *appSettings. JSON*içindeki ayarları geçersiz kılmaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="b3fc4-968">Anahtar `Logging:LogLevel:Default` için günlük düzeyi `Warning`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="b3fc4-969">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-969">XML Configuration Provider</span></span>

<span data-ttu-id="b3fc4-970"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="b3fc4-971">XML dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-972">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-973">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-973">Whether the file is optional.</span></span>
* <span data-ttu-id="b3fc4-974">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b3fc4-975">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b3fc4-976">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="b3fc4-977">Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="b3fc4-978">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="b3fc4-979">XML yapılandırma dosyaları, yinelenen bölümler için farklı öğe adları kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-979">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="b3fc4-980">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-981">section0:key0</span></span>
* <span data-ttu-id="b3fc4-982">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-982">section0:key1</span></span>
* <span data-ttu-id="b3fc4-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-983">section1:key0</span></span>
* <span data-ttu-id="b3fc4-984">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-984">section1:key1</span></span>

<span data-ttu-id="b3fc4-985">Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="b3fc4-986">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-987">Bölüm: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-987">section:section0:key:key0</span></span>
* <span data-ttu-id="b3fc4-988">Bölüm: section0: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-988">section:section0:key:key1</span></span>
* <span data-ttu-id="b3fc4-989">Bölüm: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-989">section:section1:key:key0</span></span>
* <span data-ttu-id="b3fc4-990">Bölüm: Section1: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-990">section:section1:key:key1</span></span>

<span data-ttu-id="b3fc4-991">Öznitelikler, değerler sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="b3fc4-992">Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b3fc4-993">anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="b3fc4-993">key:attribute</span></span>
* <span data-ttu-id="b3fc4-994">Section: Key: özniteliği</span><span class="sxs-lookup"><span data-stu-id="b3fc4-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="b3fc4-995">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="b3fc4-996"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="b3fc4-997">Anahtar, dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-997">The key is the file name.</span></span> <span data-ttu-id="b3fc4-998">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-998">The value contains the file's contents.</span></span> <span data-ttu-id="b3fc4-999">Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="b3fc4-1000">Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="b3fc4-1001">Dosyaların `directoryPath` mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="b3fc4-1002">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="b3fc4-1003">Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="b3fc4-1004">Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="b3fc4-1005">Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="b3fc4-1006">Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="b3fc4-1007">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="b3fc4-1008">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="b3fc4-1009"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="b3fc4-1010">Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b3fc4-1011">Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>`başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="b3fc4-1012">Uygulamanın yapılandırmasını belirtmek için Konağı derlerken `ConfigureAppConfiguration` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b3fc4-1013">Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="b3fc4-1014">Sözlük, yapılandırmayı sağlamak için bir `AddInMemoryCollection` çağrısıyla kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="b3fc4-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1015">GetValue</span></span>

<span data-ttu-id="b3fc4-1016">[Configurationciltçi. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen koleksiyon olmayan türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="b3fc4-1017">Aşırı yükleme varsayılan bir değeri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="b3fc4-1018">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1018">The following example:</span></span>

* <span data-ttu-id="b3fc4-1019">Anahtar `NumberKey`, yapılandırmadan dize değerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="b3fc4-1020">Yapılandırma anahtarlarında `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="b3fc4-1021">Değeri bir `int`olarak türler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="b3fc4-1022">Değeri, sayfanın kullanımı için `NumberConfig` özelliği içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="b3fc4-1023">GetSection, GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="b3fc4-1024">İzleyen örnekler için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="b3fc4-1025">İki bölüm arasında dört anahtar bulunur ve bunlardan biri alt bölümleri çifti içerir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="b3fc4-1026">Dosya yapılandırmaya okunduğu zaman yapılandırma değerlerini tutmak için aşağıdaki benzersiz hiyerarşik anahtarlar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="b3fc4-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1027">section0:key0</span></span>
* <span data-ttu-id="b3fc4-1028">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1028">section0:key1</span></span>
* <span data-ttu-id="b3fc4-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1029">section1:key0</span></span>
* <span data-ttu-id="b3fc4-1030">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1030">section1:key1</span></span>
* <span data-ttu-id="b3fc4-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="b3fc4-1032">section2: subsection0: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="b3fc4-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="b3fc4-1034">section2: subsection1: KEY1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="b3fc4-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1035">GetSection</span></span>

<span data-ttu-id="b3fc4-1036">[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü ayıklar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="b3fc4-1037">`section1`yalnızca anahtar-değer çiftlerini içeren bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> döndürmek için `GetSection` çağırın ve bölüm adını sağlayın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="b3fc4-1038">`configSection` bir değer, yalnızca bir anahtar ve yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="b3fc4-1039">Benzer şekilde, `section2:subsection0`anahtarlar için değerleri almak için, `GetSection` çağırın ve Bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="b3fc4-1040">`GetSection` hiçbir şekilde `null`döndürmez.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="b3fc4-1041">Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="b3fc4-1042">`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="b3fc4-1043">Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="b3fc4-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1044">GetChildren</span></span>

<span data-ttu-id="b3fc4-1045">`section2` üzerinde [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı, şunları içeren bir `IEnumerable<IConfigurationSection>` edinir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="b3fc4-1046">Var</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1046">Exists</span></span>

<span data-ttu-id="b3fc4-1047">Bir yapılandırma bölümünün mevcut olup olmadığını anlamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) kullanın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="b3fc4-1048">Örnek veriler verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölümü olmadığından `sectionExists` `false`.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="b3fc4-1049">Bir sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1049">Bind to a class</span></span>

<span data-ttu-id="b3fc4-1050">Yapılandırma, *Seçenekler modelini*kullanarak ilgili ayarların gruplarını temsil eden sınıflara bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="b3fc4-1051">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b3fc4-1052">Yapılandırma değerleri dizeler olarak döndürülür, ancak <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> çağrısı [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesnelerinin oluşturulmasını mümkün yapar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="b3fc4-1053">Ciltçi, değerleri, belirtilen türün tüm genel okuma/yazma özelliklerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="b3fc4-1054">Alanlar bağlanmadı **.**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="b3fc4-1055">Örnek uygulama bir `Starship` modeli içerir (*modeller/Başlangıçgönder. cs*):</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1056">*Starsevk. JSON* dosyasının `starship` bölümü, örnek uygulama yapılandırmayı yüklemek Için JSON yapılandırma sağlayıcısını kullandığında yapılandırmayı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="b3fc4-1057">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="b3fc4-1058">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1058">Key</span></span>                   | <span data-ttu-id="b3fc4-1059">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="b3fc4-1060">starsevk: ad</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1060">starship:name</span></span>         | <span data-ttu-id="b3fc4-1061">USS kurumsal</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="b3fc4-1062">starsevk: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1062">starship:registry</span></span>     | <span data-ttu-id="b3fc4-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="b3fc4-1064">starsevk: sınıfı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1064">starship:class</span></span>        | <span data-ttu-id="b3fc4-1065">Anaytion</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1065">Constitution</span></span>                                      |
| <span data-ttu-id="b3fc4-1066">başlangıçgönder: Uzunluk</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1066">starship:length</span></span>       | <span data-ttu-id="b3fc4-1067">304,8</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1067">304.8</span></span>                                             |
| <span data-ttu-id="b3fc4-1068">starsevkiyat: Commissioned</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1068">starship:commissioned</span></span> | <span data-ttu-id="b3fc4-1069">False</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1069">False</span></span>                                             |
| <span data-ttu-id="b3fc4-1070">dır</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1070">trademark</span></span>             | <span data-ttu-id="b3fc4-1071">Paramount resimleri Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="b3fc4-1072">Örnek uygulama, `starship` anahtarıyla `GetSection` çağırır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="b3fc4-1073">`starship` anahtar-değer çiftleri yalıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="b3fc4-1074">`Bind` yöntemi, `Starship` sınıfının bir örneğinde geçen alt bölümde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="b3fc4-1075">Örnek değerlerini bağladıktan sonra örnek, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="b3fc4-1076">Bir nesne grafiğine bağlama</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1076">Bind to an object graph</span></span>

<span data-ttu-id="b3fc4-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, tüm POCO nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="b3fc4-1078">Basit bir nesne bağlamakla birlikte yalnızca genel okuma/yazma özellikleri bağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="b3fc4-1079">Örnek, nesne grafı `Metadata` ve `Actors` sınıfları (*modeller/TvShow. cs*) içeren bir `TvShow` modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1080">Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="b3fc4-1081">Yapılandırma, `Bind` yöntemi ile `TvShow` nesne grafiğinin tamamına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="b3fc4-1082">Bağlantılı örnek, işleme için bir özelliğe atandı:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="b3fc4-1083">[Configurationciltçi.\<t > bağlamalar alın](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) ve belirtilen türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="b3fc4-1084">`Get<T>`, `Bind`kullanmaktan daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="b3fc4-1085">Aşağıdaki kod, ilişkili örneğin işleme için kullanılan özelliğe doğrudan atanmasını sağlayan önceki örnekle `Get<T>` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="b3fc4-1086">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1086">Bind an array to a class</span></span>

<span data-ttu-id="b3fc4-1087">*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="b3fc4-1088"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b3fc4-1089">Sayısal anahtar segmentini (`:0:`, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir POCO sınıf dizisine dizi bağlama özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="b3fc4-1090">Bağlama, kural tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1090">Binding is provided by convention.</span></span> <span data-ttu-id="b3fc4-1091">Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="b3fc4-1092">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1092">**In-memory array processing**</span></span>

<span data-ttu-id="b3fc4-1093">Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="b3fc4-1094">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1094">Key</span></span>             | <span data-ttu-id="b3fc4-1095">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b3fc4-1096">dizi: girdiler: 0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1096">array:entries:0</span></span> | <span data-ttu-id="b3fc4-1097">value0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1097">value0</span></span> |
| <span data-ttu-id="b3fc4-1098">dizi: girdiler: 1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1098">array:entries:1</span></span> | <span data-ttu-id="b3fc4-1099">Value1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1099">value1</span></span> |
| <span data-ttu-id="b3fc4-1100">dizi: girdiler: 2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1100">array:entries:2</span></span> | <span data-ttu-id="b3fc4-1101">Value2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1101">value2</span></span> |
| <span data-ttu-id="b3fc4-1102">dizi: girdiler: 4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1102">array:entries:4</span></span> | <span data-ttu-id="b3fc4-1103">value4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1103">value4</span></span> |
| <span data-ttu-id="b3fc4-1104">dizi: girdiler: 5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1104">array:entries:5</span></span> | <span data-ttu-id="b3fc4-1105">value5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1105">value5</span></span> |

<span data-ttu-id="b3fc4-1106">Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="b3fc4-1107">Dizi, Dizin &num;3 için bir değer atlıyor.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="b3fc4-1108">Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="b3fc4-1109">Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1110">Yapılandırma verileri nesnesine bağlanır:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="b3fc4-1111">[Configurationciltçi. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="b3fc4-1112">`ArrayExample`bir örneği olan bağlantılı nesne, yapılandırmadan dizi verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="b3fc4-1113">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b3fc4-1114">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b3fc4-1115">0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1115">0</span></span>                            | <span data-ttu-id="b3fc4-1116">value0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1116">value0</span></span>                       |
| <span data-ttu-id="b3fc4-1117">1\.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1117">1</span></span>                            | <span data-ttu-id="b3fc4-1118">Value1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1118">value1</span></span>                       |
| <span data-ttu-id="b3fc4-1119">2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1119">2</span></span>                            | <span data-ttu-id="b3fc4-1120">Value2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1120">value2</span></span>                       |
| <span data-ttu-id="b3fc4-1121">3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1121">3</span></span>                            | <span data-ttu-id="b3fc4-1122">value4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1122">value4</span></span>                       |
| <span data-ttu-id="b3fc4-1123">4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1123">4</span></span>                            | <span data-ttu-id="b3fc4-1124">value5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1124">value5</span></span>                       |

<span data-ttu-id="b3fc4-1125">İlişkili nesnede &num;3 dizini, `array:4` yapılandırma anahtarı ve `value4`değeri için yapılandırma verilerini tutar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="b3fc4-1126">Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="b3fc4-1127">Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="b3fc4-1128">Yapılandırmada doğru anahtar-değer çiftini üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce, &num;3 dizini için eksik yapılandırma öğesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="b3fc4-1129">Örnek, eksik anahtar-değer çiftine sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, `ArrayExample.Entries` tam yapılandırma dizisiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="b3fc4-1130">*missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="b3fc4-1131">`ConfigureAppConfiguration` içinde:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="b3fc4-1132">Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="b3fc4-1133">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1133">Key</span></span>             | <span data-ttu-id="b3fc4-1134">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b3fc4-1135">dizi: girdiler: 3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1135">array:entries:3</span></span> | <span data-ttu-id="b3fc4-1136">value3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1136">value3</span></span> |

<span data-ttu-id="b3fc4-1137">`ArrayExample` sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ün girdisini içeriyorsa, `ArrayExample.Entries` dizisi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="b3fc4-1138">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b3fc4-1139">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b3fc4-1140">0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1140">0</span></span>                            | <span data-ttu-id="b3fc4-1141">value0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1141">value0</span></span>                       |
| <span data-ttu-id="b3fc4-1142">1\.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1142">1</span></span>                            | <span data-ttu-id="b3fc4-1143">Value1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1143">value1</span></span>                       |
| <span data-ttu-id="b3fc4-1144">2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1144">2</span></span>                            | <span data-ttu-id="b3fc4-1145">Value2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1145">value2</span></span>                       |
| <span data-ttu-id="b3fc4-1146">3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1146">3</span></span>                            | <span data-ttu-id="b3fc4-1147">value3</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1147">value3</span></span>                       |
| <span data-ttu-id="b3fc4-1148">4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1148">4</span></span>                            | <span data-ttu-id="b3fc4-1149">value4</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1149">value4</span></span>                       |
| <span data-ttu-id="b3fc4-1150">5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1150">5</span></span>                            | <span data-ttu-id="b3fc4-1151">value5</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1151">value5</span></span>                       |

<span data-ttu-id="b3fc4-1152">**JSON dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1152">**JSON array processing**</span></span>

<span data-ttu-id="b3fc4-1153">JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="b3fc4-1154">Aşağıdaki yapılandırma dosyasında, `subsection` bir dizidir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="b3fc4-1155">JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="b3fc4-1156">Anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1156">Key</span></span>                     | <span data-ttu-id="b3fc4-1157">Value</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="b3fc4-1158">json_array: anahtar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1158">json_array:key</span></span>          | <span data-ttu-id="b3fc4-1159">değer EA</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1159">valueA</span></span> |
| <span data-ttu-id="b3fc4-1160">json_array: alt bölüm: 0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1160">json_array:subsection:0</span></span> | <span data-ttu-id="b3fc4-1161">valueB</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1161">valueB</span></span> |
| <span data-ttu-id="b3fc4-1162">json_array: alt bölüm: 1</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1162">json_array:subsection:1</span></span> | <span data-ttu-id="b3fc4-1163">değer EC</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1163">valueC</span></span> |
| <span data-ttu-id="b3fc4-1164">json_array: alt bölüm: 2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1164">json_array:subsection:2</span></span> | <span data-ttu-id="b3fc4-1165">Değerler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1165">valueD</span></span> |

<span data-ttu-id="b3fc4-1166">Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1167">Bağlamadan sonra, `JsonArrayExample.Key` `valueA`değerini tutar.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="b3fc4-1168">Alt bölüm değerleri, `Subsection`POCO dizisi özelliğinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="b3fc4-1169">`JsonArrayExample.Subsection` dizini</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="b3fc4-1170">`JsonArrayExample.Subsection` değeri</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="b3fc4-1171">0</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1171">0</span></span>                                   | <span data-ttu-id="b3fc4-1172">valueB</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1172">valueB</span></span>                              |
| <span data-ttu-id="b3fc4-1173">1\.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1173">1</span></span>                                   | <span data-ttu-id="b3fc4-1174">değer EC</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1174">valueC</span></span>                              |
| <span data-ttu-id="b3fc4-1175">2</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1175">2</span></span>                                   | <span data-ttu-id="b3fc4-1176">Değerler</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="b3fc4-1177">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1177">Custom configuration provider</span></span>

<span data-ttu-id="b3fc4-1178">Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="b3fc4-1179">Sağlayıcı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="b3fc4-1180">EF bellek içi veritabanı, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="b3fc4-1181">Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="b3fc4-1182">Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="b3fc4-1183">Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="b3fc4-1184">Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="b3fc4-1185">Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="b3fc4-1186">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1187">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="b3fc4-1188">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1189"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="b3fc4-1190">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1191"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="b3fc4-1192">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="b3fc4-1193">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1194">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="b3fc4-1195">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="b3fc4-1196">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="b3fc4-1197">Başlangıç sırasında yapılandırmaya erişim</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1197">Access configuration during startup</span></span>

<span data-ttu-id="b3fc4-1198">`Startup.ConfigureServices`yapılandırma değerlerine erişmek için `Startup` oluşturucusuna `IConfiguration` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b3fc4-1199">`Startup.Configure`yapılandırmaya erişmek için `IConfiguration` doğrudan yönteme ekleyin ya da oluşturucuyu kullanarak örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="b3fc4-1200">Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. [uygulama başlatma: kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="b3fc4-1201">Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="b3fc4-1202">Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfa ya da görünüme <xref:Microsoft.Extensions.Configuration.IConfiguration> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="b3fc4-1203">Razor Pages sayfasında:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1203">In a Razor Pages page:</span></span>

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

<span data-ttu-id="b3fc4-1204">MVC görünümünde:</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1204">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="b3fc4-1205">Bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="b3fc4-1206"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="b3fc4-1207">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b3fc4-1208">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b3fc4-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
