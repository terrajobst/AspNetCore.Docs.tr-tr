---
title: ASP.NET Core yapılandırma
author: guardrex
description: ASP.NET Core uygulamasını yapılandırmak için yapılandırma API 'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 263f9f7c4c800a74b745fd636196e1e135afc91b
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73033910"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="a45ca-103">ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a45ca-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="a45ca-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="a45ca-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a45ca-105">ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="a45ca-106">Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="a45ca-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a45ca-107">Azure Key Vault</span></span>
* <span data-ttu-id="a45ca-108">Azure Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a45ca-108">Azure App Configuration</span></span>
* <span data-ttu-id="a45ca-109">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a45ca-109">Command-line arguments</span></span>
* <span data-ttu-id="a45ca-110">Özel sağlayıcılar (yüklü veya oluşturulmuş)</span><span class="sxs-lookup"><span data-stu-id="a45ca-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="a45ca-111">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="a45ca-111">Directory files</span></span>
* <span data-ttu-id="a45ca-112">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a45ca-112">Environment variables</span></span>
* <span data-ttu-id="a45ca-113">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="a45ca-113">In-memory .NET objects</span></span>
* <span data-ttu-id="a45ca-114">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="a45ca-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a45ca-115">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), Framework tarafından örtük olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a45ca-116">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="a45ca-117">Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="a45ca-118">*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="a45ca-119">Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="a45ca-120">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="a45ca-121">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a45ca-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="a45ca-122">Uygulama yapılandırmasına karşı konak</span><span class="sxs-lookup"><span data-stu-id="a45ca-122">Host versus app configuration</span></span>

<span data-ttu-id="a45ca-123">Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="a45ca-124">Ana bilgisayar, uygulama başlatma ve ömür yönetiminden sorumludur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a45ca-125">Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="a45ca-126">Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="a45ca-127">Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="a45ca-128">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a45ca-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a45ca-129">ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="a45ca-130">`CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="a45ca-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="a45ca-131">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="a45ca-132">[Web konağını](xref:fundamentals/host/web-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [bu konunun ASP.NET Core 2,2 sürümüne](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="a45ca-133">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="a45ca-134">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `DOTNET_` (örneğin, `DOTNET_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="a45ca-135">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`DOTNET_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="a45ca-136">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="a45ca-137">Web ana bilgisayar varsayılan yapılandırması oluşturuldu (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="a45ca-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="a45ca-138">Kestrel, Web sunucusu olarak kullanılır ve uygulamanın yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="a45ca-139">Konak filtreleme ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="a45ca-140">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` ortam değişkeni `true`olarak ayarlandıysa, Iletilen üstbilgiler ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="a45ca-141">IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-141">Enable IIS integration.</span></span>
* <span data-ttu-id="a45ca-142">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="a45ca-143">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="a45ca-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a45ca-144">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="a45ca-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a45ca-145">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="a45ca-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a45ca-146">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="a45ca-147">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a45ca-148">ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="a45ca-149">`CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="a45ca-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="a45ca-150">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="a45ca-151">[Genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [Bu konunun en son sürümüne](xref:fundamentals/configuration/index)bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="a45ca-152">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="a45ca-153">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="a45ca-154">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`ASPNETCORE_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="a45ca-155">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="a45ca-156">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="a45ca-157">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="a45ca-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a45ca-158">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="a45ca-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a45ca-159">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="a45ca-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a45ca-160">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="a45ca-161">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="a45ca-162">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="a45ca-162">Security</span></span>

<span data-ttu-id="a45ca-163">Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="a45ca-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="a45ca-164">Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="a45ca-165">Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="a45ca-166">Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="a45ca-167">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="a45ca-168"><xref:security/app-secrets> &ndash;, hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler Içerir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="a45ca-169">Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="a45ca-170">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="a45ca-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="a45ca-172">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="a45ca-173">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="a45ca-173">Hierarchical configuration data</span></span>

<span data-ttu-id="a45ca-174">Yapılandırma API 'SI, hiyerarşik verileri yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini muhafaza ediyor.</span><span class="sxs-lookup"><span data-stu-id="a45ca-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="a45ca-175">Aşağıdaki JSON dosyasında, iki bölümün yapılandırılmış hiyerarşisinde dört anahtar mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="a45ca-176">Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="a45ca-177">Bölüm ve anahtarlar, özgün yapıyı sürdürmek için iki nokta üst üste (`:`) kullanımıyla düzleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="a45ca-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-178">section0:key0</span></span>
* <span data-ttu-id="a45ca-179">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-179">section0:key1</span></span>
* <span data-ttu-id="a45ca-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-180">section1:key0</span></span>
* <span data-ttu-id="a45ca-181">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-181">section1:key1</span></span>

<span data-ttu-id="a45ca-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="a45ca-183">Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="a45ca-184">Kurallar</span><span class="sxs-lookup"><span data-stu-id="a45ca-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="a45ca-185">Kaynaklar ve sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="a45ca-185">Sources and providers</span></span>

<span data-ttu-id="a45ca-186">Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="a45ca-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="a45ca-187">Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="a45ca-188">Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.</span><span class="sxs-lookup"><span data-stu-id="a45ca-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="a45ca-189"><xref:Microsoft.Extensions.Configuration.IConfiguration>, uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a45ca-190"><xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfın yapılandırmasını elde etmek için bir Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="a45ca-191">Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="a45ca-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="a45ca-192">Belirlenmesine</span><span class="sxs-lookup"><span data-stu-id="a45ca-192">Keys</span></span>

<span data-ttu-id="a45ca-193">Yapılandırma anahtarları aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="a45ca-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="a45ca-194">Anahtarlar büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-194">Keys are case-insensitive.</span></span> <span data-ttu-id="a45ca-195">Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="a45ca-196">Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="a45ca-197">Hiyerarşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="a45ca-197">Hierarchical keys</span></span>
  * <span data-ttu-id="a45ca-198">Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="a45ca-199">Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="a45ca-200">Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="a45ca-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="a45ca-201">Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="a45ca-202">Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tirelerin yerini iki nokta ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="a45ca-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="a45ca-204">Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="a45ca-205">Değerler</span><span class="sxs-lookup"><span data-stu-id="a45ca-205">Values</span></span>

<span data-ttu-id="a45ca-206">Yapılandırma değerleri aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="a45ca-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="a45ca-207">Değerler dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-207">Values are strings.</span></span>
* <span data-ttu-id="a45ca-208">Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="a45ca-209">sağlayıcılarla</span><span class="sxs-lookup"><span data-stu-id="a45ca-209">Providers</span></span>

<span data-ttu-id="a45ca-210">Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="a45ca-211">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a45ca-211">Provider</span></span> | <span data-ttu-id="a45ca-212">&hellip; yapılandırma sağlar</span><span class="sxs-lookup"><span data-stu-id="a45ca-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="a45ca-213">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="a45ca-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="a45ca-214">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a45ca-214">Azure Key Vault</span></span> |
| <span data-ttu-id="a45ca-215">[Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri)</span><span class="sxs-lookup"><span data-stu-id="a45ca-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="a45ca-216">Azure Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a45ca-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="a45ca-217">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="a45ca-218">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="a45ca-218">Command-line parameters</span></span> |
| [<span data-ttu-id="a45ca-219">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="a45ca-220">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="a45ca-220">Custom source</span></span> |
| [<span data-ttu-id="a45ca-221">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="a45ca-222">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a45ca-222">Environment variables</span></span> |
| [<span data-ttu-id="a45ca-223">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="a45ca-224">Dosyalar (ıNı, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="a45ca-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="a45ca-225">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="a45ca-226">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="a45ca-226">Directory files</span></span> |
| [<span data-ttu-id="a45ca-227">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="a45ca-228">Bellek içi Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="a45ca-228">In-memory collections</span></span> |
| <span data-ttu-id="a45ca-229">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="a45ca-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="a45ca-230">Kullanıcı profili dizinindeki dosya</span><span class="sxs-lookup"><span data-stu-id="a45ca-230">File in the user profile directory</span></span> |

<span data-ttu-id="a45ca-231">Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="a45ca-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="a45ca-232">Bu konuda açıklanan yapılandırma sağlayıcıları, kodunuzun onları düzenleyebilir sırada değil alfabetik sırayla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="a45ca-233">Kodunuzda yapılandırma sağlayıcılarını, temeldeki yapılandırma kaynakları için önceliklerinize uyacak şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="a45ca-234">Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="a45ca-235">Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="a45ca-236">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a45ca-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="a45ca-237">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (yalnızca geliştirme ortamı)</span><span class="sxs-lookup"><span data-stu-id="a45ca-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="a45ca-238">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a45ca-238">Environment variables</span></span>
1. <span data-ttu-id="a45ca-239">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a45ca-239">Command-line arguments</span></span>

<span data-ttu-id="a45ca-240">Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="a45ca-241">Önceki sağlayıcı dizisi, `CreateDefaultBuilder` ile yeni bir konak Oluşturucu başlattığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a45ca-242">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="a45ca-243">Konak oluşturucuyu ConfigureHostConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a45ca-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="a45ca-244">Konak oluşturucuyu yapılandırmak için <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> çağırın ve yapılandırmayı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="a45ca-245">`ConfigureHostConfiguration`, derleme sürecinde daha sonra kullanmak üzere <xref:Microsoft.Extensions.Hosting.IHostEnvironment> başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="a45ca-246">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="a45ca-247">Konak oluşturucuyu UseConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a45ca-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="a45ca-248">Konak oluşturucuyu yapılandırmak için, yapılandırma ile konak Oluşturucu üzerinde <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

::: moniker-end

## <a name="configureappconfiguration"></a><span data-ttu-id="a45ca-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="a45ca-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="a45ca-250">`CreateDefaultBuilder`tarafından otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirlemek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="a45ca-251">Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="a45ca-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="a45ca-252">Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için `AddCommandLine` son ' u çağırın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="a45ca-253">Uygulama başlatma sırasında yapılandırmayı kullan</span><span class="sxs-lookup"><span data-stu-id="a45ca-253">Consume configuration during app startup</span></span>

<span data-ttu-id="a45ca-254">`ConfigureAppConfiguration` içinde uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında `Startup.ConfigureServices`dahil olmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-254">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a45ca-255">Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-255">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="a45ca-256">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-256">Command-line Configuration Provider</span></span>

<span data-ttu-id="a45ca-257"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, çalışma zamanında komut satırı bağımsız değişkeni anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-257">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="a45ca-258">Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> uzantısı yöntemi bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> örneğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-258">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a45ca-259">`AddCommandLine`, `CreateDefaultBuilder(string [])` çağrıldığında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-259">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="a45ca-260">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-260">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="a45ca-261">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-261">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a45ca-262">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="a45ca-262">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="a45ca-263">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="a45ca-263">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="a45ca-264">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-264">Environment variables.</span></span>

<span data-ttu-id="a45ca-265">`CreateDefaultBuilder`, komut satırı yapılandırma sağlayıcısını en son ekler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-265">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="a45ca-266">Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.</span><span class="sxs-lookup"><span data-stu-id="a45ca-266">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="a45ca-267">`CreateDefaultBuilder` ana bilgisayar oluşturulduğunda davranır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-267">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="a45ca-268">Bu nedenle, `CreateDefaultBuilder` tarafından etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-268">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="a45ca-269">ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` `CreateDefaultBuilder` tarafından zaten çağırılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-269">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a45ca-270">Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için, `ConfigureAppConfiguration` 'de uygulamanın ek sağlayıcılarını çağırın ve `AddCommandLine` son ' u çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-270">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="a45ca-271">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="a45ca-271">**Example**</span></span>

<span data-ttu-id="a45ca-272">Örnek uygulama, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-272">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="a45ca-273">Projenin dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="a45ca-274">`dotnet run` komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="a45ca-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="a45ca-275">Uygulama çalıştıktan sonra, `http://localhost:5000` konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a45ca-276">Çıktının `dotnet run` için belirtilen yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="a45ca-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="a45ca-277">Arguments</span></span>

<span data-ttu-id="a45ca-278">Değer bir eşittir işareti (`=`) izlemelidir veya değer bir boşluk izleyen anahtarın bir ön eki (`--` veya `/`) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="a45ca-279">Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="a45ca-279">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="a45ca-280">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="a45ca-280">Key prefix</span></span>               | <span data-ttu-id="a45ca-281">Örnek</span><span class="sxs-lookup"><span data-stu-id="a45ca-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="a45ca-282">Ön ek yok</span><span class="sxs-lookup"><span data-stu-id="a45ca-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="a45ca-283">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="a45ca-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="a45ca-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="a45ca-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="a45ca-285">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="a45ca-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="a45ca-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="a45ca-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="a45ca-287">Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="a45ca-288">Örnek komutlar:</span><span class="sxs-lookup"><span data-stu-id="a45ca-288">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="a45ca-289">Eşleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="a45ca-289">Switch mappings</span></span>

<span data-ttu-id="a45ca-290">Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="a45ca-291">Yapılandırmayı bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ile el ile oluşturduğunuzda, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemine anahtar değiştirme sözlüğü sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a45ca-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="a45ca-292">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="a45ca-293">Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="a45ca-294">Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="a45ca-295">Anahtar eşlemeleri sözlük anahtarı kuralları:</span><span class="sxs-lookup"><span data-stu-id="a45ca-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="a45ca-296">Anahtarlar tireyle (`-`) veya çift tireyle başlamalıdır (`--`).</span><span class="sxs-lookup"><span data-stu-id="a45ca-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="a45ca-297">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="a45ca-298">Anahtar eşlemeleri sözlüğü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a45ca-298">Create a switch mappings dictionary.</span></span> <span data-ttu-id="a45ca-299">Aşağıdaki örnekte, iki anahtar eşlemesi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-299">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="a45ca-300">Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne `AddCommandLine` çağırın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-300">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="a45ca-301">Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-301">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="a45ca-302">`CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`'e iletmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-302">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a45ca-303">Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="a45ca-303">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="a45ca-304">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra, aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-304">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="a45ca-305">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-305">Key</span></span>       | <span data-ttu-id="a45ca-306">Değer</span><span class="sxs-lookup"><span data-stu-id="a45ca-306">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="a45ca-307">Uygulama başlatılırken anahtar eşlenmiş anahtarlar kullanılıyorsa, yapılandırma sözlük tarafından sağlanan anahtardaki yapılandırma değerini alır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-307">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="a45ca-308">Önceki komutu çalıştırdıktan sonra, yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-308">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="a45ca-309">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-309">Key</span></span>               | <span data-ttu-id="a45ca-310">Değer</span><span class="sxs-lookup"><span data-stu-id="a45ca-310">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="a45ca-311">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-311">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="a45ca-312"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-312">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="a45ca-313">Ortam değişkenleri yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-313">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="a45ca-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceğiniz ortam değişkenlerini ayarlamanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="a45ca-315">Daha fazla bilgi için bkz. [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="a45ca-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a45ca-316">`AddEnvironmentVariables`, [genel ana bilgisayarla](xref:fundamentals/host/generic-host) yeni bir konak Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `DOTNET_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-316">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="a45ca-317">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-317">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a45ca-318">`AddEnvironmentVariables`, [Web ana](xref:fundamentals/host/web-host) bilgisayarıyla yeni bir ana bilgisayar Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `ASPNETCORE_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="a45ca-319">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="a45ca-320">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-320">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a45ca-321">Önek olmadan `AddEnvironmentVariables` çağırarak, ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="a45ca-321">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="a45ca-322">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="a45ca-322">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="a45ca-323">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="a45ca-323">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="a45ca-324">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-324">Command-line arguments.</span></span>

<span data-ttu-id="a45ca-325">Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-325">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="a45ca-326">Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-326">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="a45ca-327">Ek ortam değişkenlerinden uygulama yapılandırması sağlamanız gerekiyorsa, uygulamanın `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle birlikte `AddEnvironmentVariables` çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-327">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

<span data-ttu-id="a45ca-328">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="a45ca-328">**Example**</span></span>

<span data-ttu-id="a45ca-329">Örnek uygulama, `AddEnvironmentVariables` bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-329">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="a45ca-330">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-330">Run the sample app.</span></span> <span data-ttu-id="a45ca-331">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-331">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a45ca-332">Çıktının `ENVIRONMENT` ortam değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-332">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="a45ca-333">Değer, uygulamanın çalıştığı ortamı yansıtır, genellikle yerel olarak çalışırken `Development`.</span><span class="sxs-lookup"><span data-stu-id="a45ca-333">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="a45ca-334">Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-334">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="a45ca-335">Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-335">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="a45ca-336">Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek istiyorsanız, *Pages/Index. cshtml. cs* ' deki `FilteredConfiguration` aşağıdaki gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a45ca-336">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="a45ca-337">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="a45ca-337">Prefixes</span></span>

<span data-ttu-id="a45ca-338">Uygulama yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağladığınız zaman filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-338">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="a45ca-339">Örneğin, önek `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-339">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="a45ca-340">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-340">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="a45ca-341">Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-341">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="a45ca-342">Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-342">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="a45ca-343">**Bağlantı dizesi önekleri**</span><span class="sxs-lookup"><span data-stu-id="a45ca-343">**Connection string prefixes**</span></span>

<span data-ttu-id="a45ca-344">Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-344">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="a45ca-345">`AddEnvironmentVariables`için bir önek sağlanmazsa, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-345">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="a45ca-346">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="a45ca-346">Connection string prefix</span></span> | <span data-ttu-id="a45ca-347">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a45ca-347">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="a45ca-348">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="a45ca-348">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="a45ca-349">MySQL</span><span class="sxs-lookup"><span data-stu-id="a45ca-349">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="a45ca-350">Azure SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="a45ca-350">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="a45ca-351">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a45ca-351">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="a45ca-352">Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:</span><span class="sxs-lookup"><span data-stu-id="a45ca-352">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="a45ca-353">Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-353">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="a45ca-354">Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).</span><span class="sxs-lookup"><span data-stu-id="a45ca-354">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="a45ca-355">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="a45ca-355">Environment variable key</span></span> | <span data-ttu-id="a45ca-356">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="a45ca-356">Converted configuration key</span></span> | <span data-ttu-id="a45ca-357">Sağlayıcı yapılandırma girişi</span><span class="sxs-lookup"><span data-stu-id="a45ca-357">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a45ca-358">Yapılandırma girişi oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="a45ca-358">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a45ca-359">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a45ca-359">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a45ca-360">Değer: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="a45ca-360">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a45ca-361">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a45ca-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a45ca-362">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a45ca-362">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a45ca-363">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a45ca-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a45ca-364">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a45ca-364">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="a45ca-365">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-365">File Configuration Provider</span></span>

<span data-ttu-id="a45ca-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="a45ca-367">Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-367">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="a45ca-368">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-368">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="a45ca-369">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-369">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="a45ca-370">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-370">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="a45ca-371">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-371">INI Configuration Provider</span></span>

<span data-ttu-id="a45ca-372"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-372">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="a45ca-373">INI dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> örneğinde <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-373">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a45ca-374">İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-374">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="a45ca-375">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="a45ca-375">Overloads permit specifying:</span></span>

* <span data-ttu-id="a45ca-376">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="a45ca-376">Whether the file is optional.</span></span>
* <span data-ttu-id="a45ca-377">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-377">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a45ca-378">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-378">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a45ca-379">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-379">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="a45ca-380">Bir ıNı yapılandırma dosyasına genel bir örnek:</span><span class="sxs-lookup"><span data-stu-id="a45ca-380">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="a45ca-381">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="a45ca-381">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a45ca-382">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-382">section0:key0</span></span>
* <span data-ttu-id="a45ca-383">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-383">section0:key1</span></span>
* <span data-ttu-id="a45ca-384">Section1: alt bölüm: anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-384">section1:subsection:key</span></span>
* <span data-ttu-id="a45ca-385">section2: subsection0: anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-385">section2:subsection0:key</span></span>
* <span data-ttu-id="a45ca-386">section2: subsection1: anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-386">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="a45ca-387">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-387">JSON Configuration Provider</span></span>

<span data-ttu-id="a45ca-388"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-388">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="a45ca-389">JSON dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-389">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a45ca-390">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="a45ca-390">Overloads permit specifying:</span></span>

* <span data-ttu-id="a45ca-391">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="a45ca-391">Whether the file is optional.</span></span>
* <span data-ttu-id="a45ca-392">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-392">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a45ca-393">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-393">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a45ca-394">`CreateDefaultBuilder` ile yeni bir ana bilgisayar Oluşturucu başlattığınızda `AddJsonFile` otomatik olarak iki kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-394">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a45ca-395">Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-395">The method is called to load configuration from:</span></span>

* <span data-ttu-id="a45ca-396">*appSettings. json* &ndash; bu dosya ilk kez okundu.</span><span class="sxs-lookup"><span data-stu-id="a45ca-396">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="a45ca-397">Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-397">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="a45ca-398">*appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-398">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="a45ca-399">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-399">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="a45ca-400">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-400">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a45ca-401">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-401">Environment variables.</span></span>
* <span data-ttu-id="a45ca-402">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="a45ca-402">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="a45ca-403">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a45ca-403">Command-line arguments.</span></span>

<span data-ttu-id="a45ca-404">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-404">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="a45ca-405">Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-405">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="a45ca-406">Ana bilgisayarı derlerken, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtecek `ConfigureAppConfiguration` çağrısı yapın *. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-406">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="a45ca-407">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="a45ca-407">**Example**</span></span>

<span data-ttu-id="a45ca-408">Örnek uygulama, `AddJsonFile` iki çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-408">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="a45ca-409">Yapılandırma *appSettings. JSON* ve appSettings 'ten yüklendi *. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="a45ca-409">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="a45ca-410">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-410">Run the sample app.</span></span> <span data-ttu-id="a45ca-411">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a45ca-412">Çıktının, ortama bağlı olarak tabloda gösterilen yapılandırma için anahtar-değer çiftleri içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-412">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="a45ca-413">Günlüğe kaydetme yapılandırma anahtarları hiyerarşik ayırıcı olarak iki nokta (`:`) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-413">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="a45ca-414">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-414">Key</span></span>                        | <span data-ttu-id="a45ca-415">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="a45ca-415">Development Value</span></span> | <span data-ttu-id="a45ca-416">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="a45ca-416">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="a45ca-417">Günlüğe kaydetme: LogLevel: System</span><span class="sxs-lookup"><span data-stu-id="a45ca-417">Logging:LogLevel:System</span></span>    | <span data-ttu-id="a45ca-418">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a45ca-418">Information</span></span>       | <span data-ttu-id="a45ca-419">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a45ca-419">Information</span></span>      |
| <span data-ttu-id="a45ca-420">Günlüğe kaydetme: LogLevel: Microsoft</span><span class="sxs-lookup"><span data-stu-id="a45ca-420">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="a45ca-421">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a45ca-421">Information</span></span>       | <span data-ttu-id="a45ca-422">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="a45ca-422">Information</span></span>      |
| <span data-ttu-id="a45ca-423">Günlüğe kaydetme: LogLevel: default</span><span class="sxs-lookup"><span data-stu-id="a45ca-423">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="a45ca-424">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a45ca-424">Debug</span></span>             | <span data-ttu-id="a45ca-425">Hata</span><span class="sxs-lookup"><span data-stu-id="a45ca-425">Error</span></span>            |
| <span data-ttu-id="a45ca-426">Allowedkonakları</span><span class="sxs-lookup"><span data-stu-id="a45ca-426">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="a45ca-427">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-427">XML Configuration Provider</span></span>

<span data-ttu-id="a45ca-428"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-428">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="a45ca-429">XML dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-429">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a45ca-430">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="a45ca-430">Overloads permit specifying:</span></span>

* <span data-ttu-id="a45ca-431">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="a45ca-431">Whether the file is optional.</span></span>
* <span data-ttu-id="a45ca-432">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-432">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a45ca-433">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-433">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a45ca-434">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-434">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="a45ca-435">Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-435">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="a45ca-436">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-436">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="a45ca-437">XML yapılandırma dosyaları, yinelenen bölümler için farklı öğe adları kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-437">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="a45ca-438">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="a45ca-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a45ca-439">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-439">section0:key0</span></span>
* <span data-ttu-id="a45ca-440">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-440">section0:key1</span></span>
* <span data-ttu-id="a45ca-441">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-441">section1:key0</span></span>
* <span data-ttu-id="a45ca-442">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-442">section1:key1</span></span>

<span data-ttu-id="a45ca-443">Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-443">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="a45ca-444">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="a45ca-444">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a45ca-445">Bölüm: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-445">section:section0:key:key0</span></span>
* <span data-ttu-id="a45ca-446">Bölüm: section0: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-446">section:section0:key:key1</span></span>
* <span data-ttu-id="a45ca-447">Bölüm: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-447">section:section1:key:key0</span></span>
* <span data-ttu-id="a45ca-448">Bölüm: Section1: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-448">section:section1:key:key1</span></span>

<span data-ttu-id="a45ca-449">Öznitelikler, değerler sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-449">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="a45ca-450">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="a45ca-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a45ca-451">anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="a45ca-451">key:attribute</span></span>
* <span data-ttu-id="a45ca-452">Section: Key: özniteliği</span><span class="sxs-lookup"><span data-stu-id="a45ca-452">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="a45ca-453">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-453">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="a45ca-454"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-454">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="a45ca-455">Anahtar, dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-455">The key is the file name.</span></span> <span data-ttu-id="a45ca-456">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-456">The value contains the file's contents.</span></span> <span data-ttu-id="a45ca-457">Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-457">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="a45ca-458">Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-458">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="a45ca-459">Dosyaların `directoryPath` mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-459">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="a45ca-460">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="a45ca-460">Overloads permit specifying:</span></span>

* <span data-ttu-id="a45ca-461">Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="a45ca-461">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="a45ca-462">Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-462">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="a45ca-463">Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-463">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="a45ca-464">Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-464">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="a45ca-465">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-465">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="a45ca-466">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-466">Memory Configuration Provider</span></span>

<span data-ttu-id="a45ca-467"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-467">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="a45ca-468">Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-468">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a45ca-469">Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>` başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-469">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="a45ca-470">Uygulamanın yapılandırmasını belirtmek için Konağı derlerken `ConfigureAppConfiguration` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-470">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="a45ca-471">Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-471">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="a45ca-472">Sözlük, yapılandırmayı sağlamak için bir `AddInMemoryCollection` çağrısıyla kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-472">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="a45ca-473">GetValue</span><span class="sxs-lookup"><span data-stu-id="a45ca-473">GetValue</span></span>

<span data-ttu-id="a45ca-474">[Configurationciltçi. GetValue \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen koleksiyon olmayan türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a45ca-474">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="a45ca-475">Aşırı yükleme varsayılan bir değeri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="a45ca-475">An overload accepts a default value.</span></span>

<span data-ttu-id="a45ca-476">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="a45ca-476">The following example:</span></span>

* <span data-ttu-id="a45ca-477">Anahtar `NumberKey`, yapılandırmadan dize değerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-477">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="a45ca-478">Yapılandırma anahtarlarında `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-478">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="a45ca-479">Değeri bir `int` olarak türler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-479">Types the value as an `int`.</span></span>
* <span data-ttu-id="a45ca-480">Değeri, sayfanın kullanımı için `NumberConfig` özelliği içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-480">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="a45ca-481">GetSection, GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="a45ca-481">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="a45ca-482">İzleyen örnekler için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="a45ca-482">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="a45ca-483">İki bölüm arasında dört anahtar bulunur ve bunlardan biri alt bölümleri çifti içerir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-483">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="a45ca-484">Dosya yapılandırmaya okunduğu zaman yapılandırma değerlerini tutmak için aşağıdaki benzersiz hiyerarşik anahtarlar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-484">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="a45ca-485">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-485">section0:key0</span></span>
* <span data-ttu-id="a45ca-486">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-486">section0:key1</span></span>
* <span data-ttu-id="a45ca-487">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-487">section1:key0</span></span>
* <span data-ttu-id="a45ca-488">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-488">section1:key1</span></span>
* <span data-ttu-id="a45ca-489">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-489">section2:subsection0:key0</span></span>
* <span data-ttu-id="a45ca-490">section2: subsection0: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-490">section2:subsection0:key1</span></span>
* <span data-ttu-id="a45ca-491">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="a45ca-491">section2:subsection1:key0</span></span>
* <span data-ttu-id="a45ca-492">section2: subsection1: KEY1</span><span class="sxs-lookup"><span data-stu-id="a45ca-492">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="a45ca-493">GetSection</span><span class="sxs-lookup"><span data-stu-id="a45ca-493">GetSection</span></span>

<span data-ttu-id="a45ca-494">[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü ayıklar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-494">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="a45ca-495">`section1`yalnızca anahtar-değer çiftlerini içeren bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> döndürmek için `GetSection` çağırın ve bölüm adını sağlayın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-495">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="a45ca-496">`configSection` bir değer, yalnızca bir anahtar ve yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-496">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="a45ca-497">Benzer şekilde, `section2:subsection0` anahtarlar için değerleri almak için, `GetSection` çağırın ve Bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-497">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="a45ca-498">`GetSection` hiçbir şekilde `null` döndürmez.</span><span class="sxs-lookup"><span data-stu-id="a45ca-498">`GetSection` never returns `null`.</span></span> <span data-ttu-id="a45ca-499">Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a45ca-499">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="a45ca-500">`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor.</span><span class="sxs-lookup"><span data-stu-id="a45ca-500">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="a45ca-501">Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="a45ca-501">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="a45ca-502">GetChildren</span><span class="sxs-lookup"><span data-stu-id="a45ca-502">GetChildren</span></span>

<span data-ttu-id="a45ca-503">`section2` üzerinde [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı, şunları içeren bir `IEnumerable<IConfigurationSection>` edinir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-503">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="a45ca-504">Bulunur</span><span class="sxs-lookup"><span data-stu-id="a45ca-504">Exists</span></span>

<span data-ttu-id="a45ca-505">Bir yapılandırma bölümünün mevcut olup olmadığını anlamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) kullanın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-505">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="a45ca-506">Örnek veriler verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölümü olmadığından `sectionExists` `false`.</span><span class="sxs-lookup"><span data-stu-id="a45ca-506">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="a45ca-507">Bir sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="a45ca-507">Bind to a class</span></span>

<span data-ttu-id="a45ca-508">Yapılandırma, *Seçenekler modelini*kullanarak ilgili ayarların gruplarını temsil eden sınıflara bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-508">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="a45ca-509">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-509">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="a45ca-510">Yapılandırma değerleri dizeler olarak döndürülür, ancak <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> çağrısı [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesnelerinin oluşturulmasını mümkün yapar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-510">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="a45ca-511">Örnek uygulama bir `Starship` modeli içerir (*modeller/Başlangıçgönder. cs*):</span><span class="sxs-lookup"><span data-stu-id="a45ca-511">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a45ca-512">*Starsevk. JSON* dosyasının `starship` bölümü, örnek uygulama yapılandırmayı yüklemek Için JSON yapılandırma sağlayıcısını kullandığında yapılandırmayı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-512">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="a45ca-513">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-513">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="a45ca-514">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-514">Key</span></span>                   | <span data-ttu-id="a45ca-515">Değer</span><span class="sxs-lookup"><span data-stu-id="a45ca-515">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="a45ca-516">starsevk: ad</span><span class="sxs-lookup"><span data-stu-id="a45ca-516">starship:name</span></span>         | <span data-ttu-id="a45ca-517">USS kurumsal</span><span class="sxs-lookup"><span data-stu-id="a45ca-517">USS Enterprise</span></span>                                    |
| <span data-ttu-id="a45ca-518">starsevk: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="a45ca-518">starship:registry</span></span>     | <span data-ttu-id="a45ca-519">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="a45ca-519">NCC-1701</span></span>                                          |
| <span data-ttu-id="a45ca-520">starsevk: sınıfı</span><span class="sxs-lookup"><span data-stu-id="a45ca-520">starship:class</span></span>        | <span data-ttu-id="a45ca-521">Anaytion</span><span class="sxs-lookup"><span data-stu-id="a45ca-521">Constitution</span></span>                                      |
| <span data-ttu-id="a45ca-522">başlangıçgönder: Uzunluk</span><span class="sxs-lookup"><span data-stu-id="a45ca-522">starship:length</span></span>       | <span data-ttu-id="a45ca-523">304,8</span><span class="sxs-lookup"><span data-stu-id="a45ca-523">304.8</span></span>                                             |
| <span data-ttu-id="a45ca-524">starsevkiyat: Commissioned</span><span class="sxs-lookup"><span data-stu-id="a45ca-524">starship:commissioned</span></span> | <span data-ttu-id="a45ca-525">False</span><span class="sxs-lookup"><span data-stu-id="a45ca-525">False</span></span>                                             |
| <span data-ttu-id="a45ca-526">dır</span><span class="sxs-lookup"><span data-stu-id="a45ca-526">trademark</span></span>             | <span data-ttu-id="a45ca-527">Paramount resimleri Corp.  https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="a45ca-527">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="a45ca-528">Örnek uygulama, `starship` anahtarıyla `GetSection` çağırır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-528">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="a45ca-529">`starship` anahtar-değer çiftleri yalıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-529">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="a45ca-530">`Bind` yöntemi, `Starship` sınıfının bir örneğinde geçen alt bölümde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-530">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="a45ca-531">Örnek değerlerini bağladıktan sonra örnek, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-531">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="a45ca-532">Bir nesne grafiğine bağlama</span><span class="sxs-lookup"><span data-stu-id="a45ca-532">Bind to an object graph</span></span>

<span data-ttu-id="a45ca-533"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, tüm POCO nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-533"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="a45ca-534">Örnek, nesne grafı `Metadata` ve `Actors` sınıfları (*modeller/TvShow. cs*) içeren bir `TvShow` modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-534">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a45ca-535">Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-535">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="a45ca-536">Yapılandırma, `Bind` yöntemi ile `TvShow` nesne grafiğinin tamamına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-536">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="a45ca-537">Bağlantılı örnek, işleme için bir özelliğe atandı:</span><span class="sxs-lookup"><span data-stu-id="a45ca-537">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="a45ca-538">[Configurationciltçi. Get \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="a45ca-538">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="a45ca-539">`Get<T>`, `Bind` kullanmaktan daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-539">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="a45ca-540">Aşağıdaki kod, ilişkili örneğin işleme için kullanılan özelliğe doğrudan atanmasını sağlayan önceki örnekle `Get<T>` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-540">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="a45ca-541">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="a45ca-541">Bind an array to a class</span></span>

<span data-ttu-id="a45ca-542">*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*</span><span class="sxs-lookup"><span data-stu-id="a45ca-542">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="a45ca-543"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="a45ca-543">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="a45ca-544">Sayısal anahtar segmentini (`:0:`, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir POCO sınıf dizisine dizi bağlama özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-544">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="a45ca-545">Bağlama, kural tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-545">Binding is provided by convention.</span></span> <span data-ttu-id="a45ca-546">Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-546">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="a45ca-547">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="a45ca-547">**In-memory array processing**</span></span>

<span data-ttu-id="a45ca-548">Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="a45ca-548">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="a45ca-549">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-549">Key</span></span>             | <span data-ttu-id="a45ca-550">Değer</span><span class="sxs-lookup"><span data-stu-id="a45ca-550">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="a45ca-551">dizi: girdiler: 0</span><span class="sxs-lookup"><span data-stu-id="a45ca-551">array:entries:0</span></span> | <span data-ttu-id="a45ca-552">value0</span><span class="sxs-lookup"><span data-stu-id="a45ca-552">value0</span></span> |
| <span data-ttu-id="a45ca-553">dizi: girdiler: 1</span><span class="sxs-lookup"><span data-stu-id="a45ca-553">array:entries:1</span></span> | <span data-ttu-id="a45ca-554">value1</span><span class="sxs-lookup"><span data-stu-id="a45ca-554">value1</span></span> |
| <span data-ttu-id="a45ca-555">dizi: girdiler: 2</span><span class="sxs-lookup"><span data-stu-id="a45ca-555">array:entries:2</span></span> | <span data-ttu-id="a45ca-556">value2</span><span class="sxs-lookup"><span data-stu-id="a45ca-556">value2</span></span> |
| <span data-ttu-id="a45ca-557">dizi: girdiler: 4</span><span class="sxs-lookup"><span data-stu-id="a45ca-557">array:entries:4</span></span> | <span data-ttu-id="a45ca-558">value4</span><span class="sxs-lookup"><span data-stu-id="a45ca-558">value4</span></span> |
| <span data-ttu-id="a45ca-559">dizi: girdiler: 5</span><span class="sxs-lookup"><span data-stu-id="a45ca-559">array:entries:5</span></span> | <span data-ttu-id="a45ca-560">value5</span><span class="sxs-lookup"><span data-stu-id="a45ca-560">value5</span></span> |

<span data-ttu-id="a45ca-561">Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-561">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="a45ca-562">Dizi, Dizin &num;3 için bir değer atlar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-562">The array skips a value for index &num;3.</span></span> <span data-ttu-id="a45ca-563">Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-563">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="a45ca-564">Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-564">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a45ca-565">Yapılandırma verileri nesnesine bağlanır:</span><span class="sxs-lookup"><span data-stu-id="a45ca-565">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="a45ca-566">[Configurationciltçi. Get \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-566">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="a45ca-567">`ArrayExample`bir örneği olan bağlantılı nesne, yapılandırmadan dizi verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-567">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="a45ca-568">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="a45ca-568">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="a45ca-569">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="a45ca-569">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="a45ca-570">0</span><span class="sxs-lookup"><span data-stu-id="a45ca-570">0</span></span>                            | <span data-ttu-id="a45ca-571">value0</span><span class="sxs-lookup"><span data-stu-id="a45ca-571">value0</span></span>                       |
| <span data-ttu-id="a45ca-572">1\.</span><span class="sxs-lookup"><span data-stu-id="a45ca-572">1</span></span>                            | <span data-ttu-id="a45ca-573">value1</span><span class="sxs-lookup"><span data-stu-id="a45ca-573">value1</span></span>                       |
| <span data-ttu-id="a45ca-574">2</span><span class="sxs-lookup"><span data-stu-id="a45ca-574">2</span></span>                            | <span data-ttu-id="a45ca-575">value2</span><span class="sxs-lookup"><span data-stu-id="a45ca-575">value2</span></span>                       |
| <span data-ttu-id="a45ca-576">3</span><span class="sxs-lookup"><span data-stu-id="a45ca-576">3</span></span>                            | <span data-ttu-id="a45ca-577">value4</span><span class="sxs-lookup"><span data-stu-id="a45ca-577">value4</span></span>                       |
| <span data-ttu-id="a45ca-578">4</span><span class="sxs-lookup"><span data-stu-id="a45ca-578">4</span></span>                            | <span data-ttu-id="a45ca-579">value5</span><span class="sxs-lookup"><span data-stu-id="a45ca-579">value5</span></span>                       |

<span data-ttu-id="a45ca-580">İlişkili nesnedeki Dizin &num;3, `array:4` yapılandırma anahtarı ve `value4` değeri için yapılandırma verilerini tutar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-580">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="a45ca-581">Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-581">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="a45ca-582">Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="a45ca-582">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="a45ca-583">Yapılandırmada doğru anahtar-değer çiftini üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce dizin &num;3 için eksik yapılandırma öğesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-583">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="a45ca-584">Örnek, eksik anahtar-değer çiftine sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, `ArrayExample.Entries` tam yapılandırma dizisiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-584">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="a45ca-585">*missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-585">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="a45ca-586">`ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a45ca-586">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="a45ca-587">Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.</span><span class="sxs-lookup"><span data-stu-id="a45ca-587">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="a45ca-588">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-588">Key</span></span>             | <span data-ttu-id="a45ca-589">Değer</span><span class="sxs-lookup"><span data-stu-id="a45ca-589">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="a45ca-590">dizi: girdiler: 3</span><span class="sxs-lookup"><span data-stu-id="a45ca-590">array:entries:3</span></span> | <span data-ttu-id="a45ca-591">value3</span><span class="sxs-lookup"><span data-stu-id="a45ca-591">value3</span></span> |

<span data-ttu-id="a45ca-592">`ArrayExample` sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ün girdisini içeriyorsa, `ArrayExample.Entries` dizisi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-592">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="a45ca-593">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="a45ca-593">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="a45ca-594">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="a45ca-594">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="a45ca-595">0</span><span class="sxs-lookup"><span data-stu-id="a45ca-595">0</span></span>                            | <span data-ttu-id="a45ca-596">value0</span><span class="sxs-lookup"><span data-stu-id="a45ca-596">value0</span></span>                       |
| <span data-ttu-id="a45ca-597">1\.</span><span class="sxs-lookup"><span data-stu-id="a45ca-597">1</span></span>                            | <span data-ttu-id="a45ca-598">value1</span><span class="sxs-lookup"><span data-stu-id="a45ca-598">value1</span></span>                       |
| <span data-ttu-id="a45ca-599">2</span><span class="sxs-lookup"><span data-stu-id="a45ca-599">2</span></span>                            | <span data-ttu-id="a45ca-600">value2</span><span class="sxs-lookup"><span data-stu-id="a45ca-600">value2</span></span>                       |
| <span data-ttu-id="a45ca-601">3</span><span class="sxs-lookup"><span data-stu-id="a45ca-601">3</span></span>                            | <span data-ttu-id="a45ca-602">value3</span><span class="sxs-lookup"><span data-stu-id="a45ca-602">value3</span></span>                       |
| <span data-ttu-id="a45ca-603">4</span><span class="sxs-lookup"><span data-stu-id="a45ca-603">4</span></span>                            | <span data-ttu-id="a45ca-604">value4</span><span class="sxs-lookup"><span data-stu-id="a45ca-604">value4</span></span>                       |
| <span data-ttu-id="a45ca-605">5</span><span class="sxs-lookup"><span data-stu-id="a45ca-605">5</span></span>                            | <span data-ttu-id="a45ca-606">value5</span><span class="sxs-lookup"><span data-stu-id="a45ca-606">value5</span></span>                       |

<span data-ttu-id="a45ca-607">**JSON dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="a45ca-607">**JSON array processing**</span></span>

<span data-ttu-id="a45ca-608">JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-608">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="a45ca-609">Aşağıdaki yapılandırma dosyasında, `subsection` bir dizidir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-609">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="a45ca-610">JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="a45ca-610">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="a45ca-611">Anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-611">Key</span></span>                     | <span data-ttu-id="a45ca-612">Değer</span><span class="sxs-lookup"><span data-stu-id="a45ca-612">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="a45ca-613">json_array: anahtar</span><span class="sxs-lookup"><span data-stu-id="a45ca-613">json_array:key</span></span>          | <span data-ttu-id="a45ca-614">değer EA</span><span class="sxs-lookup"><span data-stu-id="a45ca-614">valueA</span></span> |
| <span data-ttu-id="a45ca-615">json_array: alt bölüm: 0</span><span class="sxs-lookup"><span data-stu-id="a45ca-615">json_array:subsection:0</span></span> | <span data-ttu-id="a45ca-616">valueB</span><span class="sxs-lookup"><span data-stu-id="a45ca-616">valueB</span></span> |
| <span data-ttu-id="a45ca-617">json_array: alt bölüm: 1</span><span class="sxs-lookup"><span data-stu-id="a45ca-617">json_array:subsection:1</span></span> | <span data-ttu-id="a45ca-618">değer EC</span><span class="sxs-lookup"><span data-stu-id="a45ca-618">valueC</span></span> |
| <span data-ttu-id="a45ca-619">json_array: alt bölüm: 2</span><span class="sxs-lookup"><span data-stu-id="a45ca-619">json_array:subsection:2</span></span> | <span data-ttu-id="a45ca-620">Değerler</span><span class="sxs-lookup"><span data-stu-id="a45ca-620">valueD</span></span> |

<span data-ttu-id="a45ca-621">Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-621">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a45ca-622">Bağlamadan sonra, `JsonArrayExample.Key` `valueA` değerini tutar.</span><span class="sxs-lookup"><span data-stu-id="a45ca-622">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="a45ca-623">Alt bölüm değerleri, `Subsection` POCO dizisi özelliğinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-623">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="a45ca-624">`JsonArrayExample.Subsection` dizini</span><span class="sxs-lookup"><span data-stu-id="a45ca-624">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="a45ca-625">`JsonArrayExample.Subsection` değeri</span><span class="sxs-lookup"><span data-stu-id="a45ca-625">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="a45ca-626">0</span><span class="sxs-lookup"><span data-stu-id="a45ca-626">0</span></span>                                   | <span data-ttu-id="a45ca-627">valueB</span><span class="sxs-lookup"><span data-stu-id="a45ca-627">valueB</span></span>                              |
| <span data-ttu-id="a45ca-628">1\.</span><span class="sxs-lookup"><span data-stu-id="a45ca-628">1</span></span>                                   | <span data-ttu-id="a45ca-629">değer EC</span><span class="sxs-lookup"><span data-stu-id="a45ca-629">valueC</span></span>                              |
| <span data-ttu-id="a45ca-630">2</span><span class="sxs-lookup"><span data-stu-id="a45ca-630">2</span></span>                                   | <span data-ttu-id="a45ca-631">Değerler</span><span class="sxs-lookup"><span data-stu-id="a45ca-631">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="a45ca-632">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="a45ca-632">Custom configuration provider</span></span>

<span data-ttu-id="a45ca-633">Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-633">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="a45ca-634">Sağlayıcı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-634">The provider has the following characteristics:</span></span>

* <span data-ttu-id="a45ca-635">EF bellek içi veritabanı, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-635">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="a45ca-636">Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-636">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="a45ca-637">Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur.</span><span class="sxs-lookup"><span data-stu-id="a45ca-637">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="a45ca-638">Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.</span><span class="sxs-lookup"><span data-stu-id="a45ca-638">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="a45ca-639">Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-639">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="a45ca-640">Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="a45ca-640">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a45ca-641">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-641">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="a45ca-642">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-642">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="a45ca-643">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-643">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a45ca-644"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a45ca-644">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="a45ca-645">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-645">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="a45ca-646"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a45ca-646">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="a45ca-647">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-647">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="a45ca-648">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-648">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="a45ca-649">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-649">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="a45ca-650">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-650">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="a45ca-651">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-651">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a45ca-652">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-652">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="a45ca-653">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-653">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="a45ca-654">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-654">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a45ca-655"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a45ca-655">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="a45ca-656">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-656">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="a45ca-657"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a45ca-657">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="a45ca-658">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="a45ca-658">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="a45ca-659">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-659">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="a45ca-660">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-660">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="a45ca-661">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="a45ca-661">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="a45ca-662">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="a45ca-662">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="a45ca-663">Başlangıç sırasında yapılandırmaya erişim</span><span class="sxs-lookup"><span data-stu-id="a45ca-663">Access configuration during startup</span></span>

<span data-ttu-id="a45ca-664">`Startup.ConfigureServices`yapılandırma değerlerine erişmek için `Startup` oluşturucusuna `IConfiguration` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-664">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a45ca-665">`Startup.Configure`yapılandırmaya erişmek için `IConfiguration` doğrudan yönteme ekleyin ya da oluşturucuyu kullanarak örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="a45ca-665">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="a45ca-666">Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. [uygulama başlatma: kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="a45ca-666">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="a45ca-667">Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a45ca-667">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="a45ca-668">Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfa ya da görünüme <xref:Microsoft.Extensions.Configuration.IConfiguration> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a45ca-668">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="a45ca-669">Razor Pages sayfasında:</span><span class="sxs-lookup"><span data-stu-id="a45ca-669">In a Razor Pages page:</span></span>

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

<span data-ttu-id="a45ca-670">MVC görünümünde:</span><span class="sxs-lookup"><span data-stu-id="a45ca-670">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="a45ca-671">Bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="a45ca-671">Add configuration from an external assembly</span></span>

<span data-ttu-id="a45ca-672"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a45ca-672">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="a45ca-673">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a45ca-673">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a45ca-674">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a45ca-674">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
