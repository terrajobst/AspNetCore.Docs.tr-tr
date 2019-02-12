---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/iis/index
ms.openlocfilehash: dfb67cd4c2a3f0e6fb270eb2e4850a664cdf5741
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56103169"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="fda78-103">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="fda78-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="fda78-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fda78-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="fda78-105">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="fda78-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="fda78-106">Desteklenen işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="fda78-106">Supported operating systems</span></span>

<span data-ttu-id="fda78-107">Aşağıdaki işletim sistemleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="fda78-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="fda78-108">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fda78-108">Windows 7 or later</span></span>
* <span data-ttu-id="fda78-109">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fda78-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="fda78-110">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla çağrılan WebListener), ııs'li bir ters proxy yapılandırması çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="fda78-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="fda78-111">Kullanım [Kestrel sunucu](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="fda78-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="fda78-112">Azure'da barındırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="fda78-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="fda78-113">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="fda78-113">Supported platforms</span></span>

<span data-ttu-id="fda78-114">(X86) 32-bit ve 64-bit (x 64) dağıtım desteklenen yayımlanan uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="fda78-114">Apps published for 32-bit (x86) and 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="fda78-115">32 bit uygulama sürece dağıtma uygulama:</span><span class="sxs-lookup"><span data-stu-id="fda78-115">Deploy a 32-bit app unless the app:</span></span>

* <span data-ttu-id="fda78-116">Bir 64-bit uygulamalar tarafından kullanılabilecek daha büyük sanal bellek adres alanı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fda78-116">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="fda78-117">Daha büyük IIS yığın boyutu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fda78-117">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="fda78-118">64-bit yerel bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="fda78-118">Has 64-bit native dependencies.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="fda78-119">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fda78-119">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="fda78-120">IISIntegration bileşenlerini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="fda78-120">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fda78-121">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="fda78-121">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fda78-122">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="fda78-122">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fda78-123">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="fda78-123">**In-process hosting model**</span></span>

<span data-ttu-id="fda78-124">`CreateDefaultBuilder` çağrıları `UseIIS` önyükleme yöntemi [CoreCLR](/dotnet/standard/glossary#coreclr) ve IIS çalışan işlemi uygulama barındırın (*w3wp.exe* veya *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="fda78-124">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="fda78-125">Performans testleri belirten bir .NET Core uygulaması işlem içi barındırma için uygulama işlem dışı ve proxy isteklerini barındırma kıyasla önemli ölçüde daha yüksek istek üretilen işini teslim [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="fda78-125">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="fda78-126">İşlem içi barındırma modeli, .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="fda78-126">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="fda78-127">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="fda78-127">**Out-of-process hosting model**</span></span>

<span data-ttu-id="fda78-128">IIS ile işlem dışı barındırmak için `CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fda78-128">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="fda78-129">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fda78-129">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="fda78-130">`CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fda78-130">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="fda78-131">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="fda78-131">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="fda78-132">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="fda78-132">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="fda78-133">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="fda78-133">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="fda78-134">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="fda78-134">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="fda78-135">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="fda78-135">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="fda78-136">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fda78-136">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="fda78-137">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirttiğiniz bağlantı noktalarındaki Kestrel dinlediği.</span><span class="sxs-lookup"><span data-stu-id="fda78-137">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="fda78-138">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fda78-138">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="fda78-139">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fda78-139">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="fda78-140">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fda78-140">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="fda78-141">`CreateDefaultBuilder` çağrıları [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fda78-141">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="fda78-142">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="fda78-142">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="fda78-143">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="fda78-143">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="fda78-144">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="fda78-144">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="fda78-145">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="fda78-145">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="fda78-146">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="fda78-146">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="fda78-147">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fda78-147">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="fda78-148">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fda78-148">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fda78-149">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fda78-149">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="fda78-150">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fda78-150">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="fda78-151">`CreateDefaultBuilder` çağrıları [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fda78-151">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="fda78-152">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="fda78-152">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="fda78-153">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="fda78-153">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="fda78-154">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="fda78-154">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="fda78-155">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="fda78-155">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="fda78-156">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="fda78-156">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="fda78-157">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fda78-157">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="fda78-158">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fda78-158">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fda78-159">Bir bağımlılık dahil [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıklarını bir pakette.</span><span class="sxs-lookup"><span data-stu-id="fda78-159">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="fda78-160">Ekleyerek IIS tümleştirme Ara yazılımları kullanmayı [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) genişletme yöntemi için [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span><span class="sxs-lookup"><span data-stu-id="fda78-160">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="fda78-161">Her ikisi de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) ve [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fda78-161">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="fda78-162">Kod arama `UseIISIntegration` kod taşınabilirliği etkilemez.</span><span class="sxs-lookup"><span data-stu-id="fda78-162">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="fda78-163">Uygulamanın IIS çalıştırırsanız değildir (örneğin, uygulamayı doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="fda78-163">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="fda78-164">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fda78-164">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="fda78-165">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="fda78-165">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="fda78-166">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="fda78-166">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="fda78-167">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="fda78-167">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="fda78-168">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="fda78-168">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="fda78-169">Bir çağrı `UseUrls` Modülü'nü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fda78-169">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="fda78-170">Varsa `UseUrls` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fda78-170">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="fda78-171">`UseUrls` Olan bir ASP.NET Core 1.0 uygulamada çağrılır, çağrı **önce** çağırma `UseIISIntegration` böylece modül yapılandırılan bağlantı noktası üzerine değil.</span><span class="sxs-lookup"><span data-stu-id="fda78-171">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="fda78-172">Ayarı modülü geçersiz kıldığından bu arama sırası ASP.NET Core 1.1 ile gerekli değildir `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fda78-172">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="fda78-173">Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core ana](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="fda78-173">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="fda78-174">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="fda78-174">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fda78-175">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="fda78-175">**In-process hosting model**</span></span>

<span data-ttu-id="fda78-176">IIS sunucusu seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) içinde [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="fda78-176">To configure IIS Server options, include a service configuration for [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="fda78-177">Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="fda78-177">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="fda78-178">Seçenek</span><span class="sxs-lookup"><span data-stu-id="fda78-178">Option</span></span>                         | <span data-ttu-id="fda78-179">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="fda78-179">Default</span></span> | <span data-ttu-id="fda78-180">Ayar</span><span class="sxs-lookup"><span data-stu-id="fda78-180">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="fda78-181">Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="fda78-181">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="fda78-182">Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="fda78-182">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="fda78-183">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="fda78-183">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="fda78-184">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="fda78-184">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="fda78-185">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fda78-185">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="fda78-186">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="fda78-186">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="fda78-187">IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) içinde [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="fda78-187">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="fda78-188">Aşağıdaki örnek uygulamayı doldurmasını önler `HttpContext.Connection.ClientCertificate`:</span><span class="sxs-lookup"><span data-stu-id="fda78-188">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="fda78-189">Seçenek</span><span class="sxs-lookup"><span data-stu-id="fda78-189">Option</span></span>                         | <span data-ttu-id="fda78-190">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="fda78-190">Default</span></span> | <span data-ttu-id="fda78-191">Ayar</span><span class="sxs-lookup"><span data-stu-id="fda78-191">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="fda78-192">Varsa `true`, IIS tümleştirme ara yazılımı ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="fda78-192">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="fda78-193">Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="fda78-193">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="fda78-194">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="fda78-194">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="fda78-195">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu.</span><span class="sxs-lookup"><span data-stu-id="fda78-195">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="fda78-196">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fda78-196">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="fda78-197">Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="fda78-197">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="fda78-198">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="fda78-198">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="fda78-199">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="fda78-199">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="fda78-200">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-200">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="fda78-201">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="fda78-201">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="fda78-202">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="fda78-202">web.config file</span></span>

<span data-ttu-id="fda78-203">*Web.config* dosya yapılandırır [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fda78-203">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="fda78-204">Oluşturma, dönüştürme ve yayımlama *web.config* dosya bir MSBuild hedefi tarafından gerçekleştirilir (`_TransformWebConfig`) projeyi ne zaman yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="fda78-204">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="fda78-205">Bu Web SDK'sı hedeflerin mevcut hedefidir (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="fda78-205">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="fda78-206">SDK'sı, proje dosyasının en üstüne ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="fda78-206">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="fda78-207">Varsa bir *web.config* dosyası projedeki mevcut değil, dosyanın doğru oluşturulması *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modül](xref:host-and-deploy/aspnet-core-module) ve taşınabilir [yayımlanmış çıktısı](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="fda78-207">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="fda78-208">Varsa bir *web.config* dosyası projesinde, dosyanın doğru dönüştürülür *processPath* ve *bağımsız değişkenleri* ASP.NET Core modülü yapılandırmak için ve azure'a taşındı yayımlanmış çıktısı.</span><span class="sxs-lookup"><span data-stu-id="fda78-208">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="fda78-209">Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="fda78-209">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="fda78-210">*Web.config* etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları dosyası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-210">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="fda78-211">ASP.NET Core uygulamaları istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.</span><span class="sxs-lookup"><span data-stu-id="fda78-211">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="fda78-212">Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın  **\<IsTransformWebConfigDisabled >** özelliği proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="fda78-212">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="fda78-213">Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fda78-213">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="fda78-214">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fda78-214">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="fda78-215">Web.config dosyası konumu</span><span class="sxs-lookup"><span data-stu-id="fda78-215">web.config file location</span></span>

<span data-ttu-id="fda78-216">Ayarlamak için [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) doğru *web.config* dosya dağıtılan uygulamayı içerik kök yolda (genellikle uygulama temel yolu) mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fda78-216">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="fda78-217">IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur.</span><span class="sxs-lookup"><span data-stu-id="fda78-217">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="fda78-218">*Web.config* dosya, Web dağıtımı kullanarak birden fazla uygulama yayımlamayı etkinleştirmek için uygulamanın kök dizininde gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fda78-218">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="fda78-219">Mevcut uygulamanın fiziksel yola gibi hassas dosyalar  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="fda78-219">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="fda78-220">Zaman *web.config* dosya varsa ve ve site normalde başlatır, bunların istenmesi halinde, IIS bu hassas dosyalar hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="fda78-220">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="fda78-221">Varsa *web.config* dosyası eksik, yanlış adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet verebilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-221">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="fda78-222">***Web.config* doğru adlı, dağıtımdaki her zaman mevcut ve yukarı site normal başlangıç için yapılandırmak için dosya olmalıdır. Hiçbir zaman Kaldır *web.config* dosyasından bir üretim dağıtımı.**</span><span class="sxs-lookup"><span data-stu-id="fda78-222">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="fda78-223">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fda78-223">IIS configuration</span></span>

<span data-ttu-id="fda78-224">**Windows Server işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="fda78-224">**Windows Server operating systems**</span></span>

<span data-ttu-id="fda78-225">Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fda78-225">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="fda78-226">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="fda78-226">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="fda78-227">Üzerinde **sunucu rolleri** kutusunu işaretleyin, adım **Web sunucusu (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="fda78-227">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="fda78-229">Sonra **özellikleri** adım **rol hizmetlerini** adım, Web sunucusu (IIS) yükler.</span><span class="sxs-lookup"><span data-stu-id="fda78-229">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="fda78-230">İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="fda78-230">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="fda78-232">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="fda78-232">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="fda78-233">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="fda78-233">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="fda78-234">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="fda78-234">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="fda78-235">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="fda78-235">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="fda78-236">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="fda78-236">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="fda78-237">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="fda78-237">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="fda78-238">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="fda78-238">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="fda78-239">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="fda78-239">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="fda78-240">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="fda78-240">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="fda78-241">Aracılığıyla devam **onay** Hizmetleri ve web sunucusu rolü yüklemek için adım.</span><span class="sxs-lookup"><span data-stu-id="fda78-241">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="fda78-242">Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değilse **Web sunucusu (IIS)** rol.</span><span class="sxs-lookup"><span data-stu-id="fda78-242">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="fda78-243">**Windows masaüstü işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="fda78-243">**Windows desktop operating systems**</span></span>

<span data-ttu-id="fda78-244">Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="fda78-244">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="fda78-245">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="fda78-245">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="fda78-246">Açık **Internet Information Services** düğümü.</span><span class="sxs-lookup"><span data-stu-id="fda78-246">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="fda78-247">Açık **Web yönetimi araçları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="fda78-247">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="fda78-248">İçin kutuyu **IIS Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="fda78-248">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="fda78-249">İçin kutuyu **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="fda78-249">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="fda78-250">Varsayılan özelliklerini kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="fda78-250">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="fda78-251">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="fda78-251">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="fda78-252">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="fda78-252">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="fda78-253">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="fda78-253">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="fda78-254">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="fda78-254">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="fda78-255">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="fda78-255">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="fda78-256">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="fda78-256">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="fda78-257">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="fda78-257">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="fda78-258">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="fda78-258">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="fda78-259">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="fda78-259">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="fda78-260">IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="fda78-260">If the IIS installation requires a restart, restart the system.</span></span>

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="fda78-262">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="fda78-262">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="fda78-263">Yükleme *.NET Core barındırma paket* barındıran sistemde.</span><span class="sxs-lookup"><span data-stu-id="fda78-263">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="fda78-264">.NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fda78-264">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="fda78-265">Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="fda78-265">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="fda78-266">Sistem, Internet bağlantısı yoksa, alma ve yükleme [Microsoft Visual C++ 2015 yeniden dağıtılabilir](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core barındırma paketini yüklemeden önce.</span><span class="sxs-lookup"><span data-stu-id="fda78-266">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fda78-267">Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fda78-267">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="fda78-268">IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fda78-268">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="fda78-269">Doğrudan indirme (geçerli sürüm)</span><span class="sxs-lookup"><span data-stu-id="fda78-269">Direct download (current version)</span></span>

<span data-ttu-id="fda78-270">Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:</span><span class="sxs-lookup"><span data-stu-id="fda78-270">Download the installer using the following link:</span></span>

[<span data-ttu-id="fda78-271">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="fda78-271">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="fda78-272">Yükleyici önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="fda78-272">Earlier versions of the installer</span></span>

<span data-ttu-id="fda78-273">Yükleyici önceki bir sürümünü almak için:</span><span class="sxs-lookup"><span data-stu-id="fda78-273">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="fda78-274">Gidin [.NET indirme arşivleri](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="fda78-274">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="fda78-275">Altında **.NET Core**, .NET Core sürümünüzü seçin.</span><span class="sxs-lookup"><span data-stu-id="fda78-275">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="fda78-276">İçinde **uygulamaları - çalışma zamanı çalıştırma** sütun, istenen .NET Core çalışma zamanı sürümü satırını bulur.</span><span class="sxs-lookup"><span data-stu-id="fda78-276">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="fda78-277">Kullanarak yükleyiciyi indirin **çalışma zamanı & paketi barındırma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="fda78-277">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="fda78-278">Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="fda78-278">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="fda78-279">Daha fazla bilgi için [Destek İlkesi](https://www.microsoft.com/net/download/dotnet-core/2.0).</span><span class="sxs-lookup"><span data-stu-id="fda78-279">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="fda78-280">Barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="fda78-280">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="fda78-281">Sunucuda yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fda78-281">Run the installer on the server.</span></span> <span data-ttu-id="fda78-282">Yükleyiciyi bir yönetici komut isteminden çalıştırırken aşağıdaki anahtarlar kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fda78-282">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="fda78-283">`OPT_NO_ANCM=1` &ndash; ASP.NET Core modülü yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="fda78-283">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="fda78-284">`OPT_NO_RUNTIME=1` &ndash; .NET Core çalışma zamanı yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="fda78-284">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="fda78-285">`OPT_NO_SHAREDFX=1` &ndash; ASP.NET paylaşılan Framework (ASP.NET çalışma zamanı) yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="fda78-285">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="fda78-286">`OPT_NO_X86=1` &ndash; X86 yükleme atlanıyor çalışma zamanları.</span><span class="sxs-lookup"><span data-stu-id="fda78-286">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="fda78-287">32-bit uygulamaları barındırma gerekmez, bildiğiniz durumlarda bu anahtarı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fda78-287">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="fda78-288">Hem 32 bit hem de 64-bit uygulamaları gelecekte barındıracak ihtimali varsa, yoksa bu anahtarı kullanın ve her iki çalışma zamanları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="fda78-288">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="fda78-289">Sistemi yeniden başlatın veya yürütme **net stop olan /y** ardından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="fda78-289">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="fda78-290">Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.</span><span class="sxs-lookup"><span data-stu-id="fda78-290">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="fda78-291">Windows barındırma Paket Yükleyici IIS yüklemesini tamamlamak için bir sıfırlama gerektiren algılarsa, yükleyici IIS sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="fda78-291">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="fda78-292">Yükleyici IIS sıfırlama tetiklenirse, tüm Web siteleri ve IIS uygulama havuzları yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="fda78-292">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="fda78-293">IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="fda78-293">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="fda78-294">Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin</span><span class="sxs-lookup"><span data-stu-id="fda78-294">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="fda78-295">Uygulamaları olan sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="fda78-295">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="fda78-296">Web Dağıtımı'nı yüklemek için kullanın [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyicisini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="fda78-296">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="fda78-297">Tercih edilen yöntem, Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="fda78-297">The preferred method is to use WebPI.</span></span> <span data-ttu-id="fda78-298">Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="fda78-298">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="fda78-299">IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="fda78-299">Create the IIS site</span></span>

1. <span data-ttu-id="fda78-300">Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fda78-300">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="fda78-301">Bir uygulamanın dağıtım Düzen açıklanan [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="fda78-301">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="fda78-302">IIS Yöneticisi'nde açın sunucu düğümünde **bağlantıları** paneli.</span><span class="sxs-lookup"><span data-stu-id="fda78-302">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="fda78-303">Sağ **siteleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="fda78-303">Right-click the **Sites** folder.</span></span> <span data-ttu-id="fda78-304">Seçin **Web sitesi Ekle** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="fda78-304">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="fda78-305">Sağlayan bir **Site adı** ayarlayıp **fiziksel yolu** uygulamanın dağıtım klasörüne.</span><span class="sxs-lookup"><span data-stu-id="fda78-305">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="fda78-306">Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="fda78-306">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="fda78-308">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fda78-308">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="fda78-309">Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fda78-309">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="fda78-310">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fda78-310">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="fda78-311">Joker karakterler yerine açık bir ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="fda78-311">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="fda78-312">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="fda78-312">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="fda78-313">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fda78-313">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="fda78-314">Sunucu düğümü altında seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="fda78-314">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="fda78-315">Sitenin uygulama havuzu sağ tıklayıp **temel ayarları** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="fda78-315">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="fda78-316">İçinde **uygulama havuzunu Düzenle** penceresinde **.NET CLR sürümü** için **yönetilen kod yok**:</span><span class="sxs-lookup"><span data-stu-id="fda78-316">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="fda78-318">ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="fda78-318">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="fda78-319">ASP.NET Core, Masaüstü CLR yüklemede de içermez.</span><span class="sxs-lookup"><span data-stu-id="fda78-319">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="fda78-320">Ayarı **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fda78-320">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="fda78-321">*ASP.NET Core 2.2 veya üzeri*: Bir 64-bit (x64) için [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) kullanan [işlem içi barındırma modeli](xref:fundamentals/servers/index#in-process-hosting-model), 32 bit (x 86) işlemleri için uygulama havuzunu devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="fda78-321">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="fda78-322">İçinde **eylemleri** kenar IIS Yöneticisi'nin > **uygulama havuzları**seçin **uygulama havuzu Varsayılanlarını Ayarla** veya **Gelişmiş ayarlar**.</span><span class="sxs-lookup"><span data-stu-id="fda78-322">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="fda78-323">Bulun **etkinleştirme 32-Bit uygulamaları** ve değerine `False`.</span><span class="sxs-lookup"><span data-stu-id="fda78-323">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="fda78-324">Bu ayar için dağıtılan uygulamaları etkilemez [barındırma işlemi çıkış](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="fda78-324">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="fda78-325">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fda78-325">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="fda78-326">Varsayılan uygulama havuzu kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimlik için doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve diğer gerekli kaynakları erişmek için gerekli izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="fda78-326">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="fda78-327">Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fda78-327">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="fda78-328">**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="fda78-328">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="fda78-329">Daha fazla bilgi için [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="fda78-329">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="fda78-330">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="fda78-330">Deploy the app</span></span>

<span data-ttu-id="fda78-331">Uygulamayı barındıran sistemde oluşturduğunuz klasöre dağıtın.</span><span class="sxs-lookup"><span data-stu-id="fda78-331">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="fda78-332">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="fda78-332">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="fda78-333">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="fda78-333">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="fda78-334">Bkz: [uygulama dağıtımı ASP.NET Core için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu için Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fda78-334">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="fda78-335">Barındırma sağlayıcısı oluşturmak için bir yayımlama profili veya destek sağlar, bunların profilini indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="fda78-335">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="fda78-337">Web dağıtımı Visual Studio'nun dışında</span><span class="sxs-lookup"><span data-stu-id="fda78-337">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="fda78-338">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio'nun dışında komut satırından da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-338">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="fda78-339">Daha fazla bilgi için [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="fda78-339">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="fda78-340">Alternatif Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="fda78-340">Alternatives to Web Deploy</span></span>

<span data-ttu-id="fda78-341">Uygulama barındırma sistemin el ile kopyalama, Xcopy, Robocopy ve PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="fda78-341">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="fda78-342">IIS'ye ASP.NET Core dağıtımı hakkında daha fazla bilgi için bkz. [IIS Yöneticiler için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümü.</span><span class="sxs-lookup"><span data-stu-id="fda78-342">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="fda78-343">Web sitesine Gözat</span><span class="sxs-lookup"><span data-stu-id="fda78-343">Browse the website</span></span>

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="fda78-345">Kilitli dağıtım dosyaları</span><span class="sxs-lookup"><span data-stu-id="fda78-345">Locked deployment files</span></span>

<span data-ttu-id="fda78-346">Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı.</span><span class="sxs-lookup"><span data-stu-id="fda78-346">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="fda78-347">Dağıtım sırasında kilitli dosyalar üzerine yazılamaz.</span><span class="sxs-lookup"><span data-stu-id="fda78-347">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="fda78-348">Uygulama havuzunu kullanan bir dağıtımda kilitli dosyalar serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="fda78-348">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="fda78-349">Web dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="fda78-349">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="fda78-350">Bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-350">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="fda78-351">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="fda78-351">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="fda78-352">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="fda78-352">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="fda78-353">El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.</span><span class="sxs-lookup"><span data-stu-id="fda78-353">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="fda78-354">Silmek için PowerShell kullanma *app_offline.html* (PowerShell 5 veya üzeri gerekir):</span><span class="sxs-lookup"><span data-stu-id="fda78-354">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="fda78-355">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="fda78-355">Data protection</span></span>

<span data-ttu-id="fda78-356">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="fda78-356">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="fda78-357">Veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma dağıtım betiği ile veya bir kalıcı oluşturmak için kullanıcı kodunda yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="fda78-357">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="fda78-358">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="fda78-358">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="fda78-359">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="fda78-359">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="fda78-360">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="fda78-360">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="fda78-361">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fda78-361">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="fda78-362">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-362">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="fda78-363">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="fda78-363">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="fda78-364">Veri koruma anahtarı halka kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="fda78-364">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="fda78-365">**Veri koruma kayıt defteri anahtarları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="fda78-365">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="fda78-366">ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="fda78-366">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="fda78-367">Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fda78-367">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="fda78-368">Tek başına, webfarm olmayan IIS yüklemeleri [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) ile ASP.NET Core uygulaması kullanılan her bir uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-368">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="fda78-369">Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fda78-369">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="fda78-370">Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="fda78-370">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="fda78-371">Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-371">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="fda78-372">Varsayılan olarak, veri koruma anahtarları şifreli değildir.</span><span class="sxs-lookup"><span data-stu-id="fda78-372">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="fda78-373">Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="fda78-373">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="fda78-374">X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-374">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="fda78-375">Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: Kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve kullanıcının uygulama çalıştığı tüm makinelerde kullanılabilir emin olun.</span><span class="sxs-lookup"><span data-stu-id="fda78-375">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="fda78-376">Bkz: [ASP.NET Core veri koruma yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="fda78-376">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="fda78-377">**Kullanıcı profili yüklemek için IIS uygulama havuzu yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="fda78-377">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="fda78-378">Bu ayar **işlem modeli** bölümüne **Gelişmiş ayarlar** uygulama havuzu için.</span><span class="sxs-lookup"><span data-stu-id="fda78-378">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="fda78-379">Ayarlanan kullanıcı profilini Yükle `True`.</span><span class="sxs-lookup"><span data-stu-id="fda78-379">Set Load User Profile to `True`.</span></span> <span data-ttu-id="fda78-380">Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI ile uygulama havuzu tarafından kullanılan kullanıcı hesabı için belirli bir anahtar kullanarak.</span><span class="sxs-lookup"><span data-stu-id="fda78-380">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="fda78-381">**Dosya sistemi anahtarı halkası deposu olarak kullanın**</span><span class="sxs-lookup"><span data-stu-id="fda78-381">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="fda78-382">Uygulama kodu ayarlamak [dosya sistemi bir anahtar halkası deposu olarak kullanırsınız](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="fda78-382">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="fda78-383">Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="fda78-383">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="fda78-384">Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.</span><span class="sxs-lookup"><span data-stu-id="fda78-384">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="fda78-385">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="fda78-385">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="fda78-386">Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fda78-386">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="fda78-387">X X509 dağıtma her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="fda78-387">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="fda78-388">Yapılandırma [kod veri koruması](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="fda78-388">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="fda78-389">**Veri koruma için makine genelinde bir ilke ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="fda78-389">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="fda78-390">Veri koruma sisteminde bir varsayılan ayarı desteği sınırlıdır [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'lerini kullanan tüm uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="fda78-390">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="fda78-391">Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="fda78-391">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="fda78-392">Sanal dizinler</span><span class="sxs-lookup"><span data-stu-id="fda78-392">Virtual Directories</span></span>

<span data-ttu-id="fda78-393">[IIS sanal dizinlerinin](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ile ASP.NET Core uygulamaları desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="fda78-393">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="fda78-394">Bir uygulama olarak barındırılan bir [alt uygulama](#sub-applications).</span><span class="sxs-lookup"><span data-stu-id="fda78-394">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="fda78-395">Alt uygulamalar</span><span class="sxs-lookup"><span data-stu-id="fda78-395">Sub-applications</span></span>

<span data-ttu-id="fda78-396">ASP.NET Core uygulaması olarak barındırılan bir [IIS alt uygulama (uygulama içi sub)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span><span class="sxs-lookup"><span data-stu-id="fda78-396">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="fda78-397">Sub uygulamanın yolu, uygulama kök URL'SİNİN bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="fda78-397">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fda78-398">Bir alt uygulama ASP.NET Core modülü bir işleyici içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="fda78-398">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="fda78-399">Modül bir alt uygulamasının işleyici olarak eklenip eklenmediğini *web.config* dosyası bir *iç sunucu hatası 500.19* hatalı yapılandırma dosyasına başvuran alındığında alt uygulama göz atmak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="fda78-399">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="fda78-400">Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosyası için bir ASP.NET Core alt uygulama:</span><span class="sxs-lookup"><span data-stu-id="fda78-400">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="fda78-401">ASP.NET Core uygulaması altında ASP.NET Core sub-uygulama barındırma, devralınan işleyici sub-uygulamanın açıkça Kaldır *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="fda78-401">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

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

<span data-ttu-id="fda78-402">Alt uygulama içindeki statik varlık bağlantılar tilde eğik çizgi kullanılmalıdır (`~/`) gösterimi.</span><span class="sxs-lookup"><span data-stu-id="fda78-402">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="fda78-403">Tilde eğik çizgi gösterimi Tetikleyiciler bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro) işlenmiş göreli bağlantısını için alt-uygulamanın pathbase önüne eklediğinizden.</span><span class="sxs-lookup"><span data-stu-id="fda78-403">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="fda78-404">Alt uygulama için `/subapp_path`, bir görüntü ile bağlantılı `src="~/image.png"` olarak işlenen `src="/subapp_path/image.png"`.</span><span class="sxs-lookup"><span data-stu-id="fda78-404">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="fda78-405">Kök uygulamanın statik dosya ara yazılımlarını statik dosya istek işlemiyor.</span><span class="sxs-lookup"><span data-stu-id="fda78-405">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="fda78-406">İstek, alt uygulamanın statik dosya ara yazılımı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="fda78-406">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="fda78-407">Statik bir varlık, ın `src` özniteliği için mutlak bir yol ayarlayın (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase işlenir.</span><span class="sxs-lookup"><span data-stu-id="fda78-407">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="fda78-408">Kök uygulamanın statik dosya ara yazılımlarını kök uygulamanın varlığından hizmet dener [webroot](xref:fundamentals/index#web-root-webroot), hangi sonuçlanıyor bir *404 - Bulunamadı* yanıt statik varlık kök uygulama kullanılabilir değilse.</span><span class="sxs-lookup"><span data-stu-id="fda78-408">The root app's Static File Middleware attempts to serve the asset from the root app's [webroot](xref:fundamentals/index#web-root-webroot), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="fda78-409">ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:</span><span class="sxs-lookup"><span data-stu-id="fda78-409">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="fda78-410">Alt uygulama için bir uygulama havuzu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fda78-410">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="fda78-411">Ayarlama **.NET CLR sürümü** için **yönetilen kod yok**.</span><span class="sxs-lookup"><span data-stu-id="fda78-411">Set the **.NET CLR Version** to **No Managed Code**.</span></span>

1. <span data-ttu-id="fda78-412">IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fda78-412">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="fda78-413">IIS Yöneticisi'nde alt uygulama klasörüne sağ tıklayıp **uygulamasına dönüştürün**.</span><span class="sxs-lookup"><span data-stu-id="fda78-413">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="fda78-414">İçinde **uygulama Ekle** iletişim kutusunda, kullanmak **seçin** için düğme **uygulama havuzu** alt uygulama için oluşturduğunuz uygulama havuzuna atanamıyor.</span><span class="sxs-lookup"><span data-stu-id="fda78-414">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="fda78-415">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="fda78-415">Select **OK**.</span></span>

<span data-ttu-id="fda78-416">Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.</span><span class="sxs-lookup"><span data-stu-id="fda78-416">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="fda78-417">Barındırma modeli ve ASP.NET Core modülü yapılandırma işlem hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module> ve <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="fda78-417">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="fda78-418">Web.config ile IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fda78-418">Configuration of IIS with web.config</span></span>

<span data-ttu-id="fda78-419">IIS yapılandırması tarafından etkilenir `<system.webServer>` bölümünü *web.config* işlevsel ASP.NET Core modülü ile ASP.NET Core uygulamaları için IIS senaryolar için.</span><span class="sxs-lookup"><span data-stu-id="fda78-419">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="fda78-420">Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır.</span><span class="sxs-lookup"><span data-stu-id="fda78-420">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="fda78-421">IIS dinamik sıkıştırması kullanmak için sunucu düzeyinde yapılandırılmışsa `<urlCompression>` uygulamanın öğesinde *web.config* dosya devre dışı bırakabilir, ASP.NET Core uygulaması için.</span><span class="sxs-lookup"><span data-stu-id="fda78-421">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="fda78-422">Daha fazla bilgi için [yapılandırma başvurusu için \<system.webServer >](/iis/configuration/system.webServer/), [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module), ve [ASP.NET içeren IIS modülleri Çekirdek](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="fda78-422">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="fda78-423">(IIS 10.0 veya üzeri desteklenir) yalıtılmış uygulama havuzlarında çalışmakta olan tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgeleri.</span><span class="sxs-lookup"><span data-stu-id="fda78-423">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="fda78-424">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="fda78-424">Configuration sections of web.config</span></span>

<span data-ttu-id="fda78-425">ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma için ASP.NET Core uygulamaları tarafından kullanılmaz:</span><span class="sxs-lookup"><span data-stu-id="fda78-425">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="fda78-426">ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="fda78-426">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="fda78-427">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="fda78-427">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="fda78-428">Uygulama havuzları</span><span class="sxs-lookup"><span data-stu-id="fda78-428">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fda78-429">Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="fda78-429">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="fda78-430">İşlemdeki barındırma &ndash; uygulamalarını ayrı uygulama havuzlarında çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fda78-430">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="fda78-431">Barındırma işlemi çıkış &ndash; her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="fda78-431">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="fda78-432">IIS **Web sitesi Ekle** iletişim varsayılan olarak bir uygulama başına tek bir uygulama havuzu.</span><span class="sxs-lookup"><span data-stu-id="fda78-432">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="fda78-433">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="fda78-433">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="fda78-434">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fda78-434">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fda78-435">Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="fda78-435">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="fda78-436">IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="fda78-436">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="fda78-437">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="fda78-437">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="fda78-438">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fda78-438">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="fda78-439">Uygulama havuzu kimliği</span><span class="sxs-lookup"><span data-stu-id="fda78-439">Application Pool Identity</span></span>

<span data-ttu-id="fda78-440">Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="fda78-440">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="fda78-441">IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fda78-441">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="fda78-442">IIS Yönetim konsolunda altında **Gelişmiş ayarlar** emin olmak için uygulama havuzu, **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="fda78-442">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="fda78-444">IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fda78-444">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="fda78-445">Bu kimlik kullanarak kaynakları güvenliği sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-445">Resources can be secured using this identity.</span></span> <span data-ttu-id="fda78-446">Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.</span><span class="sxs-lookup"><span data-stu-id="fda78-446">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="fda78-447">IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="fda78-447">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="fda78-448">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="fda78-448">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="fda78-449">Sağ tıklatın ve dizin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="fda78-449">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="fda78-450">Altında **güvenlik** sekmesinde **Düzenle** düğmesine ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="fda78-450">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="fda78-451">Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="fda78-451">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="fda78-452">ENTER **IIS uygulama havuzu\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alan.</span><span class="sxs-lookup"><span data-stu-id="fda78-452">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="fda78-453">Seçin **Adları Denetle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="fda78-453">Select the **Check Names** button.</span></span> <span data-ttu-id="fda78-454">İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="fda78-454">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="fda78-455">Zaman **Adları Denetle** düğmesi seçili değerini **DefaultAppPool** nesne adları alanında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="fda78-455">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="fda78-456">Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="fda78-456">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="fda78-457">Kullanım **IIS uygulama havuzu\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.</span><span class="sxs-lookup"><span data-stu-id="fda78-457">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü için seçin: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="fda78-459">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="fda78-459">Select **OK**.</span></span>

   ![Kullanıcılar veya gruplar iletişim uygulama klasörü için seçin: Nesne adı "DefaultAppPool", "Adları denetle" seçtikten sonra nesne adları alanında gösterilir.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="fda78-461">Okuma &amp; Yürütme izinleri varsayılan verilmesi.</span><span class="sxs-lookup"><span data-stu-id="fda78-461">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="fda78-462">Gerektiğinde ek izinler sağlar.</span><span class="sxs-lookup"><span data-stu-id="fda78-462">Provide additional permissions as needed.</span></span>

<span data-ttu-id="fda78-463">Erişim de verilebilir bir komut istemi kullanarak **ICACLS** aracı.</span><span class="sxs-lookup"><span data-stu-id="fda78-463">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="fda78-464">Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:</span><span class="sxs-lookup"><span data-stu-id="fda78-464">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="fda78-465">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls) konu.</span><span class="sxs-lookup"><span data-stu-id="fda78-465">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="fda78-466">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="fda78-466">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fda78-467">[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki IIS dağıtım senaryolarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="fda78-467">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="fda78-468">İşlem içi</span><span class="sxs-lookup"><span data-stu-id="fda78-468">In-process</span></span>
  * <span data-ttu-id="fda78-469">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fda78-469">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="fda78-470">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="fda78-470">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="fda78-471">İşlem dışı</span><span class="sxs-lookup"><span data-stu-id="fda78-471">Out-of-process</span></span>
  * <span data-ttu-id="fda78-472">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fda78-472">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="fda78-473">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="fda78-473">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="fda78-474">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="fda78-474">TLS 1.2 or later connection</span></span>

<span data-ttu-id="fda78-475">Bir HTTP/2 bağlantı kurulduğunda, işlem içi dağıtımı için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="fda78-475">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="fda78-476">Bir HTTP/2 bağlantı kurulduğunda, bir işlem dışı dağıtım için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="fda78-476">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="fda78-477">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module> konu ve <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="fda78-477">For more information on the in-process and out-of-process hosting models, see the <xref:host-and-deploy/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fda78-478">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki temel gereksinimlere işlem dışı dağıtımları için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="fda78-478">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="fda78-479">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fda78-479">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="fda78-480">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="fda78-480">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="fda78-481">Hedef çerçeve: HTTP/2 bağlantı beri işlem dışı dağıtımlar için geçerli değildir tamamen IIS tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="fda78-481">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="fda78-482">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="fda78-482">TLS 1.2 or later connection</span></span>

<span data-ttu-id="fda78-483">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="fda78-483">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="fda78-484">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="fda78-484">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="fda78-485">Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner.</span><span class="sxs-lookup"><span data-stu-id="fda78-485">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="fda78-486">IIS dağıtımları olan HTTP/2 yapılandırma hakkında daha fazla bilgi için bkz. [HTTP/2 IIS'de](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="fda78-486">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="fda78-487">CORS denetim öncesi isteği</span><span class="sxs-lookup"><span data-stu-id="fda78-487">CORS preflight requests</span></span>

<span data-ttu-id="fda78-488">*Bu bölüm, yalnızca .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="fda78-488">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="fda78-489">ASP.NET Core uygulaması için hedefleyen .NET Framework'IIS içinde varsayılan olarak OPTIONS istekleri uygulamaya geçirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="fda78-489">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="fda78-490">Uygulamanın IIS işleyicileri yapılandırma hakkında bilgi edinmek için *web.config* seçenekleri isteklerini iletmek için bkz: [ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme: CORS işleyişi](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span><span class="sxs-lookup"><span data-stu-id="fda78-490">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="fda78-491">IIS Yöneticiler için dağıtım kaynakları</span><span class="sxs-lookup"><span data-stu-id="fda78-491">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="fda78-492">IIS IIS belgelerinde kapsamlı hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fda78-492">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="fda78-493">IIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="fda78-493">IIS documentation</span></span>](/iis)

<span data-ttu-id="fda78-494">.NET Core uygulaması dağıtım modelleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fda78-494">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="fda78-495">.NET core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="fda78-495">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="fda78-496">ASP.NET Core modülü Kestrel web sunucusu, bir ters proxy sunucusu olarak IIS veya IIS Express kullanacak şekilde nasıl olanak tanıdığını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fda78-496">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="fda78-497">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="fda78-497">ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="fda78-498">ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fda78-498">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="fda78-499">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="fda78-499">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="fda78-500">Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fda78-500">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="fda78-501">Dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="fda78-501">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="fda78-502">ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri keşfedin.</span><span class="sxs-lookup"><span data-stu-id="fda78-502">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="fda78-503">IIS modülleri</span><span class="sxs-lookup"><span data-stu-id="fda78-503">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="fda78-504">ASP.NET Core uygulamaları IIS dağıtımları sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fda78-504">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="fda78-505">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="fda78-505">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="fda78-506">Sık karşılaşılan barındırırken IIS üzerinde ASP.NET Core uygulamaları ayırmak.</span><span class="sxs-lookup"><span data-stu-id="fda78-506">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="fda78-507">Azure App Service ve IIS için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="fda78-507">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="fda78-508">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fda78-508">Additional resources</span></span>

* <xref:test/troubleshoot>
* [<span data-ttu-id="fda78-509">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="fda78-509">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="fda78-510">Resmi Microsoft IIS sitesi</span><span class="sxs-lookup"><span data-stu-id="fda78-510">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="fda78-511">Windows Server Teknik İçerik Kitaplığı</span><span class="sxs-lookup"><span data-stu-id="fda78-511">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="fda78-512">IIS HTTP/2</span><span class="sxs-lookup"><span data-stu-id="fda78-512">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
