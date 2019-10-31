---
title: ASP.NET Core yapılandırma
author: guardrex
description: ASP.NET Core uygulamasını yapılandırmak için yapılandırma API 'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: c63609cfb91a1668b8e125c54fcfecf5f4ec259b
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143354"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="24940-103">ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24940-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="24940-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="24940-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="24940-105">ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır.</span><span class="sxs-lookup"><span data-stu-id="24940-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="24940-106">Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="24940-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="24940-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24940-107">Azure Key Vault</span></span>
* <span data-ttu-id="24940-108">Azure Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="24940-108">Azure App Configuration</span></span>
* <span data-ttu-id="24940-109">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24940-109">Command-line arguments</span></span>
* <span data-ttu-id="24940-110">Özel sağlayıcılar (yüklü veya oluşturulmuş)</span><span class="sxs-lookup"><span data-stu-id="24940-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="24940-111">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="24940-111">Directory files</span></span>
* <span data-ttu-id="24940-112">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24940-112">Environment variables</span></span>
* <span data-ttu-id="24940-113">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="24940-113">In-memory .NET objects</span></span>
* <span data-ttu-id="24940-114">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="24940-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="24940-115">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), Framework tarafından örtük olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="24940-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="24940-116">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="24940-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="24940-117">Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="24940-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="24940-118">*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="24940-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="24940-119">Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="24940-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="24940-120">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="24940-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="24940-121">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="24940-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="24940-122">Uygulama yapılandırmasına karşı konak</span><span class="sxs-lookup"><span data-stu-id="24940-122">Host versus app configuration</span></span>

<span data-ttu-id="24940-123">Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır.</span><span class="sxs-lookup"><span data-stu-id="24940-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="24940-124">Ana bilgisayar, uygulama başlatma ve ömür yönetiminden sorumludur.</span><span class="sxs-lookup"><span data-stu-id="24940-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="24940-125">Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="24940-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="24940-126">Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir.</span><span class="sxs-lookup"><span data-stu-id="24940-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="24940-127">Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="24940-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="24940-128">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24940-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="24940-129">ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="24940-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="24940-130">`CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="24940-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="24940-131">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="24940-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="24940-132">[Web konağını](xref:fundamentals/host/web-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [bu konunun ASP.NET Core 2,2 sürümüne](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="24940-133">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="24940-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="24940-134">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `DOTNET_` (örneğin, `DOTNET_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="24940-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="24940-135">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`DOTNET_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="24940-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="24940-136">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="24940-137">Web ana bilgisayar varsayılan yapılandırması oluşturuldu (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="24940-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="24940-138">Kestrel, Web sunucusu olarak kullanılır ve uygulamanın yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="24940-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="24940-139">Konak filtreleme ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="24940-140">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` ortam değişkeni `true`olarak ayarlandıysa, Iletilen üstbilgiler ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="24940-141">IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="24940-141">Enable IIS integration.</span></span>
* <span data-ttu-id="24940-142">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="24940-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="24940-143">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="24940-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="24940-144">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="24940-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="24940-145">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="24940-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="24940-146">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="24940-147">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="24940-148">ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="24940-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="24940-149">`CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="24940-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="24940-150">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="24940-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="24940-151">[Genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [Bu konunun en son sürümüne](xref:fundamentals/configuration/index)bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="24940-152">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="24940-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="24940-153">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) ön ekine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="24940-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="24940-154">Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`ASPNETCORE_`) çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="24940-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="24940-155">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="24940-156">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="24940-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="24940-157">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="24940-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="24940-158">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="24940-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="24940-159">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="24940-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="24940-160">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="24940-161">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="24940-162">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="24940-162">Security</span></span>

<span data-ttu-id="24940-163">Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="24940-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="24940-164">Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="24940-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="24940-165">Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="24940-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="24940-166">Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="24940-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="24940-167">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="24940-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="24940-168"><xref:security/app-secrets> &ndash;, hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler Içerir.</span><span class="sxs-lookup"><span data-stu-id="24940-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="24940-169">Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="24940-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="24940-170">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="24940-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="24940-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar.</span><span class="sxs-lookup"><span data-stu-id="24940-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="24940-172">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="24940-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="24940-173">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="24940-173">Hierarchical configuration data</span></span>

<span data-ttu-id="24940-174">Yapılandırma API 'SI, hiyerarşik verileri yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini muhafaza ediyor.</span><span class="sxs-lookup"><span data-stu-id="24940-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="24940-175">Aşağıdaki JSON dosyasında, iki bölümün yapılandırılmış hiyerarşisinde dört anahtar mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="24940-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="24940-176">Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24940-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="24940-177">Bölüm ve anahtarlar, özgün yapıyı sürdürmek için iki nokta üst üste (`:`) kullanımıyla düzleştirilir:</span><span class="sxs-lookup"><span data-stu-id="24940-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="24940-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24940-178">section0:key0</span></span>
* <span data-ttu-id="24940-179">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-179">section0:key1</span></span>
* <span data-ttu-id="24940-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="24940-180">section1:key0</span></span>
* <span data-ttu-id="24940-181">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-181">section1:key1</span></span>

<span data-ttu-id="24940-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="24940-183">Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="24940-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="24940-184">Kurallar</span><span class="sxs-lookup"><span data-stu-id="24940-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="24940-185">Kaynaklar ve sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="24940-185">Sources and providers</span></span>

<span data-ttu-id="24940-186">Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="24940-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="24940-187">Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="24940-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="24940-188">Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.</span><span class="sxs-lookup"><span data-stu-id="24940-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="24940-189"><xref:Microsoft.Extensions.Configuration.IConfiguration>, uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="24940-190"><xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfın yapılandırmasını elde etmek için bir Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="24940-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="24940-191">Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="24940-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="24940-192">Belirlenmesine</span><span class="sxs-lookup"><span data-stu-id="24940-192">Keys</span></span>

<span data-ttu-id="24940-193">Yapılandırma anahtarları aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="24940-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="24940-194">Anahtarlar büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="24940-194">Keys are case-insensitive.</span></span> <span data-ttu-id="24940-195">Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="24940-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="24940-196">Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="24940-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="24940-197">Hiyerarşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="24940-197">Hierarchical keys</span></span>
  * <span data-ttu-id="24940-198">Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="24940-199">Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="24940-200">Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="24940-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="24940-201">Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="24940-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="24940-202">Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tirelerin yerini iki nokta ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="24940-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="24940-203"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="24940-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="24940-204">Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="24940-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="24940-205">Değerler</span><span class="sxs-lookup"><span data-stu-id="24940-205">Values</span></span>

<span data-ttu-id="24940-206">Yapılandırma değerleri aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="24940-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="24940-207">Değerler dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="24940-207">Values are strings.</span></span>
* <span data-ttu-id="24940-208">Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="24940-209">sağlayıcılarla</span><span class="sxs-lookup"><span data-stu-id="24940-209">Providers</span></span>

<span data-ttu-id="24940-210">Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="24940-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="24940-211">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24940-211">Provider</span></span> | <span data-ttu-id="24940-212">&hellip; yapılandırma sağlar</span><span class="sxs-lookup"><span data-stu-id="24940-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="24940-213">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="24940-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="24940-214">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24940-214">Azure Key Vault</span></span> |
| <span data-ttu-id="24940-215">[Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri)</span><span class="sxs-lookup"><span data-stu-id="24940-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="24940-216">Azure Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="24940-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="24940-217">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="24940-218">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="24940-218">Command-line parameters</span></span> |
| [<span data-ttu-id="24940-219">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="24940-220">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="24940-220">Custom source</span></span> |
| [<span data-ttu-id="24940-221">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="24940-222">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24940-222">Environment variables</span></span> |
| [<span data-ttu-id="24940-223">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="24940-224">Dosyalar (ıNı, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="24940-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="24940-225">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="24940-226">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="24940-226">Directory files</span></span> |
| [<span data-ttu-id="24940-227">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="24940-228">Bellek içi Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="24940-228">In-memory collections</span></span> |
| <span data-ttu-id="24940-229">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="24940-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="24940-230">Kullanıcı profili dizinindeki dosya</span><span class="sxs-lookup"><span data-stu-id="24940-230">File in the user profile directory</span></span> |

<span data-ttu-id="24940-231">Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="24940-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="24940-232">Bu konuda açıklanan yapılandırma sağlayıcıları, kodunuzun onları düzenleyebilir sırada değil alfabetik sırayla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="24940-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="24940-233">Kodunuzda yapılandırma sağlayıcılarını, temeldeki yapılandırma kaynakları için önceliklerinize uyacak şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="24940-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="24940-234">Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:</span><span class="sxs-lookup"><span data-stu-id="24940-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="24940-235">Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır.</span><span class="sxs-lookup"><span data-stu-id="24940-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="24940-236">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24940-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="24940-237">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (yalnızca geliştirme ortamı)</span><span class="sxs-lookup"><span data-stu-id="24940-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="24940-238">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24940-238">Environment variables</span></span>
1. <span data-ttu-id="24940-239">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="24940-239">Command-line arguments</span></span>

<span data-ttu-id="24940-240">Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="24940-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="24940-241">Önceki sağlayıcı dizisi, `CreateDefaultBuilder` ile yeni bir konak Oluşturucu başlattığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24940-242">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="24940-243">Konak oluşturucuyu ConfigureHostConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24940-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="24940-244">Konak oluşturucuyu yapılandırmak için <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> çağırın ve yapılandırmayı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="24940-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="24940-245">`ConfigureHostConfiguration`, derleme sürecinde daha sonra kullanmak üzere <xref:Microsoft.Extensions.Hosting.IHostEnvironment> başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="24940-246">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="24940-247">Konak oluşturucuyu UseConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="24940-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="24940-248">Konak oluşturucuyu yapılandırmak için, yapılandırma ile konak Oluşturucu üzerinde <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="24940-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="24940-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="24940-250">`CreateDefaultBuilder`tarafından otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirlemek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="24940-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="24940-251">Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="24940-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="24940-252">Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için `AddCommandLine` son ' u çağırın:</span><span class="sxs-lookup"><span data-stu-id="24940-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="24940-253">CreateDefaultBuilder tarafından eklenen sağlayıcıları kaldır</span><span class="sxs-lookup"><span data-stu-id="24940-253">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="24940-254">`CreateDefaultBuilder`tarafından eklenen sağlayıcıları kaldırmak için, önce [ılisteationbuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) üzerinde [clear](/dotnet/api/system.collections.generic.icollection-1.clear) öğesini çağırın:</span><span class="sxs-lookup"><span data-stu-id="24940-254">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="24940-255">Uygulama başlatma sırasında yapılandırmayı kullan</span><span class="sxs-lookup"><span data-stu-id="24940-255">Consume configuration during app startup</span></span>

<span data-ttu-id="24940-256">`ConfigureAppConfiguration` içinde uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında `Startup.ConfigureServices`dahil olmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-256">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="24940-257">Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-257">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="24940-258">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-258">Command-line Configuration Provider</span></span>

<span data-ttu-id="24940-259"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, çalışma zamanında komut satırı bağımsız değişkeni anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="24940-259">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="24940-260">Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> uzantısı yöntemi bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> örneğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="24940-260">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24940-261">`AddCommandLine`, `CreateDefaultBuilder(string [])` çağrıldığında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="24940-261">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="24940-262">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-262">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="24940-263">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="24940-263">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="24940-264">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="24940-264">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="24940-265">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="24940-265">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="24940-266">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-266">Environment variables.</span></span>

<span data-ttu-id="24940-267">`CreateDefaultBuilder`, komut satırı yapılandırma sağlayıcısını en son ekler.</span><span class="sxs-lookup"><span data-stu-id="24940-267">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="24940-268">Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.</span><span class="sxs-lookup"><span data-stu-id="24940-268">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="24940-269">`CreateDefaultBuilder` ana bilgisayar oluşturulduğunda davranır.</span><span class="sxs-lookup"><span data-stu-id="24940-269">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="24940-270">Bu nedenle, `CreateDefaultBuilder` tarafından etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="24940-270">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="24940-271">ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` `CreateDefaultBuilder` tarafından zaten çağırılır.</span><span class="sxs-lookup"><span data-stu-id="24940-271">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24940-272">Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için, `ConfigureAppConfiguration` 'de uygulamanın ek sağlayıcılarını çağırın ve `AddCommandLine` son ' u çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-272">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="24940-273">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="24940-273">**Example**</span></span>

<span data-ttu-id="24940-274">Örnek uygulama, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="24940-274">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="24940-275">Projenin dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="24940-275">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="24940-276">`dotnet run` komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="24940-276">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="24940-277">Uygulama çalıştıktan sonra, `http://localhost:5000` konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="24940-277">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="24940-278">Çıktının `dotnet run` için belirtilen yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-278">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="24940-279">Arguments</span><span class="sxs-lookup"><span data-stu-id="24940-279">Arguments</span></span>

<span data-ttu-id="24940-280">Değer bir eşittir işareti (`=`) izlemelidir veya değer bir boşluk izleyen anahtarın bir ön eki (`--` veya `/`) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="24940-280">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="24940-281">Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="24940-281">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="24940-282">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="24940-282">Key prefix</span></span>               | <span data-ttu-id="24940-283">Örnek</span><span class="sxs-lookup"><span data-stu-id="24940-283">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="24940-284">Ön ek yok</span><span class="sxs-lookup"><span data-stu-id="24940-284">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="24940-285">İki kısa çizgi (`--`)</span><span class="sxs-lookup"><span data-stu-id="24940-285">Two dashes (`--`)</span></span>        | <span data-ttu-id="24940-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="24940-286">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="24940-287">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="24940-287">Forward slash (`/`)</span></span>      | <span data-ttu-id="24940-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="24940-288">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="24940-289">Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="24940-289">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="24940-290">Örnek komutlar:</span><span class="sxs-lookup"><span data-stu-id="24940-290">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="24940-291">Eşleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="24940-291">Switch mappings</span></span>

<span data-ttu-id="24940-292">Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir.</span><span class="sxs-lookup"><span data-stu-id="24940-292">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="24940-293">Yapılandırmayı bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ile el ile oluşturduğunuzda, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemine anahtar değiştirme sözlüğü sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="24940-293">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="24940-294">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="24940-294">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="24940-295">Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="24940-295">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="24940-296">Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="24940-296">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="24940-297">Anahtar eşlemeleri sözlük anahtarı kuralları:</span><span class="sxs-lookup"><span data-stu-id="24940-297">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="24940-298">Anahtarlar tireyle (`-`) veya çift tireyle başlamalıdır (`--`).</span><span class="sxs-lookup"><span data-stu-id="24940-298">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="24940-299">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="24940-299">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="24940-300">Anahtar eşlemeleri sözlüğü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24940-300">Create a switch mappings dictionary.</span></span> <span data-ttu-id="24940-301">Aşağıdaki örnekte, iki anahtar eşlemesi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="24940-301">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="24940-302">Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne `AddCommandLine` çağırın:</span><span class="sxs-lookup"><span data-stu-id="24940-302">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="24940-303">Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir.</span><span class="sxs-lookup"><span data-stu-id="24940-303">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="24940-304">`CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`'e iletmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="24940-304">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24940-305">Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="24940-305">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="24940-306">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra, aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="24940-306">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="24940-307">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-307">Key</span></span>       | <span data-ttu-id="24940-308">Değer</span><span class="sxs-lookup"><span data-stu-id="24940-308">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="24940-309">Uygulama başlatılırken anahtar eşlenmiş anahtarlar kullanılıyorsa, yapılandırma sözlük tarafından sağlanan anahtardaki yapılandırma değerini alır:</span><span class="sxs-lookup"><span data-stu-id="24940-309">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="24940-310">Önceki komutu çalıştırdıktan sonra, yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="24940-310">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="24940-311">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-311">Key</span></span>               | <span data-ttu-id="24940-312">Değer</span><span class="sxs-lookup"><span data-stu-id="24940-312">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="24940-313">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-313">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="24940-314"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="24940-314">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="24940-315">Ortam değişkenleri yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-315">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="24940-316">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceğiniz ortam değişkenlerini ayarlamanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="24940-316">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="24940-317">Daha fazla bilgi için bkz. [Azure uygulamaları: Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="24940-317">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="24940-318">`AddEnvironmentVariables`, [genel ana bilgisayarla](xref:fundamentals/host/generic-host) yeni bir konak Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `DOTNET_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="24940-319">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="24940-320">`AddEnvironmentVariables`, [Web ana](xref:fundamentals/host/web-host) bilgisayarıyla yeni bir ana bilgisayar Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `ASPNETCORE_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-320">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="24940-321">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-321">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="24940-322">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="24940-322">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="24940-323">Önek olmadan `AddEnvironmentVariables` çağırarak, ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="24940-323">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="24940-324">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="24940-324">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="24940-325">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="24940-325">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="24940-326">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-326">Command-line arguments.</span></span>

<span data-ttu-id="24940-327">Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="24940-327">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="24940-328">Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="24940-328">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="24940-329">Ek ortam değişkenlerinden uygulama yapılandırması sağlamanız gerekiyorsa, uygulamanın `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle birlikte `AddEnvironmentVariables` çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-329">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="24940-330">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="24940-330">**Example**</span></span>

<span data-ttu-id="24940-331">Örnek uygulama, `AddEnvironmentVariables` bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="24940-331">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="24940-332">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24940-332">Run the sample app.</span></span> <span data-ttu-id="24940-333">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="24940-333">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="24940-334">Çıktının `ENVIRONMENT` ortam değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-334">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="24940-335">Değer, uygulamanın çalıştığı ortamı yansıtır, genellikle yerel olarak çalışırken `Development`.</span><span class="sxs-lookup"><span data-stu-id="24940-335">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="24940-336">Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler.</span><span class="sxs-lookup"><span data-stu-id="24940-336">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="24940-337">Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-337">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="24940-338">Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek istiyorsanız, *Pages/Index. cshtml. cs* ' deki `FilteredConfiguration` aşağıdaki gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="24940-338">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="24940-339">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="24940-339">Prefixes</span></span>

<span data-ttu-id="24940-340">Uygulama yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağladığınız zaman filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="24940-340">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="24940-341">Örneğin, önek `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:</span><span class="sxs-lookup"><span data-stu-id="24940-341">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="24940-342">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="24940-342">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="24940-343">Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="24940-343">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="24940-344">Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-344">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="24940-345">**Bağlantı dizesi önekleri**</span><span class="sxs-lookup"><span data-stu-id="24940-345">**Connection string prefixes**</span></span>

<span data-ttu-id="24940-346">Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir.</span><span class="sxs-lookup"><span data-stu-id="24940-346">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="24940-347">`AddEnvironmentVariables`için bir önek sağlanmazsa, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.</span><span class="sxs-lookup"><span data-stu-id="24940-347">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="24940-348">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="24940-348">Connection string prefix</span></span> | <span data-ttu-id="24940-349">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24940-349">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="24940-350">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="24940-350">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="24940-351">MySQL</span><span class="sxs-lookup"><span data-stu-id="24940-351">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="24940-352">Azure SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="24940-352">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="24940-353">SQL Server</span><span class="sxs-lookup"><span data-stu-id="24940-353">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="24940-354">Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:</span><span class="sxs-lookup"><span data-stu-id="24940-354">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="24940-355">Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24940-355">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="24940-356">Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).</span><span class="sxs-lookup"><span data-stu-id="24940-356">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="24940-357">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="24940-357">Environment variable key</span></span> | <span data-ttu-id="24940-358">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="24940-358">Converted configuration key</span></span> | <span data-ttu-id="24940-359">Sağlayıcı yapılandırma girişi</span><span class="sxs-lookup"><span data-stu-id="24940-359">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24940-360">Yapılandırma girişi oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="24940-360">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24940-361">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="24940-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="24940-362">Değer: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="24940-362">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24940-363">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="24940-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="24940-364">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="24940-364">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="24940-365">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="24940-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="24940-366">Değer: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="24940-366">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="24940-367">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-367">File Configuration Provider</span></span>

<span data-ttu-id="24940-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="24940-368"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="24940-369">Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="24940-369">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="24940-370">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-370">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="24940-371">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-371">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="24940-372">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-372">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="24940-373">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-373">INI Configuration Provider</span></span>

<span data-ttu-id="24940-374"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="24940-374">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="24940-375">INI dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> örneğinde <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-375">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24940-376">İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-376">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="24940-377">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="24940-377">Overloads permit specifying:</span></span>

* <span data-ttu-id="24940-378">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="24940-378">Whether the file is optional.</span></span>
* <span data-ttu-id="24940-379">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="24940-379">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="24940-380">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="24940-380">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="24940-381">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="24940-381">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="24940-382">Bir ıNı yapılandırma dosyasına genel bir örnek:</span><span class="sxs-lookup"><span data-stu-id="24940-382">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="24940-383">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="24940-383">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24940-384">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24940-384">section0:key0</span></span>
* <span data-ttu-id="24940-385">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-385">section0:key1</span></span>
* <span data-ttu-id="24940-386">Section1: alt bölüm: anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-386">section1:subsection:key</span></span>
* <span data-ttu-id="24940-387">section2: subsection0: anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-387">section2:subsection0:key</span></span>
* <span data-ttu-id="24940-388">section2: subsection1: anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-388">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="24940-389">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-389">JSON Configuration Provider</span></span>

<span data-ttu-id="24940-390"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="24940-390">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="24940-391">JSON dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-391">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24940-392">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="24940-392">Overloads permit specifying:</span></span>

* <span data-ttu-id="24940-393">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="24940-393">Whether the file is optional.</span></span>
* <span data-ttu-id="24940-394">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="24940-394">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="24940-395">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="24940-395">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="24940-396">`CreateDefaultBuilder` ile yeni bir ana bilgisayar Oluşturucu başlattığınızda `AddJsonFile` otomatik olarak iki kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="24940-396">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24940-397">Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="24940-397">The method is called to load configuration from:</span></span>

* <span data-ttu-id="24940-398">*appSettings. json* &ndash; bu dosya ilk kez okundu.</span><span class="sxs-lookup"><span data-stu-id="24940-398">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="24940-399">Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-399">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="24940-400">*appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="24940-400">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="24940-401">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="24940-401">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="24940-402">`CreateDefaultBuilder` de yüklenir:</span><span class="sxs-lookup"><span data-stu-id="24940-402">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="24940-403">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-403">Environment variables.</span></span>
* <span data-ttu-id="24940-404">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="24940-404">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="24940-405">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="24940-405">Command-line arguments.</span></span>

<span data-ttu-id="24940-406">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24940-406">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="24940-407">Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="24940-407">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="24940-408">Ana bilgisayarı derlerken, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtecek `ConfigureAppConfiguration` çağrısı yapın *. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="24940-408">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="24940-409">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="24940-409">**Example**</span></span>

<span data-ttu-id="24940-410">Örnek uygulama, `AddJsonFile` iki çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="24940-410">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="24940-411">Yapılandırma *appSettings. JSON* ve appSettings 'ten yüklendi *. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="24940-411">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="24940-412">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="24940-412">Run the sample app.</span></span> <span data-ttu-id="24940-413">`http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="24940-413">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="24940-414">Çıktının, ortama bağlı olarak tabloda gösterilen yapılandırma için anahtar-değer çiftleri içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-414">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="24940-415">Günlüğe kaydetme yapılandırma anahtarları hiyerarşik ayırıcı olarak iki nokta (`:`) kullanır.</span><span class="sxs-lookup"><span data-stu-id="24940-415">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="24940-416">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-416">Key</span></span>                        | <span data-ttu-id="24940-417">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="24940-417">Development Value</span></span> | <span data-ttu-id="24940-418">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="24940-418">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="24940-419">Günlüğe kaydetme: LogLevel: System</span><span class="sxs-lookup"><span data-stu-id="24940-419">Logging:LogLevel:System</span></span>    | <span data-ttu-id="24940-420">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24940-420">Information</span></span>       | <span data-ttu-id="24940-421">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24940-421">Information</span></span>      |
| <span data-ttu-id="24940-422">Günlüğe kaydetme: LogLevel: Microsoft</span><span class="sxs-lookup"><span data-stu-id="24940-422">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="24940-423">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24940-423">Information</span></span>       | <span data-ttu-id="24940-424">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="24940-424">Information</span></span>      |
| <span data-ttu-id="24940-425">Günlüğe kaydetme: LogLevel: default</span><span class="sxs-lookup"><span data-stu-id="24940-425">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="24940-426">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="24940-426">Debug</span></span>             | <span data-ttu-id="24940-427">Hata</span><span class="sxs-lookup"><span data-stu-id="24940-427">Error</span></span>            |
| <span data-ttu-id="24940-428">Allowedkonakları</span><span class="sxs-lookup"><span data-stu-id="24940-428">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="24940-429">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-429">XML Configuration Provider</span></span>

<span data-ttu-id="24940-430"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="24940-430">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="24940-431">XML dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-431">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24940-432">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="24940-432">Overloads permit specifying:</span></span>

* <span data-ttu-id="24940-433">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="24940-433">Whether the file is optional.</span></span>
* <span data-ttu-id="24940-434">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="24940-434">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="24940-435">Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="24940-435">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="24940-436">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="24940-436">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="24940-437">Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="24940-437">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="24940-438">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="24940-438">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="24940-439">XML yapılandırma dosyaları, yinelenen bölümler için farklı öğe adları kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="24940-439">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="24940-440">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="24940-440">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24940-441">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24940-441">section0:key0</span></span>
* <span data-ttu-id="24940-442">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-442">section0:key1</span></span>
* <span data-ttu-id="24940-443">section1:key0</span><span class="sxs-lookup"><span data-stu-id="24940-443">section1:key0</span></span>
* <span data-ttu-id="24940-444">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-444">section1:key1</span></span>

<span data-ttu-id="24940-445">Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:</span><span class="sxs-lookup"><span data-stu-id="24940-445">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="24940-446">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="24940-446">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24940-447">Bölüm: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="24940-447">section:section0:key:key0</span></span>
* <span data-ttu-id="24940-448">Bölüm: section0: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-448">section:section0:key:key1</span></span>
* <span data-ttu-id="24940-449">Bölüm: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="24940-449">section:section1:key:key0</span></span>
* <span data-ttu-id="24940-450">Bölüm: Section1: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-450">section:section1:key:key1</span></span>

<span data-ttu-id="24940-451">Öznitelikler, değerler sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="24940-451">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="24940-452">Önceki yapılandırma dosyası `value` aşağıdaki anahtarları yükler:</span><span class="sxs-lookup"><span data-stu-id="24940-452">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="24940-453">anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="24940-453">key:attribute</span></span>
* <span data-ttu-id="24940-454">Section: Key: özniteliği</span><span class="sxs-lookup"><span data-stu-id="24940-454">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="24940-455">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-455">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="24940-456"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="24940-456">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="24940-457">Anahtar, dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="24940-457">The key is the file name.</span></span> <span data-ttu-id="24940-458">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="24940-458">The value contains the file's contents.</span></span> <span data-ttu-id="24940-459">Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-459">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="24940-460">Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-460">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="24940-461">Dosyaların `directoryPath` mutlak bir yol olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="24940-461">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="24940-462">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="24940-462">Overloads permit specifying:</span></span>

* <span data-ttu-id="24940-463">Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.</span><span class="sxs-lookup"><span data-stu-id="24940-463">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="24940-464">Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="24940-464">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="24940-465">Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-465">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="24940-466">Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.</span><span class="sxs-lookup"><span data-stu-id="24940-466">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="24940-467">Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:</span><span class="sxs-lookup"><span data-stu-id="24940-467">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="24940-468">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-468">Memory Configuration Provider</span></span>

<span data-ttu-id="24940-469"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="24940-469">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="24940-470">Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı metodunu çağırın.</span><span class="sxs-lookup"><span data-stu-id="24940-470">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="24940-471">Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>` başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-471">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="24940-472">Uygulamanın yapılandırmasını belirtmek için Konağı derlerken `ConfigureAppConfiguration` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="24940-472">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="24940-473">Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="24940-473">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="24940-474">Sözlük, yapılandırmayı sağlamak için bir `AddInMemoryCollection` çağrısıyla kullanılır:</span><span class="sxs-lookup"><span data-stu-id="24940-474">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="24940-475">GetValue</span><span class="sxs-lookup"><span data-stu-id="24940-475">GetValue</span></span>

<span data-ttu-id="24940-476">[Configurationciltçi. GetValue \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen koleksiyon olmayan türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="24940-476">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="24940-477">Aşırı yükleme varsayılan bir değeri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="24940-477">An overload accepts a default value.</span></span>

<span data-ttu-id="24940-478">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="24940-478">The following example:</span></span>

* <span data-ttu-id="24940-479">Anahtar `NumberKey`, yapılandırmadan dize değerini ayıklar.</span><span class="sxs-lookup"><span data-stu-id="24940-479">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="24940-480">Yapılandırma anahtarlarında `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-480">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="24940-481">Değeri bir `int` olarak türler.</span><span class="sxs-lookup"><span data-stu-id="24940-481">Types the value as an `int`.</span></span>
* <span data-ttu-id="24940-482">Değeri, sayfanın kullanımı için `NumberConfig` özelliği içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="24940-482">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="24940-483">GetSection, GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="24940-483">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="24940-484">İzleyen örnekler için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="24940-484">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="24940-485">İki bölüm arasında dört anahtar bulunur ve bunlardan biri alt bölümleri çifti içerir:</span><span class="sxs-lookup"><span data-stu-id="24940-485">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="24940-486">Dosya yapılandırmaya okunduğu zaman yapılandırma değerlerini tutmak için aşağıdaki benzersiz hiyerarşik anahtarlar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="24940-486">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="24940-487">section0:key0</span><span class="sxs-lookup"><span data-stu-id="24940-487">section0:key0</span></span>
* <span data-ttu-id="24940-488">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-488">section0:key1</span></span>
* <span data-ttu-id="24940-489">section1:key0</span><span class="sxs-lookup"><span data-stu-id="24940-489">section1:key0</span></span>
* <span data-ttu-id="24940-490">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-490">section1:key1</span></span>
* <span data-ttu-id="24940-491">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="24940-491">section2:subsection0:key0</span></span>
* <span data-ttu-id="24940-492">section2: subsection0: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-492">section2:subsection0:key1</span></span>
* <span data-ttu-id="24940-493">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="24940-493">section2:subsection1:key0</span></span>
* <span data-ttu-id="24940-494">section2: subsection1: KEY1</span><span class="sxs-lookup"><span data-stu-id="24940-494">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="24940-495">GetSection</span><span class="sxs-lookup"><span data-stu-id="24940-495">GetSection</span></span>

<span data-ttu-id="24940-496">[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü ayıklar.</span><span class="sxs-lookup"><span data-stu-id="24940-496">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="24940-497">`section1`yalnızca anahtar-değer çiftlerini içeren bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> döndürmek için `GetSection` çağırın ve bölüm adını sağlayın:</span><span class="sxs-lookup"><span data-stu-id="24940-497">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="24940-498">`configSection` bir değer, yalnızca bir anahtar ve yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="24940-498">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="24940-499">Benzer şekilde, `section2:subsection0` anahtarlar için değerleri almak için, `GetSection` çağırın ve Bölüm yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="24940-499">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="24940-500">`GetSection` hiçbir şekilde `null` döndürmez.</span><span class="sxs-lookup"><span data-stu-id="24940-500">`GetSection` never returns `null`.</span></span> <span data-ttu-id="24940-501">Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="24940-501">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="24940-502">`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor.</span><span class="sxs-lookup"><span data-stu-id="24940-502">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="24940-503">Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="24940-503">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="24940-504">GetChildren</span><span class="sxs-lookup"><span data-stu-id="24940-504">GetChildren</span></span>

<span data-ttu-id="24940-505">`section2` üzerinde [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı, şunları içeren bir `IEnumerable<IConfigurationSection>` edinir:</span><span class="sxs-lookup"><span data-stu-id="24940-505">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="24940-506">Bulunur</span><span class="sxs-lookup"><span data-stu-id="24940-506">Exists</span></span>

<span data-ttu-id="24940-507">Bir yapılandırma bölümünün mevcut olup olmadığını anlamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) kullanın:</span><span class="sxs-lookup"><span data-stu-id="24940-507">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="24940-508">Örnek veriler verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölümü olmadığından `sectionExists` `false`.</span><span class="sxs-lookup"><span data-stu-id="24940-508">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="24940-509">Bir sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="24940-509">Bind to a class</span></span>

<span data-ttu-id="24940-510">Yapılandırma, *Seçenekler modelini*kullanarak ilgili ayarların gruplarını temsil eden sınıflara bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-510">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="24940-511">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="24940-511">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="24940-512">Yapılandırma değerleri dizeler olarak döndürülür, ancak <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> çağrısı [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesnelerinin oluşturulmasını mümkün yapar.</span><span class="sxs-lookup"><span data-stu-id="24940-512">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="24940-513">Örnek uygulama bir `Starship` modeli içerir (*modeller/Başlangıçgönder. cs*):</span><span class="sxs-lookup"><span data-stu-id="24940-513">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24940-514">*Starsevk. JSON* dosyasının `starship` bölümü, örnek uygulama yapılandırmayı yüklemek Için JSON yapılandırma sağlayıcısını kullandığında yapılandırmayı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="24940-514">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="24940-515">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="24940-515">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="24940-516">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-516">Key</span></span>                   | <span data-ttu-id="24940-517">Değer</span><span class="sxs-lookup"><span data-stu-id="24940-517">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="24940-518">starsevk: ad</span><span class="sxs-lookup"><span data-stu-id="24940-518">starship:name</span></span>         | <span data-ttu-id="24940-519">USS kurumsal</span><span class="sxs-lookup"><span data-stu-id="24940-519">USS Enterprise</span></span>                                    |
| <span data-ttu-id="24940-520">starsevk: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="24940-520">starship:registry</span></span>     | <span data-ttu-id="24940-521">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="24940-521">NCC-1701</span></span>                                          |
| <span data-ttu-id="24940-522">starsevk: sınıfı</span><span class="sxs-lookup"><span data-stu-id="24940-522">starship:class</span></span>        | <span data-ttu-id="24940-523">Anaytion</span><span class="sxs-lookup"><span data-stu-id="24940-523">Constitution</span></span>                                      |
| <span data-ttu-id="24940-524">başlangıçgönder: Uzunluk</span><span class="sxs-lookup"><span data-stu-id="24940-524">starship:length</span></span>       | <span data-ttu-id="24940-525">304,8</span><span class="sxs-lookup"><span data-stu-id="24940-525">304.8</span></span>                                             |
| <span data-ttu-id="24940-526">starsevkiyat: Commissioned</span><span class="sxs-lookup"><span data-stu-id="24940-526">starship:commissioned</span></span> | <span data-ttu-id="24940-527">False</span><span class="sxs-lookup"><span data-stu-id="24940-527">False</span></span>                                             |
| <span data-ttu-id="24940-528">dır</span><span class="sxs-lookup"><span data-stu-id="24940-528">trademark</span></span>             | <span data-ttu-id="24940-529">Paramount resimleri Corp.  https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="24940-529">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="24940-530">Örnek uygulama, `starship` anahtarıyla `GetSection` çağırır.</span><span class="sxs-lookup"><span data-stu-id="24940-530">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="24940-531">`starship` anahtar-değer çiftleri yalıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="24940-531">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="24940-532">`Bind` yöntemi, `Starship` sınıfının bir örneğinde geçen alt bölümde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="24940-532">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="24940-533">Örnek değerlerini bağladıktan sonra örnek, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="24940-533">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="24940-534">Bir nesne grafiğine bağlama</span><span class="sxs-lookup"><span data-stu-id="24940-534">Bind to an object graph</span></span>

<span data-ttu-id="24940-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, tüm POCO nesne grafiğini bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="24940-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="24940-536">Örnek, nesne grafı `Metadata` ve `Actors` sınıfları (*modeller/TvShow. cs*) içeren bir `TvShow` modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="24940-536">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24940-537">Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="24940-537">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="24940-538">Yapılandırma, `Bind` yöntemi ile `TvShow` nesne grafiğinin tamamına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="24940-538">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="24940-539">Bağlantılı örnek, işleme için bir özelliğe atandı:</span><span class="sxs-lookup"><span data-stu-id="24940-539">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="24940-540">[Configurationciltçi. Get \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="24940-540">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="24940-541">`Get<T>`, `Bind` kullanmaktan daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="24940-541">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="24940-542">Aşağıdaki kod, ilişkili örneğin işleme için kullanılan özelliğe doğrudan atanmasını sağlayan önceki örnekle `Get<T>` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="24940-542">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="24940-543">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="24940-543">Bind an array to a class</span></span>

<span data-ttu-id="24940-544">*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*</span><span class="sxs-lookup"><span data-stu-id="24940-544">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="24940-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="24940-545">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="24940-546">Sayısal anahtar segmentini (`:0:`, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir POCO sınıf dizisine dizi bağlama özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="24940-546">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="24940-547">Bağlama, kural tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="24940-547">Binding is provided by convention.</span></span> <span data-ttu-id="24940-548">Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="24940-548">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="24940-549">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="24940-549">**In-memory array processing**</span></span>

<span data-ttu-id="24940-550">Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="24940-550">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="24940-551">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-551">Key</span></span>             | <span data-ttu-id="24940-552">Değer</span><span class="sxs-lookup"><span data-stu-id="24940-552">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="24940-553">dizi: girdiler: 0</span><span class="sxs-lookup"><span data-stu-id="24940-553">array:entries:0</span></span> | <span data-ttu-id="24940-554">value0</span><span class="sxs-lookup"><span data-stu-id="24940-554">value0</span></span> |
| <span data-ttu-id="24940-555">dizi: girdiler: 1</span><span class="sxs-lookup"><span data-stu-id="24940-555">array:entries:1</span></span> | <span data-ttu-id="24940-556">value1</span><span class="sxs-lookup"><span data-stu-id="24940-556">value1</span></span> |
| <span data-ttu-id="24940-557">dizi: girdiler: 2</span><span class="sxs-lookup"><span data-stu-id="24940-557">array:entries:2</span></span> | <span data-ttu-id="24940-558">value2</span><span class="sxs-lookup"><span data-stu-id="24940-558">value2</span></span> |
| <span data-ttu-id="24940-559">dizi: girdiler: 4</span><span class="sxs-lookup"><span data-stu-id="24940-559">array:entries:4</span></span> | <span data-ttu-id="24940-560">value4</span><span class="sxs-lookup"><span data-stu-id="24940-560">value4</span></span> |
| <span data-ttu-id="24940-561">dizi: girdiler: 5</span><span class="sxs-lookup"><span data-stu-id="24940-561">array:entries:5</span></span> | <span data-ttu-id="24940-562">value5</span><span class="sxs-lookup"><span data-stu-id="24940-562">value5</span></span> |

<span data-ttu-id="24940-563">Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="24940-563">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="24940-564">Dizi, Dizin &num;3 için bir değer atlar.</span><span class="sxs-lookup"><span data-stu-id="24940-564">The array skips a value for index &num;3.</span></span> <span data-ttu-id="24940-565">Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="24940-565">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="24940-566">Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="24940-566">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24940-567">Yapılandırma verileri nesnesine bağlanır:</span><span class="sxs-lookup"><span data-stu-id="24940-567">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="24940-568">[Configurationciltçi. Get \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="24940-568">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="24940-569">`ArrayExample`bir örneği olan bağlantılı nesne, yapılandırmadan dizi verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="24940-569">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="24940-570">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="24940-570">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="24940-571">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="24940-571">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="24940-572">0</span><span class="sxs-lookup"><span data-stu-id="24940-572">0</span></span>                            | <span data-ttu-id="24940-573">value0</span><span class="sxs-lookup"><span data-stu-id="24940-573">value0</span></span>                       |
| <span data-ttu-id="24940-574">1\.</span><span class="sxs-lookup"><span data-stu-id="24940-574">1</span></span>                            | <span data-ttu-id="24940-575">value1</span><span class="sxs-lookup"><span data-stu-id="24940-575">value1</span></span>                       |
| <span data-ttu-id="24940-576">2</span><span class="sxs-lookup"><span data-stu-id="24940-576">2</span></span>                            | <span data-ttu-id="24940-577">value2</span><span class="sxs-lookup"><span data-stu-id="24940-577">value2</span></span>                       |
| <span data-ttu-id="24940-578">3</span><span class="sxs-lookup"><span data-stu-id="24940-578">3</span></span>                            | <span data-ttu-id="24940-579">value4</span><span class="sxs-lookup"><span data-stu-id="24940-579">value4</span></span>                       |
| <span data-ttu-id="24940-580">4</span><span class="sxs-lookup"><span data-stu-id="24940-580">4</span></span>                            | <span data-ttu-id="24940-581">value5</span><span class="sxs-lookup"><span data-stu-id="24940-581">value5</span></span>                       |

<span data-ttu-id="24940-582">İlişkili nesnedeki Dizin &num;3, `array:4` yapılandırma anahtarı ve `value4` değeri için yapılandırma verilerini tutar.</span><span class="sxs-lookup"><span data-stu-id="24940-582">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="24940-583">Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-583">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="24940-584">Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="24940-584">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="24940-585">Yapılandırmada doğru anahtar-değer çiftini üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce dizin &num;3 için eksik yapılandırma öğesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="24940-585">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="24940-586">Örnek, eksik anahtar-değer çiftine sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, `ArrayExample.Entries` tam yapılandırma dizisiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="24940-586">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="24940-587">*missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="24940-587">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="24940-588">`ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="24940-588">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="24940-589">Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.</span><span class="sxs-lookup"><span data-stu-id="24940-589">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="24940-590">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-590">Key</span></span>             | <span data-ttu-id="24940-591">Değer</span><span class="sxs-lookup"><span data-stu-id="24940-591">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="24940-592">dizi: girdiler: 3</span><span class="sxs-lookup"><span data-stu-id="24940-592">array:entries:3</span></span> | <span data-ttu-id="24940-593">value3</span><span class="sxs-lookup"><span data-stu-id="24940-593">value3</span></span> |

<span data-ttu-id="24940-594">`ArrayExample` sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ün girdisini içeriyorsa, `ArrayExample.Entries` dizisi değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="24940-594">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="24940-595">`ArrayExample.Entries` dizini</span><span class="sxs-lookup"><span data-stu-id="24940-595">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="24940-596">`ArrayExample.Entries` değeri</span><span class="sxs-lookup"><span data-stu-id="24940-596">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="24940-597">0</span><span class="sxs-lookup"><span data-stu-id="24940-597">0</span></span>                            | <span data-ttu-id="24940-598">value0</span><span class="sxs-lookup"><span data-stu-id="24940-598">value0</span></span>                       |
| <span data-ttu-id="24940-599">1\.</span><span class="sxs-lookup"><span data-stu-id="24940-599">1</span></span>                            | <span data-ttu-id="24940-600">value1</span><span class="sxs-lookup"><span data-stu-id="24940-600">value1</span></span>                       |
| <span data-ttu-id="24940-601">2</span><span class="sxs-lookup"><span data-stu-id="24940-601">2</span></span>                            | <span data-ttu-id="24940-602">value2</span><span class="sxs-lookup"><span data-stu-id="24940-602">value2</span></span>                       |
| <span data-ttu-id="24940-603">3</span><span class="sxs-lookup"><span data-stu-id="24940-603">3</span></span>                            | <span data-ttu-id="24940-604">value3</span><span class="sxs-lookup"><span data-stu-id="24940-604">value3</span></span>                       |
| <span data-ttu-id="24940-605">4</span><span class="sxs-lookup"><span data-stu-id="24940-605">4</span></span>                            | <span data-ttu-id="24940-606">value4</span><span class="sxs-lookup"><span data-stu-id="24940-606">value4</span></span>                       |
| <span data-ttu-id="24940-607">5</span><span class="sxs-lookup"><span data-stu-id="24940-607">5</span></span>                            | <span data-ttu-id="24940-608">value5</span><span class="sxs-lookup"><span data-stu-id="24940-608">value5</span></span>                       |

<span data-ttu-id="24940-609">**JSON dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="24940-609">**JSON array processing**</span></span>

<span data-ttu-id="24940-610">JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="24940-610">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="24940-611">Aşağıdaki yapılandırma dosyasında, `subsection` bir dizidir:</span><span class="sxs-lookup"><span data-stu-id="24940-611">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="24940-612">JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="24940-612">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="24940-613">Anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-613">Key</span></span>                     | <span data-ttu-id="24940-614">Değer</span><span class="sxs-lookup"><span data-stu-id="24940-614">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="24940-615">json_array: anahtar</span><span class="sxs-lookup"><span data-stu-id="24940-615">json_array:key</span></span>          | <span data-ttu-id="24940-616">değer EA</span><span class="sxs-lookup"><span data-stu-id="24940-616">valueA</span></span> |
| <span data-ttu-id="24940-617">json_array: alt bölüm: 0</span><span class="sxs-lookup"><span data-stu-id="24940-617">json_array:subsection:0</span></span> | <span data-ttu-id="24940-618">valueB</span><span class="sxs-lookup"><span data-stu-id="24940-618">valueB</span></span> |
| <span data-ttu-id="24940-619">json_array: alt bölüm: 1</span><span class="sxs-lookup"><span data-stu-id="24940-619">json_array:subsection:1</span></span> | <span data-ttu-id="24940-620">değer EC</span><span class="sxs-lookup"><span data-stu-id="24940-620">valueC</span></span> |
| <span data-ttu-id="24940-621">json_array: alt bölüm: 2</span><span class="sxs-lookup"><span data-stu-id="24940-621">json_array:subsection:2</span></span> | <span data-ttu-id="24940-622">Değerler</span><span class="sxs-lookup"><span data-stu-id="24940-622">valueD</span></span> |

<span data-ttu-id="24940-623">Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="24940-623">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="24940-624">Bağlamadan sonra, `JsonArrayExample.Key` `valueA` değerini tutar.</span><span class="sxs-lookup"><span data-stu-id="24940-624">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="24940-625">Alt bölüm değerleri, `Subsection` POCO dizisi özelliğinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="24940-625">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="24940-626">`JsonArrayExample.Subsection` dizini</span><span class="sxs-lookup"><span data-stu-id="24940-626">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="24940-627">`JsonArrayExample.Subsection` değeri</span><span class="sxs-lookup"><span data-stu-id="24940-627">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="24940-628">0</span><span class="sxs-lookup"><span data-stu-id="24940-628">0</span></span>                                   | <span data-ttu-id="24940-629">valueB</span><span class="sxs-lookup"><span data-stu-id="24940-629">valueB</span></span>                              |
| <span data-ttu-id="24940-630">1\.</span><span class="sxs-lookup"><span data-stu-id="24940-630">1</span></span>                                   | <span data-ttu-id="24940-631">değer EC</span><span class="sxs-lookup"><span data-stu-id="24940-631">valueC</span></span>                              |
| <span data-ttu-id="24940-632">2</span><span class="sxs-lookup"><span data-stu-id="24940-632">2</span></span>                                   | <span data-ttu-id="24940-633">Değerler</span><span class="sxs-lookup"><span data-stu-id="24940-633">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="24940-634">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="24940-634">Custom configuration provider</span></span>

<span data-ttu-id="24940-635">Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="24940-635">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="24940-636">Sağlayıcı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="24940-636">The provider has the following characteristics:</span></span>

* <span data-ttu-id="24940-637">EF bellek içi veritabanı, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="24940-637">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="24940-638">Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.</span><span class="sxs-lookup"><span data-stu-id="24940-638">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="24940-639">Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur.</span><span class="sxs-lookup"><span data-stu-id="24940-639">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="24940-640">Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.</span><span class="sxs-lookup"><span data-stu-id="24940-640">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="24940-641">Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="24940-641">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="24940-642">Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="24940-642">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="24940-643">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-643">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="24940-644">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-644">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="24940-645">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-645">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="24940-646"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24940-646">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="24940-647">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-647">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="24940-648"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24940-648">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="24940-649">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="24940-649">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="24940-650">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-650">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="24940-651">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="24940-651">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="24940-652">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-652">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="24940-653">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="24940-653">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="24940-654">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-654">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="24940-655">Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-655">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="24940-656">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-656">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="24940-657"><xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24940-657">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="24940-658">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-658">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="24940-659"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="24940-659">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="24940-660">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="24940-660">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="24940-661">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-661">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="24940-662">`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="24940-662">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="24940-663">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="24940-663">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="24940-664">Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="24940-664">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="24940-665">Başlangıç sırasında yapılandırmaya erişim</span><span class="sxs-lookup"><span data-stu-id="24940-665">Access configuration during startup</span></span>

<span data-ttu-id="24940-666">`Startup.ConfigureServices`yapılandırma değerlerine erişmek için `Startup` oluşturucusuna `IConfiguration` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-666">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="24940-667">`Startup.Configure`yapılandırmaya erişmek için `IConfiguration` doğrudan yönteme ekleyin ya da oluşturucuyu kullanarak örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="24940-667">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="24940-668">Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. [uygulama başlatma: kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="24940-668">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="24940-669">Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="24940-669">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="24940-670">Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfa ya da görünüme <xref:Microsoft.Extensions.Configuration.IConfiguration> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="24940-670">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="24940-671">Razor Pages sayfasında:</span><span class="sxs-lookup"><span data-stu-id="24940-671">In a Razor Pages page:</span></span>

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

<span data-ttu-id="24940-672">MVC görünümünde:</span><span class="sxs-lookup"><span data-stu-id="24940-672">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="24940-673">Bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="24940-673">Add configuration from an external assembly</span></span>

<span data-ttu-id="24940-674"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="24940-674">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="24940-675">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="24940-675">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24940-676">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="24940-676">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
