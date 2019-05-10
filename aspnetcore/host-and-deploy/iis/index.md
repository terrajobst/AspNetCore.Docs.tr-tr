---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/07/2019
uid: host-and-deploy/iis/index
ms.openlocfilehash: c8e742047230339434b910de9a8a2492bc4da1ff
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450987"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="0b940-103">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="0b940-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="0b940-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0b940-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="0b940-105">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="0b940-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="0b940-106">Desteklenen işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="0b940-106">Supported operating systems</span></span>

<span data-ttu-id="0b940-107">Aşağıdaki işletim sistemleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="0b940-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="0b940-108">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0b940-108">Windows 7 or later</span></span>
* <span data-ttu-id="0b940-109">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0b940-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="0b940-110">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla çağrılan WebListener), ııs'li bir ters proxy yapılandırması çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="0b940-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="0b940-111">Kullanım [Kestrel sunucu](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="0b940-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="0b940-112">Azure'da barındırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="0b940-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="0b940-113">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="0b940-113">Supported platforms</span></span>

<span data-ttu-id="0b940-114">(X86) 32-bit ve 64-bit (x 64) dağıtım desteklenen yayımlanan uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="0b940-114">Apps published for 32-bit (x86) and 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="0b940-115">32 bit uygulama sürece dağıtma uygulama:</span><span class="sxs-lookup"><span data-stu-id="0b940-115">Deploy a 32-bit app unless the app:</span></span>

* <span data-ttu-id="0b940-116">Bir 64-bit uygulamalar tarafından kullanılabilecek daha büyük sanal bellek adres alanı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0b940-116">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="0b940-117">Daha büyük IIS yığın boyutu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0b940-117">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="0b940-118">64-bit yerel bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="0b940-118">Has 64-bit native dependencies.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="0b940-119">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="0b940-119">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="0b940-120">IISIntegration bileşenlerini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="0b940-120">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0b940-121">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="0b940-121">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0b940-122">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="0b940-122">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0b940-123">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="0b940-123">**In-process hosting model**</span></span>

<span data-ttu-id="0b940-124">`CreateDefaultBuilder` çağrıları `UseIIS` önyükleme yöntemi [CoreCLR](/dotnet/standard/glossary#coreclr) ve IIS çalışan işlemi uygulama barındırın (*w3wp.exe* veya *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="0b940-124">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="0b940-125">Performans testleri belirten bir .NET Core uygulaması işlem içi barındırma için uygulama işlem dışı ve proxy isteklerini barındırma kıyasla önemli ölçüde daha yüksek istek üretilen işini teslim [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="0b940-125">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="0b940-126">İşlem içi barındırma modeli, .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="0b940-126">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="0b940-127">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="0b940-127">**Out-of-process hosting model**</span></span>

<span data-ttu-id="0b940-128">IIS ile işlem dışı barındırmak için `CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b940-128">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="0b940-129">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0b940-129">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0b940-130">`CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0b940-130">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="0b940-131">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="0b940-131">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="0b940-132">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="0b940-132">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="0b940-133">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="0b940-133">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="0b940-134">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="0b940-134">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="0b940-135">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0b940-135">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0b940-136">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0b940-136">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="0b940-137">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirttiğiniz bağlantı noktalarındaki Kestrel dinlediği.</span><span class="sxs-lookup"><span data-stu-id="0b940-137">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="0b940-138">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b940-138">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="0b940-139">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b940-139">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="0b940-140">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0b940-140">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0b940-141">`CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0b940-141">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="0b940-142">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="0b940-142">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="0b940-143">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="0b940-143">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="0b940-144">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="0b940-144">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="0b940-145">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="0b940-145">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="0b940-146">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0b940-146">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0b940-147">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0b940-147">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="0b940-148">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0b940-148">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0b940-149">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b940-149">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="0b940-150">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0b940-150">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0b940-151">`CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0b940-151">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="0b940-152">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="0b940-152">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="0b940-153">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="0b940-153">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="0b940-154">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="0b940-154">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="0b940-155">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="0b940-155">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="0b940-156">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0b940-156">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0b940-157">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0b940-157">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="0b940-158">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0b940-158">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0b940-159">Bir bağımlılık dahil [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıklarını bir pakette.</span><span class="sxs-lookup"><span data-stu-id="0b940-159">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="0b940-160">Ekleyerek IIS tümleştirme Ara yazılımları kullanmayı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> genişletme yöntemi için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>:</span><span class="sxs-lookup"><span data-stu-id="0b940-160">Use IIS Integration middleware by adding the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension method to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="0b940-161">Her ikisi de <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> ve <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> gereklidir.</span><span class="sxs-lookup"><span data-stu-id="0b940-161">Both <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> and <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> are required.</span></span> <span data-ttu-id="0b940-162">Kod arama `UseIISIntegration` kod taşınabilirliği etkilemez.</span><span class="sxs-lookup"><span data-stu-id="0b940-162">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="0b940-163">Uygulamanın IIS çalıştırırsanız değildir (örneğin, uygulamayı doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="0b940-163">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="0b940-164">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0b940-164">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0b940-165">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="0b940-165">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="0b940-166">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="0b940-166">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="0b940-167">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="0b940-167">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="0b940-168">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0b940-168">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0b940-169">Bir çağrı `UseUrls` Modülü'nü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0b940-169">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="0b940-170">Varsa `UseUrls` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0b940-170">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="0b940-171">`UseUrls` Olan bir ASP.NET Core 1.0 uygulamada çağrılır, çağrı **önce** çağırma `UseIISIntegration` böylece modül yapılandırılan bağlantı noktası üzerine değil.</span><span class="sxs-lookup"><span data-stu-id="0b940-171">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="0b940-172">Ayarı modülü geçersiz kıldığından bu arama sırası ASP.NET Core 1.1 ile gerekli değildir `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="0b940-172">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="0b940-173">Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core ana](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="0b940-173">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

### <a name="iis-options"></a><span data-ttu-id="0b940-174">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="0b940-174">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0b940-175">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="0b940-175">**In-process hosting model**</span></span>

<span data-ttu-id="0b940-176">IIS sunucusu seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil <xref:Microsoft.AspNetCore.Builder.IISServerOptions> içinde <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span><span class="sxs-lookup"><span data-stu-id="0b940-176">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="0b940-177">Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="0b940-177">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="0b940-178">Seçenek</span><span class="sxs-lookup"><span data-stu-id="0b940-178">Option</span></span>                         | <span data-ttu-id="0b940-179">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0b940-179">Default</span></span> | <span data-ttu-id="0b940-180">Ayar</span><span class="sxs-lookup"><span data-stu-id="0b940-180">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="0b940-181">Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-181">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="0b940-182">Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0b940-182">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="0b940-183">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="0b940-183">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="0b940-184">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-184">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="0b940-185">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0b940-185">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="0b940-186">Zaman uyumlu g/ç için izin verilip verilmediğini `HttpContext.Request` ve `HttpContext.Response`.</span><span class="sxs-lookup"><span data-stu-id="0b940-186">Whether synchronous IO is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="0b940-187">Alır veya ayarlar için en fazla istek gövdesi boyutu `HttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="0b940-187">Gets or sets the the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="0b940-188">IIS'nin sınırına sahip olduğuna dikkat edin `maxAllowedContentLength` hangi işlenmeyecek önce `MaxRequestBodySize` kümesinde `IISServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="0b940-188">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="0b940-189">Değiştirme `MaxRequestBodySize` etkilemez `maxAllowedContentLength`.</span><span class="sxs-lookup"><span data-stu-id="0b940-189">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="0b940-190">Artırmak için `maxAllowedContentLength`, bir giriş ekleyin *web.config* ayarlanacak `maxAllowedContentLength` daha yüksek bir değer.</span><span class="sxs-lookup"><span data-stu-id="0b940-190">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="0b940-191">Daha fazla ayrıntı için [yapılandırma](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span><span class="sxs-lookup"><span data-stu-id="0b940-191">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="0b940-192">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="0b940-192">**Out-of-process hosting model**</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="0b940-193">Seçenek</span><span class="sxs-lookup"><span data-stu-id="0b940-193">Option</span></span>                         | <span data-ttu-id="0b940-194">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0b940-194">Default</span></span> | <span data-ttu-id="0b940-195">Ayar</span><span class="sxs-lookup"><span data-stu-id="0b940-195">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="0b940-196">Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-196">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="0b940-197">Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0b940-197">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="0b940-198">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="0b940-198">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="0b940-199">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-199">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="0b940-200">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0b940-200">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="0b940-201">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="0b940-201">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="0b940-202">IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil <xref:Microsoft.AspNetCore.Builder.IISOptions> içinde <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span><span class="sxs-lookup"><span data-stu-id="0b940-202">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="0b940-203">Aşağıdaki örnek uygulamayı doldurmasını önler `HttpContext.Connection.ClientCertificate`:</span><span class="sxs-lookup"><span data-stu-id="0b940-203">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="0b940-204">Seçenek</span><span class="sxs-lookup"><span data-stu-id="0b940-204">Option</span></span>                         | <span data-ttu-id="0b940-205">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0b940-205">Default</span></span> | <span data-ttu-id="0b940-206">Ayar</span><span class="sxs-lookup"><span data-stu-id="0b940-206">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="0b940-207">Varsa `true`, IIS tümleştirme ara yazılımı ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-207">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="0b940-208">Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0b940-208">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="0b940-209">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="0b940-209">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="0b940-210">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu.</span><span class="sxs-lookup"><span data-stu-id="0b940-210">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="0b940-211">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0b940-211">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="0b940-212">Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="0b940-212">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0b940-213">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="0b940-213">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0b940-214">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="0b940-214">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="0b940-215">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-215">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="0b940-216">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="0b940-216">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="0b940-217">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="0b940-217">web.config file</span></span>

<span data-ttu-id="0b940-218">*Web.config* dosya yapılandırır [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b940-218">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="0b940-219">Oluşturma, dönüştürme ve yayımlama *web.config* dosya bir MSBuild hedefi tarafından gerçekleştirilir (`_TransformWebConfig`) projeyi ne zaman yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="0b940-219">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="0b940-220">Bu Web SDK'sı hedeflerin mevcut hedefidir (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="0b940-220">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="0b940-221">SDK'sı, proje dosyasının en üstüne ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="0b940-221">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="0b940-222">Varsa bir *web.config* dosyası projedeki mevcut değil, dosyanın doğru oluşturulması *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modül](xref:host-and-deploy/aspnet-core-module) ve taşınabilir [yayımlanmış çıktısı](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="0b940-222">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="0b940-223">Varsa bir *web.config* dosyası projesinde, dosyanın doğru dönüştürülür *processPath* ve *bağımsız değişkenleri* ASP.NET Core modülü yapılandırmak için ve azure'a taşındı yayımlanmış çıktısı.</span><span class="sxs-lookup"><span data-stu-id="0b940-223">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="0b940-224">Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="0b940-224">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="0b940-225">*Web.config* etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları dosyası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-225">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="0b940-226">ASP.NET Core uygulamaları istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.</span><span class="sxs-lookup"><span data-stu-id="0b940-226">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="0b940-227">Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın  **\<IsTransformWebConfigDisabled >** özelliği proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="0b940-227">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="0b940-228">Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0b940-228">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="0b940-229">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b940-229">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="0b940-230">Web.config dosyası konumu</span><span class="sxs-lookup"><span data-stu-id="0b940-230">web.config file location</span></span>

<span data-ttu-id="0b940-231">Ayarlamak için [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) doğru *web.config* dosya dağıtılan uygulamayı içerik kök yolda (genellikle uygulama temel yolu) mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0b940-231">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="0b940-232">IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur.</span><span class="sxs-lookup"><span data-stu-id="0b940-232">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="0b940-233">*Web.config* dosya, Web dağıtımı kullanarak birden fazla uygulama yayımlamayı etkinleştirmek için uygulamanın kök dizininde gereklidir.</span><span class="sxs-lookup"><span data-stu-id="0b940-233">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="0b940-234">Mevcut uygulamanın fiziksel yola gibi hassas dosyalar  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="0b940-234">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="0b940-235">Zaman *web.config* dosya varsa ve siteyi normalde başlatır, IIS bunların istenmesi halinde bu hassas dosyalar hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="0b940-235">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="0b940-236">Varsa *web.config* dosyası eksik, yanlış adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet verebilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-236">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="0b940-237">***Web.config* doğru adlı, dağıtımdaki her zaman mevcut ve yukarı site normal başlangıç için yapılandırmak için dosya olmalıdır. Hiçbir zaman Kaldır *web.config* dosyasından bir üretim dağıtımı.**</span><span class="sxs-lookup"><span data-stu-id="0b940-237">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="0b940-238">Web.config’i dönüştürme</span><span class="sxs-lookup"><span data-stu-id="0b940-238">Transform web.config</span></span>

<span data-ttu-id="0b940-239">Dönüştürmeniz gerekirse *web.config* (örneğin, ortam değişkenlerini ayarlama, yapılandırma, profili veya ortama göre), bkz: yayımlama sırasında <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="0b940-239">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="0b940-240">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="0b940-240">IIS configuration</span></span>

<span data-ttu-id="0b940-241">**Windows Server işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="0b940-241">**Windows Server operating systems**</span></span>

<span data-ttu-id="0b940-242">Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0b940-242">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="0b940-243">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="0b940-243">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="0b940-244">Üzerinde **sunucu rolleri** kutusunu işaretleyin, adım **Web sunucusu (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="0b940-244">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="0b940-246">Sonra **özellikleri** adım **rol hizmetlerini** adım, Web sunucusu (IIS) yükler.</span><span class="sxs-lookup"><span data-stu-id="0b940-246">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="0b940-247">İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="0b940-247">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="0b940-249">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="0b940-249">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="0b940-250">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="0b940-250">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="0b940-251">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="0b940-251">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="0b940-252">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-252">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="0b940-253">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="0b940-253">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="0b940-254">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0b940-254">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="0b940-255">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="0b940-255">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="0b940-256">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="0b940-256">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="0b940-257">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="0b940-257">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="0b940-258">Aracılığıyla devam **onay** Hizmetleri ve web sunucusu rolü yüklemek için adım.</span><span class="sxs-lookup"><span data-stu-id="0b940-258">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="0b940-259">Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değilse **Web sunucusu (IIS)** rol.</span><span class="sxs-lookup"><span data-stu-id="0b940-259">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="0b940-260">**Windows masaüstü işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="0b940-260">**Windows desktop operating systems**</span></span>

<span data-ttu-id="0b940-261">Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="0b940-261">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="0b940-262">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="0b940-262">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="0b940-263">Açık **Internet Information Services** düğümü.</span><span class="sxs-lookup"><span data-stu-id="0b940-263">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="0b940-264">Açık **Web yönetimi araçları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="0b940-264">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="0b940-265">İçin kutuyu **IIS Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="0b940-265">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="0b940-266">İçin kutuyu **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="0b940-266">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="0b940-267">Varsayılan özelliklerini kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0b940-267">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="0b940-268">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="0b940-268">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="0b940-269">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="0b940-269">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="0b940-270">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="0b940-270">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="0b940-271">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-271">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="0b940-272">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="0b940-272">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="0b940-273">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0b940-273">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="0b940-274">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="0b940-274">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="0b940-275">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="0b940-275">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="0b940-276">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="0b940-276">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="0b940-277">IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="0b940-277">If the IIS installation requires a restart, restart the system.</span></span>

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="0b940-279">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="0b940-279">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="0b940-280">Yükleme *.NET Core barındırma paket* barındıran sistemde.</span><span class="sxs-lookup"><span data-stu-id="0b940-280">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="0b940-281">.NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b940-281">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="0b940-282">Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="0b940-282">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="0b940-283">Sistem, Internet bağlantısı yoksa, alma ve yükleme [Microsoft Visual C++ 2015 yeniden dağıtılabilir](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core barındırma paketini yüklemeden önce.</span><span class="sxs-lookup"><span data-stu-id="0b940-283">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b940-284">Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0b940-284">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="0b940-285">IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0b940-285">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="0b940-286">.NET Core 64-bit (x 64) sürümünü yükledikten sonra barındırma paket yüklü değilse, SDK'ları eksik görünebilir ([.NET Core SDK algılandı](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="0b940-286">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="0b940-287">Bu sorunu gidermek için bkz: <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span><span class="sxs-lookup"><span data-stu-id="0b940-287">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="0b940-288">Doğrudan indirme (geçerli sürüm)</span><span class="sxs-lookup"><span data-stu-id="0b940-288">Direct download (current version)</span></span>

<span data-ttu-id="0b940-289">Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:</span><span class="sxs-lookup"><span data-stu-id="0b940-289">Download the installer using the following link:</span></span>

[<span data-ttu-id="0b940-290">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="0b940-290">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="0b940-291">Yükleyici önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="0b940-291">Earlier versions of the installer</span></span>

<span data-ttu-id="0b940-292">Yükleyici önceki bir sürümünü almak için:</span><span class="sxs-lookup"><span data-stu-id="0b940-292">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="0b940-293">Gidin [.NET indirme arşivleri](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="0b940-293">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="0b940-294">Altında **.NET Core**, .NET Core sürümünüzü seçin.</span><span class="sxs-lookup"><span data-stu-id="0b940-294">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="0b940-295">İçinde **uygulamaları - çalışma zamanı çalıştırma** sütun, istenen .NET Core çalışma zamanı sürümü satırını bulur.</span><span class="sxs-lookup"><span data-stu-id="0b940-295">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="0b940-296">Kullanarak yükleyiciyi indirin **çalışma zamanı & paketi barındırma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="0b940-296">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="0b940-297">Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="0b940-297">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="0b940-298">Daha fazla bilgi için [Destek İlkesi](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="0b940-298">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="0b940-299">Barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="0b940-299">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="0b940-300">Sunucuda yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0b940-300">Run the installer on the server.</span></span> <span data-ttu-id="0b940-301">Aşağıdaki parametreleri, yükleyici komut kabuğunu yönetici olarak çalıştırılırken kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="0b940-301">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="0b940-302">`OPT_NO_ANCM=1` &ndash; ASP.NET Core modülü yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="0b940-302">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="0b940-303">`OPT_NO_RUNTIME=1` &ndash; .NET Core çalışma zamanı yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="0b940-303">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="0b940-304">`OPT_NO_SHAREDFX=1` &ndash; ASP.NET paylaşılan Framework (ASP.NET çalışma zamanı) yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="0b940-304">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="0b940-305">`OPT_NO_X86=1` &ndash; X86 yükleme atlanıyor çalışma zamanları.</span><span class="sxs-lookup"><span data-stu-id="0b940-305">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="0b940-306">32-bit uygulamaları barındırma gerekmez, bildiğiniz durumlarda bu parametreyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="0b940-306">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="0b940-307">Hem 32 bit hem de 64-bit uygulamaları gelecekte barındıracak ihtimali varsa, yoksa bu parametreyi kullanın ve her iki çalışma zamanları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="0b940-307">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="0b940-308">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; IIS paylaşılan yapılandırması kullanmak için denetimi devre dışı olduğunda paylaşılan yapılandırma (*applicationHost.config*) IIS yüklemesi ile aynı makinede olduğu.</span><span class="sxs-lookup"><span data-stu-id="0b940-308">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="0b940-309">*Yalnızca, ASP.NET Core 2.2 veya üzeri barındırma Bundler yükleyicileri için de kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="0b940-309">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="0b940-310">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0b940-310">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="0b940-311">Sistemi yeniden başlatın veya yürütme **net stop olan /y**çizgidir **net start w3svc** komut kabuğundan.</span><span class="sxs-lookup"><span data-stu-id="0b940-311">Restart the system or execute **net stop was /y**, followed by **net start w3svc** from a command shell.</span></span> <span data-ttu-id="0b940-312">Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.</span><span class="sxs-lookup"><span data-stu-id="0b940-312">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

> [!NOTE]
> <span data-ttu-id="0b940-313">IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="0b940-313">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="0b940-314">Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin</span><span class="sxs-lookup"><span data-stu-id="0b940-314">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="0b940-315">Uygulamaları olan sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="0b940-315">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="0b940-316">Web Dağıtımı'nı yüklemek için kullanın [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyicisini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="0b940-316">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="0b940-317">Tercih edilen yöntem, Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="0b940-317">The preferred method is to use WebPI.</span></span> <span data-ttu-id="0b940-318">Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="0b940-318">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="0b940-319">IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="0b940-319">Create the IIS site</span></span>

1. <span data-ttu-id="0b940-320">Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0b940-320">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="0b940-321">Bir uygulamanın dağıtım Düzen açıklanan [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="0b940-321">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="0b940-322">IIS Yöneticisi'nde açın sunucu düğümünde **bağlantıları** paneli.</span><span class="sxs-lookup"><span data-stu-id="0b940-322">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="0b940-323">Sağ **siteleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="0b940-323">Right-click the **Sites** folder.</span></span> <span data-ttu-id="0b940-324">Seçin **Web sitesi Ekle** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="0b940-324">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="0b940-325">Sağlayan bir **Site adı** ayarlayıp **fiziksel yolu** uygulamanın dağıtım klasörüne.</span><span class="sxs-lookup"><span data-stu-id="0b940-325">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="0b940-326">Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="0b940-326">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="0b940-328">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0b940-328">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="0b940-329">Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0b940-329">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="0b940-330">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0b940-330">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="0b940-331">Joker karakterler yerine açık bir ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="0b940-331">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="0b940-332">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="0b940-332">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="0b940-333">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0b940-333">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="0b940-334">Sunucu düğümü altında seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="0b940-334">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="0b940-335">Sitenin uygulama havuzu sağ tıklayıp **temel ayarları** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="0b940-335">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="0b940-336">İçinde **uygulama havuzunu Düzenle** penceresinde **.NET CLR sürümü** için **yönetilen kod yok**:</span><span class="sxs-lookup"><span data-stu-id="0b940-336">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="0b940-338">ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="0b940-338">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="0b940-339">ASP.NET Core, Masaüstü CLR yüklemede de içermez.</span><span class="sxs-lookup"><span data-stu-id="0b940-339">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="0b940-340">Ayarı **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="0b940-340">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="0b940-341">*ASP.NET Core 2.2 veya üzeri*: Bir 64-bit (x64) için [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) kullanan [işlem içi barındırma modeli](xref:fundamentals/servers/index#in-process-hosting-model), 32 bit (x 86) işlemleri için uygulama havuzunu devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="0b940-341">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="0b940-342">İçinde **eylemleri** kenar IIS Yöneticisi'nin > **uygulama havuzları**seçin **uygulama havuzu Varsayılanlarını Ayarla** veya **Gelişmiş ayarlar**.</span><span class="sxs-lookup"><span data-stu-id="0b940-342">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="0b940-343">Bulun **etkinleştirme 32-Bit uygulamaları** ve değerine `False`.</span><span class="sxs-lookup"><span data-stu-id="0b940-343">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="0b940-344">Bu ayar için dağıtılan uygulamaları etkilemez [barındırma işlemi çıkış](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="0b940-344">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="0b940-345">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0b940-345">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="0b940-346">Varsayılan uygulama havuzu kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimlik için doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve diğer gerekli kaynakları erişmek için gerekli izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="0b940-346">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="0b940-347">Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0b940-347">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="0b940-348">**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="0b940-348">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="0b940-349">Daha fazla bilgi için [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="0b940-349">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="0b940-350">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="0b940-350">Deploy the app</span></span>

<span data-ttu-id="0b940-351">Uygulamayı barındıran sistemde oluşturduğunuz klasöre dağıtın.</span><span class="sxs-lookup"><span data-stu-id="0b940-351">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="0b940-352">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="0b940-352">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="0b940-353">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="0b940-353">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="0b940-354">Bkz: [uygulama dağıtımı ASP.NET Core için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu için Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0b940-354">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="0b940-355">Barındırma sağlayıcısı oluşturmak için bir yayımlama profili veya destek sağlar, bunların profilini indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="0b940-355">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="0b940-357">Web dağıtımı Visual Studio'nun dışında</span><span class="sxs-lookup"><span data-stu-id="0b940-357">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="0b940-358">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio'nun dışında komut satırından da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-358">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="0b940-359">Daha fazla bilgi için [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="0b940-359">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="0b940-360">Alternatif Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="0b940-360">Alternatives to Web Deploy</span></span>

<span data-ttu-id="0b940-361">Uygulama barındırma sistemin el ile kopyalama, Xcopy, Robocopy ve PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="0b940-361">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="0b940-362">IIS'ye ASP.NET Core dağıtımı hakkında daha fazla bilgi için bkz. [IIS Yöneticiler için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümü.</span><span class="sxs-lookup"><span data-stu-id="0b940-362">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="0b940-363">Web sitesine Gözat</span><span class="sxs-lookup"><span data-stu-id="0b940-363">Browse the website</span></span>

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="0b940-365">Kilitli dağıtım dosyaları</span><span class="sxs-lookup"><span data-stu-id="0b940-365">Locked deployment files</span></span>

<span data-ttu-id="0b940-366">Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı.</span><span class="sxs-lookup"><span data-stu-id="0b940-366">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="0b940-367">Dağıtım sırasında kilitli dosyalar üzerine yazılamaz.</span><span class="sxs-lookup"><span data-stu-id="0b940-367">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="0b940-368">Uygulama havuzunu kullanan bir dağıtımda kilitli dosyalar serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="0b940-368">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="0b940-369">Web dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="0b940-369">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="0b940-370">Bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-370">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="0b940-371">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="0b940-371">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="0b940-372">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="0b940-372">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="0b940-373">El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.</span><span class="sxs-lookup"><span data-stu-id="0b940-373">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="0b940-374">Silmek için PowerShell kullanma *app_offline.html* (PowerShell 5 veya üzeri gerekir):</span><span class="sxs-lookup"><span data-stu-id="0b940-374">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="0b940-375">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="0b940-375">Data protection</span></span>

<span data-ttu-id="0b940-376">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="0b940-376">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="0b940-377">Veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma dağıtım betiği ile veya bir kalıcı oluşturmak için kullanıcı kodunda yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="0b940-377">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="0b940-378">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="0b940-378">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="0b940-379">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="0b940-379">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="0b940-380">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="0b940-380">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="0b940-381">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="0b940-381">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="0b940-382">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-382">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="0b940-383">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="0b940-383">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="0b940-384">Veri koruma anahtarı halka kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="0b940-384">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="0b940-385">**Veri koruma kayıt defteri anahtarları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="0b940-385">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="0b940-386">ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="0b940-386">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="0b940-387">Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0b940-387">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="0b940-388">Tek başına, webfarm olmayan IIS yüklemeleri [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) ile ASP.NET Core uygulaması kullanılan her bir uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-388">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="0b940-389">Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0b940-389">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="0b940-390">Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="0b940-390">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="0b940-391">Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-391">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="0b940-392">Varsayılan olarak, veri koruma anahtarları şifreli değildir.</span><span class="sxs-lookup"><span data-stu-id="0b940-392">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="0b940-393">Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="0b940-393">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="0b940-394">X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-394">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="0b940-395">Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: Kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve kullanıcının uygulama çalıştığı tüm makinelerde kullanılabilir emin olun.</span><span class="sxs-lookup"><span data-stu-id="0b940-395">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="0b940-396">Bkz: [ASP.NET Core veri koruma yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="0b940-396">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="0b940-397">**Kullanıcı profili yüklemek için IIS uygulama havuzu yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="0b940-397">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="0b940-398">Bu ayar **işlem modeli** bölümüne **Gelişmiş ayarlar** uygulama havuzu için.</span><span class="sxs-lookup"><span data-stu-id="0b940-398">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="0b940-399">Ayarlama **kullanıcı profilini Yükle** için `True`.</span><span class="sxs-lookup"><span data-stu-id="0b940-399">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="0b940-400">Ayarlandığında `True`, anahtarları kullanıcı profili dizinde depolanır ve kullanıcı hesabı için özel bir anahtarla DPAPI kullanılarak korunan.</span><span class="sxs-lookup"><span data-stu-id="0b940-400">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="0b940-401">Anahtarlar için kaldı *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="0b940-401">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="0b940-402">Uygulama havuzunun [setProfileEnvironment özniteliği](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) etkinleştirilmiş olması da gerekir.</span><span class="sxs-lookup"><span data-stu-id="0b940-402">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="0b940-403">Varsayılan değer olan `setProfileEnvironment` olduğu `true`.</span><span class="sxs-lookup"><span data-stu-id="0b940-403">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="0b940-404">(Örneğin, Windows işletim sistemi), bazı senaryolarda `setProfileEnvironment` ayarlanır `false`.</span><span class="sxs-lookup"><span data-stu-id="0b940-404">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="0b940-405">Kullanıcı profili dizinde anahtarları depolanmaz, beklenen:</span><span class="sxs-lookup"><span data-stu-id="0b940-405">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="0b940-406">Gidin *%windir%/system32/inetsrv/config* klasör.</span><span class="sxs-lookup"><span data-stu-id="0b940-406">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="0b940-407">Açık *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="0b940-407">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="0b940-408">Bulun `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="0b940-408">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="0b940-409">Onaylayın `setProfileEnvironment` özniteliği mevcut olmadığında, bunun varsayılan değeri için `true`, veya özniteliğin değerini açık olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="0b940-409">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="0b940-410">**Dosya sistemi anahtarı halkası deposu olarak kullanın**</span><span class="sxs-lookup"><span data-stu-id="0b940-410">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="0b940-411">Uygulama kodu ayarlamak [dosya sistemi bir anahtar halkası deposu olarak kullanırsınız](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="0b940-411">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="0b940-412">Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="0b940-412">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="0b940-413">Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.</span><span class="sxs-lookup"><span data-stu-id="0b940-413">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="0b940-414">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="0b940-414">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="0b940-415">Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="0b940-415">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="0b940-416">X X509 dağıtma her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="0b940-416">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="0b940-417">Yapılandırma [kod veri koruması](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="0b940-417">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="0b940-418">**Veri koruma için makine genelinde bir ilke ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="0b940-418">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="0b940-419">Veri koruma sisteminde bir varsayılan ayarı desteği sınırlıdır [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'lerini kullanan tüm uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="0b940-419">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="0b940-420">Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="0b940-420">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="0b940-421">Sanal dizinler</span><span class="sxs-lookup"><span data-stu-id="0b940-421">Virtual Directories</span></span>

<span data-ttu-id="0b940-422">[IIS sanal dizinlerinin](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ile ASP.NET Core uygulamaları desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="0b940-422">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="0b940-423">Bir uygulama olarak barındırılan bir [alt uygulama](#sub-applications).</span><span class="sxs-lookup"><span data-stu-id="0b940-423">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="0b940-424">Alt uygulamalar</span><span class="sxs-lookup"><span data-stu-id="0b940-424">Sub-applications</span></span>

<span data-ttu-id="0b940-425">ASP.NET Core uygulaması olarak barındırılan bir [IIS alt uygulama (uygulama içi sub)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span><span class="sxs-lookup"><span data-stu-id="0b940-425">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="0b940-426">Sub uygulamanın yolu, uygulama kök URL'SİNİN bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="0b940-426">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0b940-427">Bir alt uygulama ASP.NET Core modülü bir işleyici içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="0b940-427">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="0b940-428">Modül bir alt uygulamasının işleyici olarak eklenip eklenmediğini *web.config* dosyası bir *iç sunucu hatası 500.19* hatalı yapılandırma dosyasına başvuran alındığında alt uygulama göz atmak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="0b940-428">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="0b940-429">Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosyası için bir ASP.NET Core alt uygulama:</span><span class="sxs-lookup"><span data-stu-id="0b940-429">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="0b940-430">ASP.NET Core uygulaması altında ASP.NET Core sub-uygulama barındırma, devralınan işleyici sub-uygulamanın açıkça Kaldır *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="0b940-430">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="0b940-431">Alt uygulama içindeki statik varlık bağlantılar tilde eğik çizgi kullanılmalıdır (`~/`) gösterimi.</span><span class="sxs-lookup"><span data-stu-id="0b940-431">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="0b940-432">Tilde eğik çizgi gösterimi Tetikleyiciler bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro) işlenmiş göreli bağlantısını için alt-uygulamanın pathbase önüne eklediğinizden.</span><span class="sxs-lookup"><span data-stu-id="0b940-432">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="0b940-433">Alt uygulama için `/subapp_path`, bir görüntü ile bağlantılı `src="~/image.png"` olarak işlenen `src="/subapp_path/image.png"`.</span><span class="sxs-lookup"><span data-stu-id="0b940-433">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="0b940-434">Kök uygulamanın statik dosya ara yazılımlarını statik dosya istek işlemiyor.</span><span class="sxs-lookup"><span data-stu-id="0b940-434">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="0b940-435">İstek, alt uygulamanın statik dosya ara yazılımı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="0b940-435">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="0b940-436">Statik bir varlık, ın `src` özniteliği için mutlak bir yol ayarlayın (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase işlenir.</span><span class="sxs-lookup"><span data-stu-id="0b940-436">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="0b940-437">Kök uygulamanın statik dosya ara yazılımlarını kök uygulamanın varlığından hizmet dener [web kök](xref:fundamentals/index#web-root), hangi sonuçlanıyor bir *404 - Bulunamadı* yanıt statik varlık kök uygulama kullanılabilir değilse.</span><span class="sxs-lookup"><span data-stu-id="0b940-437">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="0b940-438">ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:</span><span class="sxs-lookup"><span data-stu-id="0b940-438">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="0b940-439">Alt uygulama için bir uygulama havuzu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0b940-439">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="0b940-440">Ayarlama **.NET CLR sürümü** için **yönetilen kod yok**.</span><span class="sxs-lookup"><span data-stu-id="0b940-440">Set the **.NET CLR Version** to **No Managed Code**.</span></span>

1. <span data-ttu-id="0b940-441">IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0b940-441">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="0b940-442">IIS Yöneticisi'nde alt uygulama klasörüne sağ tıklayıp **uygulamasına dönüştürün**.</span><span class="sxs-lookup"><span data-stu-id="0b940-442">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="0b940-443">İçinde **uygulama Ekle** iletişim kutusunda, kullanmak **seçin** için düğme **uygulama havuzu** alt uygulama için oluşturduğunuz uygulama havuzuna atanamıyor.</span><span class="sxs-lookup"><span data-stu-id="0b940-443">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="0b940-444">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="0b940-444">Select **OK**.</span></span>

<span data-ttu-id="0b940-445">Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.</span><span class="sxs-lookup"><span data-stu-id="0b940-445">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="0b940-446">Barındırma modeli ve ASP.NET Core modülü yapılandırma işlem hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module> ve <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="0b940-446">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="0b940-447">Web.config ile IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0b940-447">Configuration of IIS with web.config</span></span>

<span data-ttu-id="0b940-448">IIS yapılandırması tarafından etkilenir `<system.webServer>` bölümünü *web.config* işlevsel ASP.NET Core modülü ile ASP.NET Core uygulamaları için IIS senaryolar için.</span><span class="sxs-lookup"><span data-stu-id="0b940-448">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="0b940-449">Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır.</span><span class="sxs-lookup"><span data-stu-id="0b940-449">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="0b940-450">IIS dinamik sıkıştırması kullanmak için sunucu düzeyinde yapılandırılmışsa `<urlCompression>` uygulamanın öğesinde *web.config* dosya devre dışı bırakabilir, ASP.NET Core uygulaması için.</span><span class="sxs-lookup"><span data-stu-id="0b940-450">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="0b940-451">Daha fazla bilgi için [yapılandırma başvurusu için \<system.webServer >](/iis/configuration/system.webServer/), [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module), ve [ASP.NET içeren IIS modülleri Çekirdek](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="0b940-451">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="0b940-452">(IIS 10.0 veya üzeri desteklenir) yalıtılmış uygulama havuzlarında çalışmakta olan tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgeleri.</span><span class="sxs-lookup"><span data-stu-id="0b940-452">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="0b940-453">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="0b940-453">Configuration sections of web.config</span></span>

<span data-ttu-id="0b940-454">ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma için ASP.NET Core uygulamaları tarafından kullanılmaz:</span><span class="sxs-lookup"><span data-stu-id="0b940-454">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="0b940-455">ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="0b940-455">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="0b940-456">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="0b940-456">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="0b940-457">Uygulama havuzları</span><span class="sxs-lookup"><span data-stu-id="0b940-457">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0b940-458">Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="0b940-458">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="0b940-459">İşlemdeki barındırma &ndash; uygulamalarını ayrı uygulama havuzlarında çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="0b940-459">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="0b940-460">Barındırma işlemi çıkış &ndash; her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="0b940-460">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="0b940-461">IIS **Web sitesi Ekle** iletişim varsayılan olarak bir uygulama başına tek bir uygulama havuzu.</span><span class="sxs-lookup"><span data-stu-id="0b940-461">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="0b940-462">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="0b940-462">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="0b940-463">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0b940-463">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0b940-464">Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="0b940-464">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="0b940-465">IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="0b940-465">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="0b940-466">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="0b940-466">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="0b940-467">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0b940-467">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="0b940-468">Uygulama havuzu kimliği</span><span class="sxs-lookup"><span data-stu-id="0b940-468">Application Pool Identity</span></span>

<span data-ttu-id="0b940-469">Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="0b940-469">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="0b940-470">IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="0b940-470">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="0b940-471">IIS Yönetim konsolunda altında **Gelişmiş ayarlar** emin olmak için uygulama havuzu, **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="0b940-471">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="0b940-473">IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0b940-473">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="0b940-474">Bu kimlik kullanarak kaynakları güvenliği sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-474">Resources can be secured using this identity.</span></span> <span data-ttu-id="0b940-475">Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.</span><span class="sxs-lookup"><span data-stu-id="0b940-475">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="0b940-476">IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0b940-476">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="0b940-477">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="0b940-477">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="0b940-478">Sağ tıklatın ve dizin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="0b940-478">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="0b940-479">Altında **güvenlik** sekmesinde **Düzenle** düğmesine ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0b940-479">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="0b940-480">Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="0b940-480">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="0b940-481">ENTER **IIS uygulama havuzu\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alan.</span><span class="sxs-lookup"><span data-stu-id="0b940-481">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="0b940-482">Seçin **Adları Denetle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0b940-482">Select the **Check Names** button.</span></span> <span data-ttu-id="0b940-483">İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="0b940-483">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="0b940-484">Zaman **Adları Denetle** düğmesi seçili değerini **DefaultAppPool** nesne adları alanında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="0b940-484">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="0b940-485">Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="0b940-485">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="0b940-486">Kullanım **IIS uygulama havuzu\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.</span><span class="sxs-lookup"><span data-stu-id="0b940-486">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü için seçin: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="0b940-488">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="0b940-488">Select **OK**.</span></span>

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü için seçin: Nesne adı "DefaultAppPool", "Adları denetle" seçtikten sonra nesne adları alanında gösterilir.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="0b940-490">Okuma &amp; Yürütme izinleri varsayılan verilmesi.</span><span class="sxs-lookup"><span data-stu-id="0b940-490">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="0b940-491">Gerektiğinde ek izinler sağlar.</span><span class="sxs-lookup"><span data-stu-id="0b940-491">Provide additional permissions as needed.</span></span>

<span data-ttu-id="0b940-492">Erişim de verilebilir bir komut istemi kullanarak **ICACLS** aracı.</span><span class="sxs-lookup"><span data-stu-id="0b940-492">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="0b940-493">Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:</span><span class="sxs-lookup"><span data-stu-id="0b940-493">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="0b940-494">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls) konu.</span><span class="sxs-lookup"><span data-stu-id="0b940-494">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="0b940-495">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="0b940-495">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0b940-496">[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki IIS dağıtım senaryolarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="0b940-496">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="0b940-497">İşlem içi</span><span class="sxs-lookup"><span data-stu-id="0b940-497">In-process</span></span>
  * <span data-ttu-id="0b940-498">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0b940-498">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="0b940-499">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="0b940-499">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="0b940-500">İşlem dışı</span><span class="sxs-lookup"><span data-stu-id="0b940-500">Out-of-process</span></span>
  * <span data-ttu-id="0b940-501">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0b940-501">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="0b940-502">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="0b940-502">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="0b940-503">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="0b940-503">TLS 1.2 or later connection</span></span>

<span data-ttu-id="0b940-504">Bir HTTP/2 bağlantı kurulduğunda, işlem içi dağıtımı için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="0b940-504">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="0b940-505">Bir HTTP/2 bağlantı kurulduğunda, bir işlem dışı dağıtım için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="0b940-505">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="0b940-506">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="0b940-506">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0b940-507">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki temel gereksinimlere işlem dışı dağıtımları için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="0b940-507">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="0b940-508">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0b940-508">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="0b940-509">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="0b940-509">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="0b940-510">Hedef çerçeve: HTTP/2 bağlantı beri işlem dışı dağıtımlar için geçerli değildir tamamen IIS tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="0b940-510">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="0b940-511">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="0b940-511">TLS 1.2 or later connection</span></span>

<span data-ttu-id="0b940-512">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="0b940-512">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="0b940-513">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="0b940-513">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="0b940-514">Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner.</span><span class="sxs-lookup"><span data-stu-id="0b940-514">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="0b940-515">IIS dağıtımları olan HTTP/2 yapılandırma hakkında daha fazla bilgi için bkz. [HTTP/2 IIS'de](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="0b940-515">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="0b940-516">CORS denetim öncesi isteği</span><span class="sxs-lookup"><span data-stu-id="0b940-516">CORS preflight requests</span></span>

<span data-ttu-id="0b940-517">*Bu bölüm, yalnızca .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="0b940-517">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="0b940-518">ASP.NET Core uygulaması için hedefleyen .NET Framework'IIS içinde varsayılan olarak OPTIONS istekleri uygulamaya geçirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="0b940-518">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="0b940-519">Uygulamanın IIS işleyicileri yapılandırma hakkında bilgi edinmek için *web.config* seçenekleri isteklerini iletmek için bkz: [ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme: CORS işleyişi](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span><span class="sxs-lookup"><span data-stu-id="0b940-519">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="0b940-520">IIS Yöneticiler için dağıtım kaynakları</span><span class="sxs-lookup"><span data-stu-id="0b940-520">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="0b940-521">IIS IIS belgelerinde kapsamlı hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="0b940-521">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="0b940-522">IIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="0b940-522">IIS documentation</span></span>](/iis)

<span data-ttu-id="0b940-523">.NET Core uygulaması dağıtım modelleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="0b940-523">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="0b940-524">.NET core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="0b940-524">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="0b940-525">ASP.NET Core modülü Kestrel web sunucusu, bir ters proxy sunucusu olarak IIS veya IIS Express kullanacak şekilde nasıl olanak tanıdığını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0b940-525">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="0b940-526">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="0b940-526">ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="0b940-527">ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0b940-527">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="0b940-528">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="0b940-528">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="0b940-529">Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="0b940-529">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="0b940-530">Dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="0b940-530">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="0b940-531">ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri keşfedin.</span><span class="sxs-lookup"><span data-stu-id="0b940-531">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="0b940-532">IIS modülleri</span><span class="sxs-lookup"><span data-stu-id="0b940-532">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="0b940-533">ASP.NET Core uygulamaları IIS dağıtımları sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="0b940-533">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="0b940-534">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="0b940-534">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="0b940-535">Sık karşılaşılan barındırırken IIS üzerinde ASP.NET Core uygulamaları ayırmak.</span><span class="sxs-lookup"><span data-stu-id="0b940-535">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="0b940-536">Azure App Service ve IIS için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="0b940-536">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="0b940-537">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0b940-537">Additional resources</span></span>

* <xref:test/troubleshoot>
* [<span data-ttu-id="0b940-538">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="0b940-538">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="0b940-539">Resmi Microsoft IIS sitesi</span><span class="sxs-lookup"><span data-stu-id="0b940-539">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="0b940-540">Windows Server Teknik İçerik Kitaplığı</span><span class="sxs-lookup"><span data-stu-id="0b940-540">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="0b940-541">IIS HTTP/2</span><span class="sxs-lookup"><span data-stu-id="0b940-541">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
