---
title: ASP.NET Core yapılandırma
author: guardrex
description: ASP.NET Core uygulamasını yapılandırmak için yapılandırma API 'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 0de2222e8072523ff0e5d261a9fe5ef8eb9a7606
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081815"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="661ad-103">ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="661ad-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="661ad-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="661ad-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="661ad-105">ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır.</span><span class="sxs-lookup"><span data-stu-id="661ad-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="661ad-106">Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="661ad-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="661ad-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="661ad-107">Azure Key Vault</span></span>
* <span data-ttu-id="661ad-108">Azure Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="661ad-108">Azure App Configuration</span></span>
* <span data-ttu-id="661ad-109">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="661ad-109">Command-line arguments</span></span>
* <span data-ttu-id="661ad-110">Özel sağlayıcılar (yüklü veya oluşturulmuş)</span><span class="sxs-lookup"><span data-stu-id="661ad-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="661ad-111">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="661ad-111">Directory files</span></span>
* <span data-ttu-id="661ad-112">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="661ad-112">Environment variables</span></span>
* <span data-ttu-id="661ad-113">Bellek içi .NET nesneleri</span><span class="sxs-lookup"><span data-stu-id="661ad-113">In-memory .NET objects</span></span>
* <span data-ttu-id="661ad-114">Ayarlar dosyaları</span><span class="sxs-lookup"><span data-stu-id="661ad-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="661ad-115">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), Framework tarafından örtük olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="661ad-116">Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="661ad-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="661ad-117">Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="661ad-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="661ad-118">*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="661ad-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="661ad-119">Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="661ad-120">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="661ad-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="661ad-121">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="661ad-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="661ad-122">Uygulama yapılandırmasına karşı konak</span><span class="sxs-lookup"><span data-stu-id="661ad-122">Host versus app configuration</span></span>

<span data-ttu-id="661ad-123">Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="661ad-124">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="661ad-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="661ad-125">Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="661ad-126">Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir.</span><span class="sxs-lookup"><span data-stu-id="661ad-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="661ad-127">Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="661ad-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="661ad-128">Varsayılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="661ad-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="661ad-129">Ana bilgisayar oluştururken ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlar çağrısı <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> tabanlı Web Apps.</span><span class="sxs-lookup"><span data-stu-id="661ad-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="661ad-130">`CreateDefaultBuilder`uygulama için varsayılan yapılandırmayı aşağıdaki sırayla sağlar:</span><span class="sxs-lookup"><span data-stu-id="661ad-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="661ad-131">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="661ad-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="661ad-132">[Web konağını](xref:fundamentals/host/web-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [bu konunun ASP.NET Core 2,2 sürümüne](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="661ad-133">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="661ad-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="661ad-134">Ortam değişkenleri `DOTNET_` [yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak (örneğin, `DOTNET_ENVIRONMENT`) ön eki olan ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="661ad-135">Yapılandırma anahtar-`DOTNET_`değer çiftleri yüklendiğinde önek () çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="661ad-136">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="661ad-137">Web ana bilgisayar varsayılan yapılandırması oluşturuldu (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="661ad-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="661ad-138">Kestrel, Web sunucusu olarak kullanılır ve uygulamanın yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="661ad-139">Konak filtreleme ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="661ad-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="661ad-140">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` Ortam değişkeni olarak `true`ayarlanmışsa iletilen üstbilgiler ara yazılımı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="661ad-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="661ad-141">IIS tümleştirmesini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="661ad-141">Enable IIS integration.</span></span>
* <span data-ttu-id="661ad-142">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="661ad-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="661ad-143">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="661ad-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="661ad-144">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="661ad-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="661ad-145">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="661ad-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="661ad-146">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="661ad-147">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="661ad-148">Ana bilgisayar oluştururken ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlar çağrısı <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tabanlı Web Apps.</span><span class="sxs-lookup"><span data-stu-id="661ad-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="661ad-149">`CreateDefaultBuilder`uygulama için varsayılan yapılandırmayı aşağıdaki sırayla sağlar:</span><span class="sxs-lookup"><span data-stu-id="661ad-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="661ad-150">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanan uygulamalar için aşağıdakiler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="661ad-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="661ad-151">[Genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [Bu konunun en son sürümüne](xref:fundamentals/configuration/index)bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="661ad-152">Ana bilgisayar yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="661ad-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="661ad-153">Ortam değişkenleri `ASPNETCORE_` [yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak (örneğin, `ASPNETCORE_ENVIRONMENT`) ön eki olan ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="661ad-154">Yapılandırma anahtar-`ASPNETCORE_`değer çiftleri yüklendiğinde önek () çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="661ad-155">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="661ad-156">Uygulama yapılandırması şuradan sağlanır:</span><span class="sxs-lookup"><span data-stu-id="661ad-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="661ad-157">[dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="661ad-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="661ad-158">*appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.</span><span class="sxs-lookup"><span data-stu-id="661ad-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="661ad-159">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="661ad-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="661ad-160">Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="661ad-161">Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="661ad-162">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="661ad-162">Security</span></span>

<span data-ttu-id="661ad-163">Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="661ad-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="661ad-164">Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="661ad-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="661ad-165">Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="661ad-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="661ad-166">Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.</span><span class="sxs-lookup"><span data-stu-id="661ad-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="661ad-167">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="661ad-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="661ad-168"><xref:security/app-secrets>&ndash; Hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir.</span><span class="sxs-lookup"><span data-stu-id="661ad-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="661ad-169">Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="661ad-170">Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="661ad-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="661ad-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar.</span><span class="sxs-lookup"><span data-stu-id="661ad-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="661ad-172">Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="661ad-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="661ad-173">Hiyerarşik yapılandırma verileri</span><span class="sxs-lookup"><span data-stu-id="661ad-173">Hierarchical configuration data</span></span>

<span data-ttu-id="661ad-174">Yapılandırma API 'SI, hiyerarşik verileri yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini muhafaza ediyor.</span><span class="sxs-lookup"><span data-stu-id="661ad-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="661ad-175">Aşağıdaki JSON dosyasında, iki bölümün yapılandırılmış hiyerarşisinde dört anahtar mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="661ad-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="661ad-176">Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="661ad-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="661ad-177">Özgün yapıyı sürdürmek için bölümler ve anahtarlar iki nokta (`:`) kullanımıyla düzleştirilir:</span><span class="sxs-lookup"><span data-stu-id="661ad-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="661ad-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-178">section0:key0</span></span>
* <span data-ttu-id="661ad-179">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-179">section0:key1</span></span>
* <span data-ttu-id="661ad-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-180">section1:key0</span></span>
* <span data-ttu-id="661ad-181">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-181">section1:key1</span></span>

<span data-ttu-id="661ad-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yapılandırma verileri bölümünün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="661ad-183">Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="661ad-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="661ad-184">Kurallar</span><span class="sxs-lookup"><span data-stu-id="661ad-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="661ad-185">Kaynaklar ve sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="661ad-185">Sources and providers</span></span>

<span data-ttu-id="661ad-186">Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="661ad-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="661ad-187">Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="661ad-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="661ad-188">Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.</span><span class="sxs-lookup"><span data-stu-id="661ad-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="661ad-189"><xref:Microsoft.Extensions.Configuration.IConfiguration>uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="661ad-190"><xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfının yapılandırmasını elde etmek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> bir Razor Pages eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="661ad-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="661ad-191">Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="661ad-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="661ad-192">Belirlenmesine</span><span class="sxs-lookup"><span data-stu-id="661ad-192">Keys</span></span>

<span data-ttu-id="661ad-193">Yapılandırma anahtarları aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="661ad-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="661ad-194">Anahtarlar büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="661ad-194">Keys are case-insensitive.</span></span> <span data-ttu-id="661ad-195">Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="661ad-196">Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="661ad-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="661ad-197">Hiyerarşik anahtarlar</span><span class="sxs-lookup"><span data-stu-id="661ad-197">Hierarchical keys</span></span>
  * <span data-ttu-id="661ad-198">Yapılandırma API 'si içinde, iki nokta üst üste`:`ayırıcı () tüm platformlarda çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="661ad-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="661ad-199">Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="661ad-200">Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="661ad-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="661ad-201">Azure Key Vault, hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="661ad-202">Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tirelerin yerini iki nokta ile değiştirmek için kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="661ad-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="661ad-203">, <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Yapılandırma anahtarlarındaki dizi dizinlerini kullanarak nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="661ad-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="661ad-204">Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="661ad-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="661ad-205">Değerler</span><span class="sxs-lookup"><span data-stu-id="661ad-205">Values</span></span>

<span data-ttu-id="661ad-206">Yapılandırma değerleri aşağıdaki kuralları benimseyin:</span><span class="sxs-lookup"><span data-stu-id="661ad-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="661ad-207">Değerler dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="661ad-207">Values are strings.</span></span>
* <span data-ttu-id="661ad-208">Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="661ad-209">sağlayıcılarla</span><span class="sxs-lookup"><span data-stu-id="661ad-209">Providers</span></span>

<span data-ttu-id="661ad-210">Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="661ad-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="661ad-211">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="661ad-211">Provider</span></span> | <span data-ttu-id="661ad-212">Şuradan yapılandırma sağlar&hellip;</span><span class="sxs-lookup"><span data-stu-id="661ad-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="661ad-213">[Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*Güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="661ad-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="661ad-214">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="661ad-214">Azure Key Vault</span></span> |
| <span data-ttu-id="661ad-215">[Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri)</span><span class="sxs-lookup"><span data-stu-id="661ad-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="661ad-216">Azure Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="661ad-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="661ad-217">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="661ad-218">Komut satırı parametreleri</span><span class="sxs-lookup"><span data-stu-id="661ad-218">Command-line parameters</span></span> |
| [<span data-ttu-id="661ad-219">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="661ad-220">Özel kaynak</span><span class="sxs-lookup"><span data-stu-id="661ad-220">Custom source</span></span> |
| [<span data-ttu-id="661ad-221">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="661ad-222">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="661ad-222">Environment variables</span></span> |
| [<span data-ttu-id="661ad-223">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="661ad-224">Dosyalar (ıNı, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="661ad-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="661ad-225">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="661ad-226">Dizin dosyaları</span><span class="sxs-lookup"><span data-stu-id="661ad-226">Directory files</span></span> |
| [<span data-ttu-id="661ad-227">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="661ad-228">Bellek içi Koleksiyonlar</span><span class="sxs-lookup"><span data-stu-id="661ad-228">In-memory collections</span></span> |
| <span data-ttu-id="661ad-229">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*Güvenlik* konuları)</span><span class="sxs-lookup"><span data-stu-id="661ad-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="661ad-230">Kullanıcı profili dizinindeki dosya</span><span class="sxs-lookup"><span data-stu-id="661ad-230">File in the user profile directory</span></span> |

<span data-ttu-id="661ad-231">Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.</span><span class="sxs-lookup"><span data-stu-id="661ad-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="661ad-232">Bu konuda açıklanan yapılandırma sağlayıcıları, kodunuzun onları düzenleyebilir sırada değil alfabetik sırayla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="661ad-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="661ad-233">Kodunuzda yapılandırma sağlayıcılarını, temeldeki yapılandırma kaynakları için önceliklerinize uyacak şekilde sıralayın.</span><span class="sxs-lookup"><span data-stu-id="661ad-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="661ad-234">Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:</span><span class="sxs-lookup"><span data-stu-id="661ad-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="661ad-235">Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır</span><span class="sxs-lookup"><span data-stu-id="661ad-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="661ad-236">Azure Anahtar Kasası.</span><span class="sxs-lookup"><span data-stu-id="661ad-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="661ad-237">[Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (Yalnızca geliştirme ortamı)</span><span class="sxs-lookup"><span data-stu-id="661ad-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="661ad-238">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="661ad-238">Environment variables</span></span>
1. <span data-ttu-id="661ad-239">Komut satırı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="661ad-239">Command-line arguments</span></span>

<span data-ttu-id="661ad-240">Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="661ad-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="661ad-241">Önceki sağlayıcı dizisi, ile `CreateDefaultBuilder`yeni bir konak Oluşturucu başlattığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="661ad-242">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="661ad-243">Konak oluşturucuyu ConfigureHostConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="661ad-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="661ad-244">Konak oluşturucuyu yapılandırmak için, yapılandırmayı çağırın <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> ve sağlayın.</span><span class="sxs-lookup"><span data-stu-id="661ad-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="661ad-245">`ConfigureHostConfiguration`, <xref:Microsoft.Extensions.Hosting.IHostEnvironment> derleme sürecinde daha sonra kullanılmak üzere başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="661ad-246">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration((hostingContext, config) =>
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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="661ad-247">Konak oluşturucuyu UseConfiguration ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="661ad-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="661ad-248">Konak oluşturucuyu yapılandırmak için, yapılandırma ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> konak Oluşturucu üzerinde çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="661ad-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="661ad-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="661ad-250">Ana `ConfigureAppConfiguration` bilgisayarı oluşturma sırasında `CreateDefaultBuilder`otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirtmek için çağırın:</span><span class="sxs-lookup"><span data-stu-id="661ad-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="661ad-251">Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="661ad-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="661ad-252">Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için, en son ' `AddCommandLine` u çağırın:</span><span class="sxs-lookup"><span data-stu-id="661ad-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="661ad-253">Uygulama başlatma sırasında yapılandırmayı kullan</span><span class="sxs-lookup"><span data-stu-id="661ad-253">Consume configuration during app startup</span></span>

<span data-ttu-id="661ad-254">Uygulamasında `ConfigureAppConfiguration` uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında, dahil olmak üzere `Startup.ConfigureServices`kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-254">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="661ad-255">Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-255">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="661ad-256">Komut satırı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-256">Command-line Configuration Provider</span></span>

<span data-ttu-id="661ad-257">, <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Çalışma zamanında anahtar-değer çiftinden yapılandırma yükler.</span><span class="sxs-lookup"><span data-stu-id="661ad-257">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="661ad-258">Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-258">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="661ad-259">`AddCommandLine`çağrıldığında otomatik olarak çağrılır `CreateDefaultBuilder(string [])` .</span><span class="sxs-lookup"><span data-stu-id="661ad-259">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="661ad-260">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-260">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="661ad-261">`CreateDefaultBuilder`Ayrıca yüklenir:</span><span class="sxs-lookup"><span data-stu-id="661ad-261">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="661ad-262">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="661ad-262">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="661ad-263">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="661ad-263">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="661ad-264">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-264">Environment variables.</span></span>

<span data-ttu-id="661ad-265">`CreateDefaultBuilder`Komut satırı yapılandırma sağlayıcısını son ekler.</span><span class="sxs-lookup"><span data-stu-id="661ad-265">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="661ad-266">Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.</span><span class="sxs-lookup"><span data-stu-id="661ad-266">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="661ad-267">`CreateDefaultBuilder`Ana bilgisayar oluşturulduğunda davranır.</span><span class="sxs-lookup"><span data-stu-id="661ad-267">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="661ad-268">Bu nedenle, tarafından `CreateDefaultBuilder` etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-268">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="661ad-269">ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` tarafından `CreateDefaultBuilder`zaten çağırılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-269">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="661ad-270">Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için uygulamanın ek sağlayıcılarını `ConfigureAppConfiguration` çağırın ve en son çağrısı `AddCommandLine` yapın.</span><span class="sxs-lookup"><span data-stu-id="661ad-270">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="661ad-271">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="661ad-271">**Example**</span></span>

<span data-ttu-id="661ad-272">Örnek uygulama, ' a `CreateDefaultBuilder` <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>çağrı içeren konağı oluşturmak için statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-272">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="661ad-273">Projenin dizininde bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="661ad-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="661ad-274">`dotnet run` Komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="661ad-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="661ad-275">Uygulama çalıştırıldıktan sonra, konumundaki `http://localhost:5000`uygulamaya bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="661ad-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="661ad-276">Çıktının ' de belirtilen `dotnet run`yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="661ad-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="661ad-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="661ad-277">Arguments</span></span>

<span data-ttu-id="661ad-278">Değer bir eşittir işaretini (`=`) izlemelidir veya değer bir boşluk izleyen anahtar bir ön eke (`--` ya da `/`) sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="661ad-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="661ad-279">Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="661ad-279">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="661ad-280">Anahtar ön eki</span><span class="sxs-lookup"><span data-stu-id="661ad-280">Key prefix</span></span>               | <span data-ttu-id="661ad-281">Örnek</span><span class="sxs-lookup"><span data-stu-id="661ad-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="661ad-282">Ön ek yok</span><span class="sxs-lookup"><span data-stu-id="661ad-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="661ad-283">İki kısa çizgi`--`()</span><span class="sxs-lookup"><span data-stu-id="661ad-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="661ad-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="661ad-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="661ad-285">Eğik çizgi (`/`)</span><span class="sxs-lookup"><span data-stu-id="661ad-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="661ad-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="661ad-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="661ad-287">Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="661ad-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="661ad-288">Örnek komutlar:</span><span class="sxs-lookup"><span data-stu-id="661ad-288">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="661ad-289">Eşleme Değiştir</span><span class="sxs-lookup"><span data-stu-id="661ad-289">Switch mappings</span></span>

<span data-ttu-id="661ad-290">Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir.</span><span class="sxs-lookup"><span data-stu-id="661ad-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="661ad-291">İle <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>yapılandırmayı el ile oluşturduğunuzda, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemine anahtar değiştirmeler sözlüğü sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="661ad-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="661ad-292">Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="661ad-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="661ad-293">Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="661ad-294">Tek tireyle (`-`) ön eki eklenmiş herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="661ad-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="661ad-295">Anahtar eşlemeleri sözlük anahtarı kuralları:</span><span class="sxs-lookup"><span data-stu-id="661ad-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="661ad-296">Anahtarlar bir tire (`-`) veya çift tireyle (`--`) başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="661ad-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="661ad-297">Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="661ad-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="661ad-298">Anahtar eşlemeleri sözlüğü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="661ad-298">Create a switch mappings dictionary.</span></span> <span data-ttu-id="661ad-299">Aşağıdaki örnekte, iki anahtar eşlemesi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="661ad-299">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="661ad-300">Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne çağırın `AddCommandLine` :</span><span class="sxs-lookup"><span data-stu-id="661ad-300">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="661ad-301">Anahtar eşlemeleri kullanan uygulamalar için, ' ye `CreateDefaultBuilder` yapılan çağrı bağımsız değişkenleri geçirmez.</span><span class="sxs-lookup"><span data-stu-id="661ad-301">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="661ad-302">Yöntemin çağrısı eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü öğesine `CreateDefaultBuilder`geçirmenin bir yolu yoktur. `AddCommandLine` `CreateDefaultBuilder`</span><span class="sxs-lookup"><span data-stu-id="661ad-302">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="661ad-303">Çözüm, bağımsız değişkenleri öğesine `CreateDefaultBuilder` geçirmektir, bunun yerine `ConfigurationBuilder` metodun `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="661ad-303">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="661ad-304">Anahtar eşlemeleri sözlüğü oluşturulduktan sonra, aşağıdaki tabloda gösterilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="661ad-304">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="661ad-305">Anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-305">Key</span></span>       | <span data-ttu-id="661ad-306">Değer</span><span class="sxs-lookup"><span data-stu-id="661ad-306">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="661ad-307">Uygulama başlatılırken anahtar eşlenmiş anahtarlar kullanılıyorsa, yapılandırma sözlük tarafından sağlanan anahtardaki yapılandırma değerini alır:</span><span class="sxs-lookup"><span data-stu-id="661ad-307">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="661ad-308">Önceki komutu çalıştırdıktan sonra, yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="661ad-308">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="661ad-309">Anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-309">Key</span></span>               | <span data-ttu-id="661ad-310">Değer</span><span class="sxs-lookup"><span data-stu-id="661ad-310">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="661ad-311">Ortam değişkenleri yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-311">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="661ad-312">, <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="661ad-312">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="661ad-313">Ortam değişkenleri yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-313">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="661ad-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceğiniz ortam değişkenlerini ayarlamanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="661ad-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="661ad-315">Daha fazla bilgi için bkz [. Azure uygulamaları: Azure portalını](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)kullanarak uygulama yapılandırmasını geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="661ad-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="661ad-316">`AddEnvironmentVariables`, [genel konakla](xref:fundamentals/host/generic-host) yeni bir ana bilgisayar Oluşturucu `DOTNET_` başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için önekli ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-316">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="661ad-317">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-317">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="661ad-318">`AddEnvironmentVariables`, [Web ana](xref:fundamentals/host/web-host) bilgisayarıyla yeni bir konak Oluşturucu `ASPNETCORE_` başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için önekli ortam değişkenlerini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="661ad-319">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="661ad-320">`CreateDefaultBuilder`Ayrıca yüklenir:</span><span class="sxs-lookup"><span data-stu-id="661ad-320">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="661ad-321">Önek olmadan çağırarak `AddEnvironmentVariables` ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="661ad-321">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="661ad-322">*AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="661ad-322">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="661ad-323">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="661ad-323">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="661ad-324">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-324">Command-line arguments.</span></span>

<span data-ttu-id="661ad-325">Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-325">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="661ad-326">Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="661ad-326">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="661ad-327">Ek ortam değişkenlerinden uygulama yapılandırması sağlamanız gerekiyorsa, uygulamasının uygulamasındaki `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle çağırın. `AddEnvironmentVariables`</span><span class="sxs-lookup"><span data-stu-id="661ad-327">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="661ad-328">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="661ad-328">**Example**</span></span>

<span data-ttu-id="661ad-329">Örnek uygulama, ' a `CreateDefaultBuilder` `AddEnvironmentVariables`çağrı içeren konağı oluşturmak için statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-329">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="661ad-330">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-330">Run the sample app.</span></span> <span data-ttu-id="661ad-331">Üzerinde `http://localhost:5000`uygulama için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="661ad-331">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="661ad-332">Çıktının ortam değişkeni `ENVIRONMENT`için anahtar-değer çiftini içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="661ad-332">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="661ad-333">Değer, genellikle `Development` yerel olarak çalışırken uygulamanın çalıştığı ortamı yansıtır.</span><span class="sxs-lookup"><span data-stu-id="661ad-333">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="661ad-334">Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler.</span><span class="sxs-lookup"><span data-stu-id="661ad-334">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="661ad-335">Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-335">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="661ad-336">Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek istiyorsanız, `FilteredConfiguration` *Pages/Index. cshtml. cs* ' deki ' ı şu şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="661ad-336">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="661ad-337">Ön Ekler</span><span class="sxs-lookup"><span data-stu-id="661ad-337">Prefixes</span></span>

<span data-ttu-id="661ad-338">Uygulamanın yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağladığınız zaman filtrelenir.</span><span class="sxs-lookup"><span data-stu-id="661ad-338">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="661ad-339">Örneğin, önekte `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:</span><span class="sxs-lookup"><span data-stu-id="661ad-339">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="661ad-340">Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-340">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="661ad-341">Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-341">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="661ad-342">Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-342">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="661ad-343">**Bağlantı dizesi önekleri**</span><span class="sxs-lookup"><span data-stu-id="661ad-343">**Connection string prefixes**</span></span>

<span data-ttu-id="661ad-344">Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir.</span><span class="sxs-lookup"><span data-stu-id="661ad-344">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="661ad-345">Üzerinde bir önek sağlanmazsa `AddEnvironmentVariables`, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.</span><span class="sxs-lookup"><span data-stu-id="661ad-345">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="661ad-346">Bağlantı dizesi öneki</span><span class="sxs-lookup"><span data-stu-id="661ad-346">Connection string prefix</span></span> | <span data-ttu-id="661ad-347">Sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="661ad-347">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="661ad-348">Özel sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="661ad-348">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="661ad-349">MySQL</span><span class="sxs-lookup"><span data-stu-id="661ad-349">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="661ad-350">Azure SQL Veritabanı</span><span class="sxs-lookup"><span data-stu-id="661ad-350">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="661ad-351">SQL Server</span><span class="sxs-lookup"><span data-stu-id="661ad-351">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="661ad-352">Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:</span><span class="sxs-lookup"><span data-stu-id="661ad-352">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="661ad-353">Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="661ad-353">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="661ad-354">Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (için `CUSTOMCONNSTR_`, belirtilen sağlayıcı olmayan).</span><span class="sxs-lookup"><span data-stu-id="661ad-354">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="661ad-355">Ortam değişkeni anahtarı</span><span class="sxs-lookup"><span data-stu-id="661ad-355">Environment variable key</span></span> | <span data-ttu-id="661ad-356">Dönüştürülen yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="661ad-356">Converted configuration key</span></span> | <span data-ttu-id="661ad-357">Sağlayıcı yapılandırma girişi</span><span class="sxs-lookup"><span data-stu-id="661ad-357">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="661ad-358">Yapılandırma girişi oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="661ad-358">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="661ad-359">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="661ad-359">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="661ad-360">Değer:`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="661ad-360">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="661ad-361">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="661ad-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="661ad-362">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="661ad-362">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="661ad-363">Anahtar: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="661ad-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="661ad-364">Değer:`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="661ad-364">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="661ad-365">Dosya yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-365">File Configuration Provider</span></span>

<span data-ttu-id="661ad-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="661ad-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="661ad-367">Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="661ad-367">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="661ad-368">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-368">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="661ad-369">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-369">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="661ad-370">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-370">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="661ad-371">INı yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-371">INI Configuration Provider</span></span>

<span data-ttu-id="661ad-372">, <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Çalışma zamanında INI dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="661ad-372">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="661ad-373">INI dosya yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-373">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="661ad-374">İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-374">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="661ad-375">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="661ad-375">Overloads permit specifying:</span></span>

* <span data-ttu-id="661ad-376">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="661ad-376">Whether the file is optional.</span></span>
* <span data-ttu-id="661ad-377">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="661ad-377">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="661ad-378">Dosyaya erişmek için kullanılır. <xref:Microsoft.Extensions.FileProviders.IFileProvider></span><span class="sxs-lookup"><span data-stu-id="661ad-378">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="661ad-379">Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın:</span><span class="sxs-lookup"><span data-stu-id="661ad-379">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="661ad-380">Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-380">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="661ad-381">Bir ıNı yapılandırma dosyasına genel bir örnek:</span><span class="sxs-lookup"><span data-stu-id="661ad-381">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="661ad-382">Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:</span><span class="sxs-lookup"><span data-stu-id="661ad-382">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="661ad-383">section0:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-383">section0:key0</span></span>
* <span data-ttu-id="661ad-384">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-384">section0:key1</span></span>
* <span data-ttu-id="661ad-385">Section1: alt bölüm: anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-385">section1:subsection:key</span></span>
* <span data-ttu-id="661ad-386">section2: subsection0: anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-386">section2:subsection0:key</span></span>
* <span data-ttu-id="661ad-387">section2: subsection1: anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-387">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="661ad-388">JSON yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-388">JSON Configuration Provider</span></span>

<span data-ttu-id="661ad-389">, <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="661ad-389">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="661ad-390">JSON dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-390">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="661ad-391">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="661ad-391">Overloads permit specifying:</span></span>

* <span data-ttu-id="661ad-392">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="661ad-392">Whether the file is optional.</span></span>
* <span data-ttu-id="661ad-393">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="661ad-393">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="661ad-394">Dosyaya erişmek için kullanılır. <xref:Microsoft.Extensions.FileProviders.IFileProvider></span><span class="sxs-lookup"><span data-stu-id="661ad-394">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="661ad-395">`AddJsonFile`, ile `CreateDefaultBuilder`yeni bir konak Oluşturucu başlattığınızda otomatik olarak iki kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-395">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="661ad-396">Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="661ad-396">The method is called to load configuration from:</span></span>

* <span data-ttu-id="661ad-397">*appSettings. JSON* &ndash; bu dosya ilk kez okundu.</span><span class="sxs-lookup"><span data-stu-id="661ad-397">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="661ad-398">Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-398">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="661ad-399">*appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. environmentname](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="661ad-399">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="661ad-400">Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="661ad-400">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="661ad-401">`CreateDefaultBuilder`Ayrıca yüklenir:</span><span class="sxs-lookup"><span data-stu-id="661ad-401">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="661ad-402">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-402">Environment variables.</span></span>
* <span data-ttu-id="661ad-403">Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="661ad-403">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="661ad-404">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="661ad-404">Command-line arguments.</span></span>

<span data-ttu-id="661ad-405">JSON yapılandırma sağlayıcısı önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="661ad-405">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="661ad-406">Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="661ad-406">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="661ad-407">Ana `ConfigureAppConfiguration` bilgisayarı, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtmek üzere oluştururken çağırın *. { Environment}. JSON*:</span><span class="sxs-lookup"><span data-stu-id="661ad-407">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="661ad-408">Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-408">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="661ad-409">**Örnek**</span><span class="sxs-lookup"><span data-stu-id="661ad-409">**Example**</span></span>

<span data-ttu-id="661ad-410">Örnek uygulama, ' ye `CreateDefaultBuilder` `AddJsonFile`iki çağrı içeren konağı oluşturmak için statik kolaylık yönteminden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-410">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="661ad-411">Yapılandırma *appSettings. JSON* ve appSettings 'ten yüklendi *. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="661ad-411">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="661ad-412">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-412">Run the sample app.</span></span> <span data-ttu-id="661ad-413">Üzerinde `http://localhost:5000`uygulama için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="661ad-413">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="661ad-414">Çıktının, ortama bağlı olarak tabloda gösterilen yapılandırma için anahtar-değer çiftleri içerdiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="661ad-414">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="661ad-415">Günlüğe kaydetme yapılandırması anahtarları hiyerarşik ayırıcı olarak`:`iki nokta () kullanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-415">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="661ad-416">Anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-416">Key</span></span>                        | <span data-ttu-id="661ad-417">Geliştirme değeri</span><span class="sxs-lookup"><span data-stu-id="661ad-417">Development Value</span></span> | <span data-ttu-id="661ad-418">Üretim değeri</span><span class="sxs-lookup"><span data-stu-id="661ad-418">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="661ad-419">Günlüğe kaydetme: LogLevel: System</span><span class="sxs-lookup"><span data-stu-id="661ad-419">Logging:LogLevel:System</span></span>    | <span data-ttu-id="661ad-420">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="661ad-420">Information</span></span>       | <span data-ttu-id="661ad-421">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="661ad-421">Information</span></span>      |
| <span data-ttu-id="661ad-422">Günlüğe kaydetme: LogLevel: Microsoft</span><span class="sxs-lookup"><span data-stu-id="661ad-422">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="661ad-423">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="661ad-423">Information</span></span>       | <span data-ttu-id="661ad-424">Bilgiler</span><span class="sxs-lookup"><span data-stu-id="661ad-424">Information</span></span>      |
| <span data-ttu-id="661ad-425">Günlüğe kaydetme: LogLevel: default</span><span class="sxs-lookup"><span data-stu-id="661ad-425">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="661ad-426">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="661ad-426">Debug</span></span>             | <span data-ttu-id="661ad-427">Hata</span><span class="sxs-lookup"><span data-stu-id="661ad-427">Error</span></span>            |
| <span data-ttu-id="661ad-428">Allowedkonakları</span><span class="sxs-lookup"><span data-stu-id="661ad-428">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="661ad-429">XML yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-429">XML Configuration Provider</span></span>

<span data-ttu-id="661ad-430">, <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Çalışma zamanında XML dosya anahtar-değer çiftlerinden yapılandırmayı yükler.</span><span class="sxs-lookup"><span data-stu-id="661ad-430">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="661ad-431">XML dosya yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-431">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="661ad-432">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="661ad-432">Overloads permit specifying:</span></span>

* <span data-ttu-id="661ad-433">Dosyanın isteğe bağlı olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="661ad-433">Whether the file is optional.</span></span>
* <span data-ttu-id="661ad-434">Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="661ad-434">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="661ad-435">Dosyaya erişmek için kullanılır. <xref:Microsoft.Extensions.FileProviders.IFileProvider></span><span class="sxs-lookup"><span data-stu-id="661ad-435">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="661ad-436">Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-436">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="661ad-437">Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="661ad-437">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="661ad-438">Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın:</span><span class="sxs-lookup"><span data-stu-id="661ad-438">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="661ad-439">Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-439">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

<span data-ttu-id="661ad-440">XML yapılandırma dosyaları, yinelenen bölümler için farklı öğe adları kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="661ad-440">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="661ad-441">Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:</span><span class="sxs-lookup"><span data-stu-id="661ad-441">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="661ad-442">section0:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-442">section0:key0</span></span>
* <span data-ttu-id="661ad-443">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-443">section0:key1</span></span>
* <span data-ttu-id="661ad-444">section1:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-444">section1:key0</span></span>
* <span data-ttu-id="661ad-445">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-445">section1:key1</span></span>

<span data-ttu-id="661ad-446">Aynı öğe adını kullanan yinelenen öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılacaksa çalışır:</span><span class="sxs-lookup"><span data-stu-id="661ad-446">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="661ad-447">Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:</span><span class="sxs-lookup"><span data-stu-id="661ad-447">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="661ad-448">Bölüm: section0: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="661ad-448">section:section0:key:key0</span></span>
* <span data-ttu-id="661ad-449">Bölüm: section0: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-449">section:section0:key:key1</span></span>
* <span data-ttu-id="661ad-450">Bölüm: Section1: Key: Key0</span><span class="sxs-lookup"><span data-stu-id="661ad-450">section:section1:key:key0</span></span>
* <span data-ttu-id="661ad-451">Bölüm: Section1: Key: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-451">section:section1:key:key1</span></span>

<span data-ttu-id="661ad-452">Öznitelikler, değerler sağlamak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="661ad-452">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="661ad-453">Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:</span><span class="sxs-lookup"><span data-stu-id="661ad-453">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="661ad-454">anahtar: öznitelik</span><span class="sxs-lookup"><span data-stu-id="661ad-454">key:attribute</span></span>
* <span data-ttu-id="661ad-455">Section: Key: özniteliği</span><span class="sxs-lookup"><span data-stu-id="661ad-455">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="661ad-456">Dosya başına anahtar yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-456">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="661ad-457">, <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-457">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="661ad-458">Anahtar, dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="661ad-458">The key is the file name.</span></span> <span data-ttu-id="661ad-459">Değer, dosyanın içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="661ad-459">The value contains the file's contents.</span></span> <span data-ttu-id="661ad-460">Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-460">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="661ad-461">Dosya başına anahtar yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-461">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="661ad-462">`directoryPath` Dosyaların mutlak bir yol olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="661ad-462">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="661ad-463">Aşırı yüklemeler belirtmeye izin ver:</span><span class="sxs-lookup"><span data-stu-id="661ad-463">Overloads permit specifying:</span></span>

* <span data-ttu-id="661ad-464">Kaynağı `Action<KeyPerFileConfigurationSource>` yapılandıran bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="661ad-464">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="661ad-465">Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="661ad-465">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="661ad-466">Çift alt çizgi (`__`), dosya adlarında yapılandırma anahtarı sınırlayıcısı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-466">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="661ad-467">Örneğin, dosya adı `Logging__LogLevel__System` yapılandırma anahtarını `Logging:LogLevel:System`üretir.</span><span class="sxs-lookup"><span data-stu-id="661ad-467">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="661ad-468">Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın:</span><span class="sxs-lookup"><span data-stu-id="661ad-468">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<span data-ttu-id="661ad-469">Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-469">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span>

## <a name="memory-configuration-provider"></a><span data-ttu-id="661ad-470">Bellek yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-470">Memory Configuration Provider</span></span>

<span data-ttu-id="661ad-471">, <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-471">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="661ad-472">Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-472">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="661ad-473">Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>`ile başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-473">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="661ad-474">Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın.</span><span class="sxs-lookup"><span data-stu-id="661ad-474">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="661ad-475">Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="661ad-475">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="661ad-476">Sözlük, yapılandırmayı sağlamak `AddInMemoryCollection` için çağrısı ile kullanılır:</span><span class="sxs-lookup"><span data-stu-id="661ad-476">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="661ad-477">GetValue</span><span class="sxs-lookup"><span data-stu-id="661ad-477">GetValue</span></span>

<span data-ttu-id="661ad-478">[Configurationciltçi. GetValue\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , yapılandırmadan belirtilen anahtarla bir değer ayıklar ve belirtilen türe dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="661ad-478">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="661ad-479">Bir aşırı yükleme, anahtar bulunamazsa varsayılan bir değer sağlamanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="661ad-479">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="661ad-480">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="661ad-480">The following example:</span></span>

* <span data-ttu-id="661ad-481">Yapılandırmadaki dize değerini anahtarla `NumberKey`ayıklar.</span><span class="sxs-lookup"><span data-stu-id="661ad-481">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="661ad-482">Yapılandırma anahtarlarında bulunmazsa, varsayılan `99` değeri kullanılır. `NumberKey`</span><span class="sxs-lookup"><span data-stu-id="661ad-482">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="661ad-483">Değeri olarak bir `int`.</span><span class="sxs-lookup"><span data-stu-id="661ad-483">Types the value as an `int`.</span></span>
* <span data-ttu-id="661ad-484">Değeri, sayfanın kullanımı için `NumberConfig` özelliği içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="661ad-484">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="661ad-485">GetSection, GetChildren ve Exists</span><span class="sxs-lookup"><span data-stu-id="661ad-485">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="661ad-486">İzleyen örnekler için aşağıdaki JSON dosyasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="661ad-486">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="661ad-487">İki bölüm arasında dört anahtar bulunur ve bunlardan biri alt bölümleri çifti içerir:</span><span class="sxs-lookup"><span data-stu-id="661ad-487">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="661ad-488">Dosya yapılandırmaya okunduğu zaman yapılandırma değerlerini tutmak için aşağıdaki benzersiz hiyerarşik anahtarlar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="661ad-488">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="661ad-489">section0:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-489">section0:key0</span></span>
* <span data-ttu-id="661ad-490">section0: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-490">section0:key1</span></span>
* <span data-ttu-id="661ad-491">section1:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-491">section1:key0</span></span>
* <span data-ttu-id="661ad-492">Section1: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-492">section1:key1</span></span>
* <span data-ttu-id="661ad-493">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-493">section2:subsection0:key0</span></span>
* <span data-ttu-id="661ad-494">section2: subsection0: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-494">section2:subsection0:key1</span></span>
* <span data-ttu-id="661ad-495">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="661ad-495">section2:subsection1:key0</span></span>
* <span data-ttu-id="661ad-496">section2: subsection1: KEY1</span><span class="sxs-lookup"><span data-stu-id="661ad-496">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="661ad-497">GetSection</span><span class="sxs-lookup"><span data-stu-id="661ad-497">GetSection</span></span>

<span data-ttu-id="661ad-498">[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü ayıklar.</span><span class="sxs-lookup"><span data-stu-id="661ad-498">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="661ad-499">Yalnızca içindeki <xref:Microsoft.Extensions.Configuration.IConfigurationSection> `section1`anahtar-değer çiftlerini içeren bir öğesini döndürmek için, bölüm `GetSection` adını çağırın ve sağlayın:</span><span class="sxs-lookup"><span data-stu-id="661ad-499">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="661ad-500">Bir değere sahip değil, yalnızca bir anahtar ve yol yoktur. `configSection`</span><span class="sxs-lookup"><span data-stu-id="661ad-500">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="661ad-501">Benzer şekilde, içindeki `section2:subsection0`anahtarların değerlerini almak için, Bölüm yolunu çağırın `GetSection` ve sağlayın:</span><span class="sxs-lookup"><span data-stu-id="661ad-501">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="661ad-502">`GetSection`hiçbir süre `null`geri döndürmez.</span><span class="sxs-lookup"><span data-stu-id="661ad-502">`GetSection` never returns `null`.</span></span> <span data-ttu-id="661ad-503">Eşleşen bir bölüm bulunamazsa boş `IConfigurationSection` değer döndürülür.</span><span class="sxs-lookup"><span data-stu-id="661ad-503">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="661ad-504">Eşleşen bir bölüm <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>döndürdüğünde,doldurulmuyor `GetSection` .</span><span class="sxs-lookup"><span data-stu-id="661ad-504">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="661ad-505">Bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve<xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> , bölüm varsa döndürülür.</span><span class="sxs-lookup"><span data-stu-id="661ad-505">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="661ad-506">GetChildren</span><span class="sxs-lookup"><span data-stu-id="661ad-506">GetChildren</span></span>

<span data-ttu-id="661ad-507">Üzerinde`section2` [bir iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı ,şunlarıiçerir:`IEnumerable<IConfigurationSection>`</span><span class="sxs-lookup"><span data-stu-id="661ad-507">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="661ad-508">Var</span><span class="sxs-lookup"><span data-stu-id="661ad-508">Exists</span></span>

<span data-ttu-id="661ad-509">Bir yapılandırma bölümünün mevcut olup olmadığını anlamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) kullanın:</span><span class="sxs-lookup"><span data-stu-id="661ad-509">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="661ad-510">Örnek veriler `sectionExists` `false` verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölüm olmadığı için.</span><span class="sxs-lookup"><span data-stu-id="661ad-510">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="661ad-511">Bir sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="661ad-511">Bind to a class</span></span>

<span data-ttu-id="661ad-512">Yapılandırma, *Seçenekler modelini*kullanarak ilgili ayarların gruplarını temsil eden sınıflara bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-512">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="661ad-513">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="661ad-513">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="661ad-514">Yapılandırma değerleri dizeler olarak döndürülür, ancak çağırma <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesnelerinin oluşturulmasını mümkün yapar.</span><span class="sxs-lookup"><span data-stu-id="661ad-514">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="661ad-515">Örnek uygulama bir `Starship` model içerir (*modeller/starsevk. cs*):</span><span class="sxs-lookup"><span data-stu-id="661ad-515">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="661ad-516">*Starsevk. JSON* dosyasının bölümü,örnekuygulamayapılandırmayıyüklemekiçinJSONyapılandırmasağlayıcısınıkullandığındayapılandırmayıoluşturur:`starship`</span><span class="sxs-lookup"><span data-stu-id="661ad-516">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="661ad-517">Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="661ad-517">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="661ad-518">Anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-518">Key</span></span>                   | <span data-ttu-id="661ad-519">Değer</span><span class="sxs-lookup"><span data-stu-id="661ad-519">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="661ad-520">starsevk: ad</span><span class="sxs-lookup"><span data-stu-id="661ad-520">starship:name</span></span>         | <span data-ttu-id="661ad-521">USS kurumsal</span><span class="sxs-lookup"><span data-stu-id="661ad-521">USS Enterprise</span></span>                                    |
| <span data-ttu-id="661ad-522">starsevk: kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="661ad-522">starship:registry</span></span>     | <span data-ttu-id="661ad-523">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="661ad-523">NCC-1701</span></span>                                          |
| <span data-ttu-id="661ad-524">starsevk: sınıfı</span><span class="sxs-lookup"><span data-stu-id="661ad-524">starship:class</span></span>        | <span data-ttu-id="661ad-525">Anaytion</span><span class="sxs-lookup"><span data-stu-id="661ad-525">Constitution</span></span>                                      |
| <span data-ttu-id="661ad-526">başlangıçgönder: Uzunluk</span><span class="sxs-lookup"><span data-stu-id="661ad-526">starship:length</span></span>       | <span data-ttu-id="661ad-527">304,8</span><span class="sxs-lookup"><span data-stu-id="661ad-527">304.8</span></span>                                             |
| <span data-ttu-id="661ad-528">starsevkiyat: Commissioned</span><span class="sxs-lookup"><span data-stu-id="661ad-528">starship:commissioned</span></span> | <span data-ttu-id="661ad-529">False</span><span class="sxs-lookup"><span data-stu-id="661ad-529">False</span></span>                                             |
| <span data-ttu-id="661ad-530">dır</span><span class="sxs-lookup"><span data-stu-id="661ad-530">trademark</span></span>             | <span data-ttu-id="661ad-531">Paramount resimleri Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="661ad-531">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="661ad-532">Örnek uygulama, `starship` anahtarla `GetSection` çağırır.</span><span class="sxs-lookup"><span data-stu-id="661ad-532">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="661ad-533">`starship` Anahtar-değer çiftleri yalıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="661ad-533">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="661ad-534">Yöntemi, `Starship` sınıfının bir örneğinde geçen alt bölümde çağrılır. `Bind`</span><span class="sxs-lookup"><span data-stu-id="661ad-534">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="661ad-535">Örnek değerlerini bağladıktan sonra örnek, işleme için bir özelliğe atanır:</span><span class="sxs-lookup"><span data-stu-id="661ad-535">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="661ad-536">Bir nesne grafiğine bağlama</span><span class="sxs-lookup"><span data-stu-id="661ad-536">Bind to an object graph</span></span>

<span data-ttu-id="661ad-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, bir POCO nesne grafiğinin tamamına bağlama yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="661ad-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="661ad-538">Örnek, nesne grafı `TvShow` içeren `Metadata` ve `Actors` sınıfları olan bir model içerir (*modeller/tvshow. cs*):</span><span class="sxs-lookup"><span data-stu-id="661ad-538">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="661ad-539">Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="661ad-539">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="661ad-540">Yapılandırma, `TvShow` `Bind` yöntemiyle nesne grafiğinin tamamına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-540">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="661ad-541">Bağlantılı örnek, işleme için bir özelliğe atandı:</span><span class="sxs-lookup"><span data-stu-id="661ad-541">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="661ad-542">[Configurationciltçi. Get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="661ad-542">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="661ad-543">`Get<T>`, kullanmaktan `Bind`daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="661ad-543">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="661ad-544">Aşağıdaki kod, ilişkili örneğin işleme için `Get<T>` kullanılan özelliğe doğrudan atanmasını sağlayan önceki örnekle birlikte nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="661ad-544">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="661ad-545">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="661ad-545">Bind an array to a class</span></span>

<span data-ttu-id="661ad-546">*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*</span><span class="sxs-lookup"><span data-stu-id="661ad-546">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="661ad-547">, <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Yapılandırma anahtarlarındaki dizi dizinlerini kullanarak nesnelere dizileri bağlamayı destekler.</span><span class="sxs-lookup"><span data-stu-id="661ad-547">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="661ad-548">Bir sayısal anahtar segmenti`:0:`(, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir poco sınıfı dizisine dizi bağlama özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="661ad-548">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="661ad-549">Bağlama, kural tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-549">Binding is provided by convention.</span></span> <span data-ttu-id="661ad-550">Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="661ad-550">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="661ad-551">**Bellek içi dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="661ad-551">**In-memory array processing**</span></span>

<span data-ttu-id="661ad-552">Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="661ad-552">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="661ad-553">Anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-553">Key</span></span>             | <span data-ttu-id="661ad-554">Değer</span><span class="sxs-lookup"><span data-stu-id="661ad-554">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="661ad-555">dizi: girdiler: 0</span><span class="sxs-lookup"><span data-stu-id="661ad-555">array:entries:0</span></span> | <span data-ttu-id="661ad-556">value0</span><span class="sxs-lookup"><span data-stu-id="661ad-556">value0</span></span> |
| <span data-ttu-id="661ad-557">dizi: girdiler: 1</span><span class="sxs-lookup"><span data-stu-id="661ad-557">array:entries:1</span></span> | <span data-ttu-id="661ad-558">value1</span><span class="sxs-lookup"><span data-stu-id="661ad-558">value1</span></span> |
| <span data-ttu-id="661ad-559">dizi: girdiler: 2</span><span class="sxs-lookup"><span data-stu-id="661ad-559">array:entries:2</span></span> | <span data-ttu-id="661ad-560">value2</span><span class="sxs-lookup"><span data-stu-id="661ad-560">value2</span></span> |
| <span data-ttu-id="661ad-561">dizi: girdiler: 4</span><span class="sxs-lookup"><span data-stu-id="661ad-561">array:entries:4</span></span> | <span data-ttu-id="661ad-562">value4</span><span class="sxs-lookup"><span data-stu-id="661ad-562">value4</span></span> |
| <span data-ttu-id="661ad-563">dizi: girdiler: 5</span><span class="sxs-lookup"><span data-stu-id="661ad-563">array:entries:5</span></span> | <span data-ttu-id="661ad-564">value5</span><span class="sxs-lookup"><span data-stu-id="661ad-564">value5</span></span> |

<span data-ttu-id="661ad-565">Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:</span><span class="sxs-lookup"><span data-stu-id="661ad-565">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

<span data-ttu-id="661ad-566">Dizi, Dizin &num;3 için bir değeri atlar.</span><span class="sxs-lookup"><span data-stu-id="661ad-566">The array skips a value for index &num;3.</span></span> <span data-ttu-id="661ad-567">Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.</span><span class="sxs-lookup"><span data-stu-id="661ad-567">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="661ad-568">Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="661ad-568">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="661ad-569">Yapılandırma verileri nesnesine bağlanır:</span><span class="sxs-lookup"><span data-stu-id="661ad-569">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="661ad-570">[Configurationciltçi. Get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="661ad-570">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="661ad-571">Bir örneği olan `ArrayExample`bağlantılı nesne, yapılandırmadan dizi verilerini alır.</span><span class="sxs-lookup"><span data-stu-id="661ad-571">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="661ad-572">`ArrayExample.Entries`İndeks</span><span class="sxs-lookup"><span data-stu-id="661ad-572">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="661ad-573">`ArrayExample.Entries`Deeri</span><span class="sxs-lookup"><span data-stu-id="661ad-573">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="661ad-574">0</span><span class="sxs-lookup"><span data-stu-id="661ad-574">0</span></span>                            | <span data-ttu-id="661ad-575">value0</span><span class="sxs-lookup"><span data-stu-id="661ad-575">value0</span></span>                       |
| <span data-ttu-id="661ad-576">1\.</span><span class="sxs-lookup"><span data-stu-id="661ad-576">1</span></span>                            | <span data-ttu-id="661ad-577">value1</span><span class="sxs-lookup"><span data-stu-id="661ad-577">value1</span></span>                       |
| <span data-ttu-id="661ad-578">2</span><span class="sxs-lookup"><span data-stu-id="661ad-578">2</span></span>                            | <span data-ttu-id="661ad-579">value2</span><span class="sxs-lookup"><span data-stu-id="661ad-579">value2</span></span>                       |
| <span data-ttu-id="661ad-580">3</span><span class="sxs-lookup"><span data-stu-id="661ad-580">3</span></span>                            | <span data-ttu-id="661ad-581">value4</span><span class="sxs-lookup"><span data-stu-id="661ad-581">value4</span></span>                       |
| <span data-ttu-id="661ad-582">4</span><span class="sxs-lookup"><span data-stu-id="661ad-582">4</span></span>                            | <span data-ttu-id="661ad-583">value5</span><span class="sxs-lookup"><span data-stu-id="661ad-583">value5</span></span>                       |

<span data-ttu-id="661ad-584">İlişkili &num;nesnede Dizin 3 `array:4` yapılandırma `value4`anahtarı ve değeri için yapılandırma verilerini barındırır.</span><span class="sxs-lookup"><span data-stu-id="661ad-584">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="661ad-585">Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-585">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="661ad-586">Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="661ad-586">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="661ad-587">Yapılandırmada doğru anahtar-değer çiftini &num;üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğe bağlamadan önce dizin 3 için eksik yapılandırma öğesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="661ad-587">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="661ad-588">Örnek, eksik anahtar-değer çiftine `ArrayExample.Entries` sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, tam yapılandırma dizisiyle eşleşir:</span><span class="sxs-lookup"><span data-stu-id="661ad-588">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="661ad-589">*missing_value. JSON*:</span><span class="sxs-lookup"><span data-stu-id="661ad-589">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="661ad-590">İçinde `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="661ad-590">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="661ad-591">Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.</span><span class="sxs-lookup"><span data-stu-id="661ad-591">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="661ad-592">Anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-592">Key</span></span>             | <span data-ttu-id="661ad-593">Değer</span><span class="sxs-lookup"><span data-stu-id="661ad-593">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="661ad-594">dizi: girdiler: 3</span><span class="sxs-lookup"><span data-stu-id="661ad-594">array:entries:3</span></span> | <span data-ttu-id="661ad-595">value3</span><span class="sxs-lookup"><span data-stu-id="661ad-595">value3</span></span> |

<span data-ttu-id="661ad-596">Sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ü içeriyorsa, `ArrayExample.Entries` dizi değerini içerir. `ArrayExample`</span><span class="sxs-lookup"><span data-stu-id="661ad-596">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="661ad-597">`ArrayExample.Entries`İndeks</span><span class="sxs-lookup"><span data-stu-id="661ad-597">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="661ad-598">`ArrayExample.Entries`Deeri</span><span class="sxs-lookup"><span data-stu-id="661ad-598">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="661ad-599">0</span><span class="sxs-lookup"><span data-stu-id="661ad-599">0</span></span>                            | <span data-ttu-id="661ad-600">value0</span><span class="sxs-lookup"><span data-stu-id="661ad-600">value0</span></span>                       |
| <span data-ttu-id="661ad-601">1\.</span><span class="sxs-lookup"><span data-stu-id="661ad-601">1</span></span>                            | <span data-ttu-id="661ad-602">value1</span><span class="sxs-lookup"><span data-stu-id="661ad-602">value1</span></span>                       |
| <span data-ttu-id="661ad-603">2</span><span class="sxs-lookup"><span data-stu-id="661ad-603">2</span></span>                            | <span data-ttu-id="661ad-604">value2</span><span class="sxs-lookup"><span data-stu-id="661ad-604">value2</span></span>                       |
| <span data-ttu-id="661ad-605">3</span><span class="sxs-lookup"><span data-stu-id="661ad-605">3</span></span>                            | <span data-ttu-id="661ad-606">value3</span><span class="sxs-lookup"><span data-stu-id="661ad-606">value3</span></span>                       |
| <span data-ttu-id="661ad-607">4</span><span class="sxs-lookup"><span data-stu-id="661ad-607">4</span></span>                            | <span data-ttu-id="661ad-608">value4</span><span class="sxs-lookup"><span data-stu-id="661ad-608">value4</span></span>                       |
| <span data-ttu-id="661ad-609">5</span><span class="sxs-lookup"><span data-stu-id="661ad-609">5</span></span>                            | <span data-ttu-id="661ad-610">value5</span><span class="sxs-lookup"><span data-stu-id="661ad-610">value5</span></span>                       |

<span data-ttu-id="661ad-611">**JSON dizi işleme**</span><span class="sxs-lookup"><span data-stu-id="661ad-611">**JSON array processing**</span></span>

<span data-ttu-id="661ad-612">JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="661ad-612">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="661ad-613">Aşağıdaki yapılandırma dosyasında `subsection` bir dizidir:</span><span class="sxs-lookup"><span data-stu-id="661ad-613">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="661ad-614">JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:</span><span class="sxs-lookup"><span data-stu-id="661ad-614">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="661ad-615">Anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-615">Key</span></span>                     | <span data-ttu-id="661ad-616">Değer</span><span class="sxs-lookup"><span data-stu-id="661ad-616">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="661ad-617">json_array: anahtar</span><span class="sxs-lookup"><span data-stu-id="661ad-617">json_array:key</span></span>          | <span data-ttu-id="661ad-618">değer EA</span><span class="sxs-lookup"><span data-stu-id="661ad-618">valueA</span></span> |
| <span data-ttu-id="661ad-619">json_array: alt bölüm: 0</span><span class="sxs-lookup"><span data-stu-id="661ad-619">json_array:subsection:0</span></span> | <span data-ttu-id="661ad-620">valueB</span><span class="sxs-lookup"><span data-stu-id="661ad-620">valueB</span></span> |
| <span data-ttu-id="661ad-621">json_array: alt bölüm: 1</span><span class="sxs-lookup"><span data-stu-id="661ad-621">json_array:subsection:1</span></span> | <span data-ttu-id="661ad-622">değer EC</span><span class="sxs-lookup"><span data-stu-id="661ad-622">valueC</span></span> |
| <span data-ttu-id="661ad-623">json_array: alt bölüm: 2</span><span class="sxs-lookup"><span data-stu-id="661ad-623">json_array:subsection:2</span></span> | <span data-ttu-id="661ad-624">Değerler</span><span class="sxs-lookup"><span data-stu-id="661ad-624">valueD</span></span> |

<span data-ttu-id="661ad-625">Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="661ad-625">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="661ad-626">Bağlama `JsonArrayExample.Key` işleminden sonra değeri `valueA`barındırır.</span><span class="sxs-lookup"><span data-stu-id="661ad-626">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="661ad-627">Alt bölüm değerleri POCO dizisi özelliğinde `Subsection`depolanır.</span><span class="sxs-lookup"><span data-stu-id="661ad-627">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="661ad-628">`JsonArrayExample.Subsection`İndeks</span><span class="sxs-lookup"><span data-stu-id="661ad-628">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="661ad-629">`JsonArrayExample.Subsection`Deeri</span><span class="sxs-lookup"><span data-stu-id="661ad-629">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="661ad-630">0</span><span class="sxs-lookup"><span data-stu-id="661ad-630">0</span></span>                                   | <span data-ttu-id="661ad-631">valueB</span><span class="sxs-lookup"><span data-stu-id="661ad-631">valueB</span></span>                              |
| <span data-ttu-id="661ad-632">1\.</span><span class="sxs-lookup"><span data-stu-id="661ad-632">1</span></span>                                   | <span data-ttu-id="661ad-633">değer EC</span><span class="sxs-lookup"><span data-stu-id="661ad-633">valueC</span></span>                              |
| <span data-ttu-id="661ad-634">2</span><span class="sxs-lookup"><span data-stu-id="661ad-634">2</span></span>                                   | <span data-ttu-id="661ad-635">Değerler</span><span class="sxs-lookup"><span data-stu-id="661ad-635">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="661ad-636">Özel yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="661ad-636">Custom configuration provider</span></span>

<span data-ttu-id="661ad-637">Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="661ad-637">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="661ad-638">Sağlayıcı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="661ad-638">The provider has the following characteristics:</span></span>

* <span data-ttu-id="661ad-639">EF bellek içi veritabanı, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="661ad-639">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="661ad-640">Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı `ConfigurationBuilder` dizesini sağlamak üzere bir ikincil uygulayın.</span><span class="sxs-lookup"><span data-stu-id="661ad-640">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="661ad-641">Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur.</span><span class="sxs-lookup"><span data-stu-id="661ad-641">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="661ad-642">Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.</span><span class="sxs-lookup"><span data-stu-id="661ad-642">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="661ad-643">Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="661ad-643">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="661ad-644">Yapılandırma değerlerini `EFConfigurationValue` veritabanında depolamak için bir varlık tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="661ad-644">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="661ad-645">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-645">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="661ad-646">Bir `EFConfigurationContext` mağazaya ekleyin ve yapılandırılan değerlere erişin.</span><span class="sxs-lookup"><span data-stu-id="661ad-646">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="661ad-647">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-647">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="661ad-648">Uygulayan <xref:Microsoft.Extensions.Configuration.IConfigurationSource>bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="661ad-648">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="661ad-649">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-649">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="661ad-650">Öğesinden <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralarak özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="661ad-650">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="661ad-651">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="661ad-651">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="661ad-652">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-652">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="661ad-653">Genişletme yöntemi, yapılandırma kaynağını bir `ConfigurationBuilder`öğesine eklemeye izin verir. `AddEFConfiguration`</span><span class="sxs-lookup"><span data-stu-id="661ad-653">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="661ad-654">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-654">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="661ad-655">Aşağıdaki kod, `EFConfigurationProvider` *program.cs*içinde özel kullanımı göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="661ad-655">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="661ad-656">*Modeller/EFConfigurationValue. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-656">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="661ad-657">Bir `EFConfigurationContext` mağazaya ekleyin ve yapılandırılan değerlere erişin.</span><span class="sxs-lookup"><span data-stu-id="661ad-657">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="661ad-658">*Efconfigurationprovider/EFConfigurationContext. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="661ad-659">Uygulayan <xref:Microsoft.Extensions.Configuration.IConfigurationSource>bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="661ad-659">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="661ad-660">*Efconfigurationprovider/EFConfigurationSource. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="661ad-661">Öğesinden <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralarak özel yapılandırma sağlayıcısını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="661ad-661">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="661ad-662">Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.</span><span class="sxs-lookup"><span data-stu-id="661ad-662">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="661ad-663">*Efconfigurationprovider/efconfigurationprovider. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-663">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="661ad-664">Genişletme yöntemi, yapılandırma kaynağını bir `ConfigurationBuilder`öğesine eklemeye izin verir. `AddEFConfiguration`</span><span class="sxs-lookup"><span data-stu-id="661ad-664">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="661ad-665">*Uzantılar/EntityFrameworkExtensions. cs*:</span><span class="sxs-lookup"><span data-stu-id="661ad-665">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="661ad-666">Aşağıdaki kod, `EFConfigurationProvider` *program.cs*içinde özel kullanımı göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="661ad-666">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="661ad-667">Başlangıç sırasında yapılandırmaya erişim</span><span class="sxs-lookup"><span data-stu-id="661ad-667">Access configuration during startup</span></span>

<span data-ttu-id="661ad-668">İçindeki `IConfiguration` yapılandırmadeğerlerineerişmek`Startup.ConfigureServices`için `Startup` oluşturucuya ekleme.</span><span class="sxs-lookup"><span data-stu-id="661ad-668">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="661ad-669">' Da yapılandırmaya `Startup.Configure`erişmek için, `IConfiguration` doğrudan yönteme ekleyin veya örneği oluşturucudan kullanın:</span><span class="sxs-lookup"><span data-stu-id="661ad-669">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="661ad-670">Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz [. uygulama başlatma: Kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="661ad-670">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="661ad-671">Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması</span><span class="sxs-lookup"><span data-stu-id="661ad-671">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="661ad-672">Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, <xref:Microsoft.Extensions.Configuration.IConfiguration> [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfaya ekleyin veya görünümü.</span><span class="sxs-lookup"><span data-stu-id="661ad-672">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="661ad-673">Razor Pages sayfasında:</span><span class="sxs-lookup"><span data-stu-id="661ad-673">In a Razor Pages page:</span></span>

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

<span data-ttu-id="661ad-674">MVC görünümünde:</span><span class="sxs-lookup"><span data-stu-id="661ad-674">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="661ad-675">Bir dış derlemeden yapılandırma Ekle</span><span class="sxs-lookup"><span data-stu-id="661ad-675">Add configuration from an external assembly</span></span>

<span data-ttu-id="661ad-676"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Uygulama ,`Startup` uygulamanın sınıfı dışında bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler eklenmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="661ad-676">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="661ad-677">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="661ad-677">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="661ad-678">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="661ad-678">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
