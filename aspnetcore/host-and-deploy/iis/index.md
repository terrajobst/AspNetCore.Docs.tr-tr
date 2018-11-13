---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1b34195dc51ca8dab5e8eda10f05ff6678fbc78c
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570171"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="46b64-103">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="46b64-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="46b64-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="46b64-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="46b64-105">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="46b64-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="46b64-106">Desteklenen işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="46b64-106">Supported operating systems</span></span>

<span data-ttu-id="46b64-107">Aşağıdaki işletim sistemleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="46b64-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="46b64-108">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="46b64-108">Windows 7 or later</span></span>
* <span data-ttu-id="46b64-109">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="46b64-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="46b64-110">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)) IIS ile bir ters proxy yapılandırması çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="46b64-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="46b64-111">Kullanım [Kestrel sunucu](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="46b64-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="46b64-112">Azure'da barındırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="46b64-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="46b64-113">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="46b64-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="46b64-114">IISIntegration bileşenlerini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="46b64-114">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="46b64-115">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="46b64-115">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="46b64-116">Tipik bir *Program.cs* çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="46b64-116">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="46b64-117">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="46b64-117">**In-process hosting model**</span></span>

<span data-ttu-id="46b64-118">`CreateDefaultBuilder` çağrıları `UseIIS` önyükleme yöntemi [CoreCLR](/dotnet/standard/glossary#coreclr) ve IIS çalışan işlemi uygulama barındırın (*w3wp.exe* veya *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="46b64-118">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="46b64-119">Performans testleri belirten bir .NET Core uygulaması işlem içi barındırma için uygulama işlem dışı ve proxy isteklerini barındırma kıyasla önemli ölçüde daha yüksek istek üretilen işini teslim [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="46b64-119">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="46b64-120">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="46b64-120">**Out-of-process hosting model**</span></span>

<span data-ttu-id="46b64-121">IIS ile işlem dışı barındırmak için `CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) temel yolu ve bağlantı noktası için yapılandırarak web sunucusu ve etkinleştirir IIS tümleştirme olarak [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-121">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="46b64-122">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="46b64-122">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="46b64-123">`CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="46b64-123">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="46b64-124">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="46b64-124">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="46b64-125">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="46b64-125">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="46b64-126">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="46b64-126">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="46b64-127">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="46b64-127">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="46b64-128">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="46b64-128">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="46b64-129">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="46b64-129">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="46b64-130">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirttiğiniz bağlantı noktalarındaki Kestrel dinlediği.</span><span class="sxs-lookup"><span data-stu-id="46b64-130">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="46b64-131">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-131">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="46b64-132">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) temel yolu ve bağlantı noktası için yapılandırarak web sunucusu ve etkinleştirir IIS tümleştirme olarak [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-132">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="46b64-133">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="46b64-133">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="46b64-134">`CreateDefaultBuilder` çağrıları [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="46b64-134">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="46b64-135">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="46b64-135">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="46b64-136">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="46b64-136">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="46b64-137">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="46b64-137">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="46b64-138">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="46b64-138">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="46b64-139">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="46b64-139">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="46b64-140">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="46b64-140">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="46b64-141">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="46b64-141">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="46b64-142">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) temel yolu ve bağlantı noktası için yapılandırarak web sunucusu ve etkinleştirir IIS tümleştirme olarak [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-142">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="46b64-143">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="46b64-143">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="46b64-144">`CreateDefaultBuilder` çağrıları [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="46b64-144">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="46b64-145">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="46b64-145">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="46b64-146">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="46b64-146">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="46b64-147">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="46b64-147">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="46b64-148">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="46b64-148">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="46b64-149">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="46b64-149">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="46b64-150">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="46b64-150">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="46b64-151">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="46b64-151">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="46b64-152">Bir bağımlılık dahil [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) uygulamanın bağımlılıklarını bir pakette.</span><span class="sxs-lookup"><span data-stu-id="46b64-152">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="46b64-153">Ekleyerek IIS tümleştirme Ara yazılımları kullanmayı [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) genişletme yöntemi için [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span><span class="sxs-lookup"><span data-stu-id="46b64-153">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="46b64-154">Her ikisi de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) ve [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="46b64-154">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="46b64-155">Kod arama `UseIISIntegration` kod taşınabilirliği etkilemez.</span><span class="sxs-lookup"><span data-stu-id="46b64-155">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="46b64-156">Uygulamanın IIS çalıştırırsanız değildir (örneğin, uygulamayı doğrudan Kestrel üzerinde çalıştırılan) `UseIISIntegration` çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="46b64-156">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="46b64-157">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="46b64-157">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="46b64-158">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="46b64-158">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="46b64-159">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="46b64-159">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="46b64-160">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="46b64-160">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="46b64-161">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="46b64-161">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="46b64-162">Bir çağrı `UseUrls` Modülü'nü kullanırken gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="46b64-162">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="46b64-163">Varsa `UseUrls` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="46b64-163">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="46b64-164">`UseUrls` Olan bir ASP.NET Core 1.0 uygulamada çağrılır, çağrı **önce** çağırma `UseIISIntegration` böylece modül yapılandırılan bağlantı noktası üzerine değil.</span><span class="sxs-lookup"><span data-stu-id="46b64-164">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="46b64-165">Ayarı modülü geçersiz kıldığından bu arama sırası ASP.NET Core 1.1 ile gerekli değildir `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="46b64-165">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="46b64-166">Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core ana](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="46b64-166">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="46b64-167">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="46b64-167">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="46b64-168">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="46b64-168">**In-process hosting model**</span></span>

<span data-ttu-id="46b64-169">IIS sunucusu seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) içinde [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="46b64-169">To configure IIS Server options, include a service configuration for [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="46b64-170">Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="46b64-170">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="46b64-171">Seçenek</span><span class="sxs-lookup"><span data-stu-id="46b64-171">Option</span></span>                         | <span data-ttu-id="46b64-172">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="46b64-172">Default</span></span> | <span data-ttu-id="46b64-173">Ayar</span><span class="sxs-lookup"><span data-stu-id="46b64-173">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="46b64-174">Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="46b64-174">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="46b64-175">Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="46b64-175">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="46b64-176">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="46b64-176">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="46b64-177">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="46b64-177">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="46b64-178">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="46b64-178">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="46b64-179">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="46b64-179">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="46b64-180">IIS seçeneklerini yapılandırmak için bir hizmet yapılandırması dahil [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) içinde [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="46b64-180">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="46b64-181">Aşağıdaki örnek uygulamayı doldurmasını önler `HttpContext.Connection.ClientCertificate`:</span><span class="sxs-lookup"><span data-stu-id="46b64-181">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="46b64-182">Seçenek</span><span class="sxs-lookup"><span data-stu-id="46b64-182">Option</span></span>                         | <span data-ttu-id="46b64-183">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="46b64-183">Default</span></span> | <span data-ttu-id="46b64-184">Ayar</span><span class="sxs-lookup"><span data-stu-id="46b64-184">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="46b64-185">Varsa `true`, IIS tümleştirme ara yazılımı ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="46b64-185">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="46b64-186">Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="46b64-186">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="46b64-187">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="46b64-187">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="46b64-188">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu.</span><span class="sxs-lookup"><span data-stu-id="46b64-188">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="46b64-189">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="46b64-189">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="46b64-190">Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="46b64-190">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="46b64-191">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="46b64-191">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="46b64-192">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="46b64-192">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="46b64-193">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-193">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="46b64-194">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="46b64-194">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="46b64-195">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="46b64-195">web.config file</span></span>

<span data-ttu-id="46b64-196">*Web.config* dosya yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-196">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="46b64-197">Oluşturma, dönüştürme ve yayımlama *web.config* dosya bir MSBuild hedefi tarafından gerçekleştirilir (`_TransformWebConfig`) projeyi ne zaman yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="46b64-197">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="46b64-198">Bu Web SDK'sı hedeflerin mevcut hedefidir (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="46b64-198">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="46b64-199">SDK'sı, proje dosyasının en üstüne ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="46b64-199">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="46b64-200">Varsa bir *web.config* dosyası projedeki mevcut değil, dosyanın doğru oluşturulması *processPath* ve *bağımsız değişkenleri* yapılandırmak için [ASP.NET Core Modül](xref:fundamentals/servers/aspnet-core-module) ve taşınabilir [yayımlanmış çıktısı](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="46b64-200">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="46b64-201">Varsa bir *web.config* dosyası projesinde, dosyanın doğru dönüştürülür *processPath* ve *bağımsız değişkenleri* ASP.NET Core modülü yapılandırmak için ve azure'a taşındı yayımlanmış çıktısı.</span><span class="sxs-lookup"><span data-stu-id="46b64-201">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="46b64-202">Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="46b64-202">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="46b64-203">*Web.config* etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları dosyası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-203">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="46b64-204">ASP.NET Core uygulamaları istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.</span><span class="sxs-lookup"><span data-stu-id="46b64-204">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="46b64-205">Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın  **\<IsTransformWebConfigDisabled >** özelliği proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="46b64-205">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="46b64-206">Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="46b64-206">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="46b64-207">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-207">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="46b64-208">Web.config dosyası konumu</span><span class="sxs-lookup"><span data-stu-id="46b64-208">web.config file location</span></span>

<span data-ttu-id="46b64-209">Ayarlamak için [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) doğru *web.config* dosya dağıtılan uygulamayı içerik kök yolda (genellikle uygulama temel yolu) mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="46b64-209">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="46b64-210">IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur.</span><span class="sxs-lookup"><span data-stu-id="46b64-210">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="46b64-211">*Web.config* dosya, Web dağıtımı kullanarak birden fazla uygulama yayımlamayı etkinleştirmek için uygulamanın kök dizininde gereklidir.</span><span class="sxs-lookup"><span data-stu-id="46b64-211">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="46b64-212">Mevcut uygulamanın fiziksel yola gibi hassas dosyalar  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="46b64-212">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="46b64-213">Zaman *web.config* dosya varsa ve ve site normalde başlatır, bunların istenmesi halinde, IIS bu hassas dosyalar hizmet değil.</span><span class="sxs-lookup"><span data-stu-id="46b64-213">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="46b64-214">Varsa *web.config* dosyası eksik, yanlış adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet verebilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-214">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="46b64-215">***Web.config* doğru adlı, dağıtımdaki her zaman mevcut ve yukarı site normal başlangıç için yapılandırmak için dosya olmalıdır. Hiçbir zaman Kaldır *web.config* dosyasından bir üretim dağıtımı.**</span><span class="sxs-lookup"><span data-stu-id="46b64-215">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="46b64-216">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="46b64-216">IIS configuration</span></span>

<span data-ttu-id="46b64-217">**Windows Server işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="46b64-217">**Windows Server operating systems**</span></span>

<span data-ttu-id="46b64-218">Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="46b64-218">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="46b64-219">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="46b64-219">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="46b64-220">Üzerinde **sunucu rolleri** kutusunu işaretleyin, adım **Web sunucusu (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="46b64-220">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="46b64-222">Sonra **özellikleri** adım **rol hizmetlerini** adım, Web sunucusu (IIS) yükler.</span><span class="sxs-lookup"><span data-stu-id="46b64-222">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="46b64-223">İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="46b64-223">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="46b64-225">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="46b64-225">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="46b64-226">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="46b64-226">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="46b64-227">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="46b64-227">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="46b64-228">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="46b64-228">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="46b64-229">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="46b64-229">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="46b64-230">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="46b64-230">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="46b64-231">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="46b64-231">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="46b64-232">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="46b64-232">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="46b64-233">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="46b64-233">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="46b64-234">Aracılığıyla devam **onay** Hizmetleri ve web sunucusu rolü yüklemek için adım.</span><span class="sxs-lookup"><span data-stu-id="46b64-234">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="46b64-235">Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değilse **Web sunucusu (IIS)** rol.</span><span class="sxs-lookup"><span data-stu-id="46b64-235">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="46b64-236">**Windows masaüstü işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="46b64-236">**Windows desktop operating systems**</span></span>

<span data-ttu-id="46b64-237">Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="46b64-237">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="46b64-238">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="46b64-238">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="46b64-239">Açık **Internet Information Services** düğümü.</span><span class="sxs-lookup"><span data-stu-id="46b64-239">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="46b64-240">Açık **Web yönetimi araçları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="46b64-240">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="46b64-241">İçin kutuyu **IIS Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="46b64-241">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="46b64-242">İçin kutuyu **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="46b64-242">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="46b64-243">Varsayılan özelliklerini kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="46b64-243">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="46b64-244">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="46b64-244">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="46b64-245">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="46b64-245">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="46b64-246">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="46b64-246">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="46b64-247">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="46b64-247">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="46b64-248">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="46b64-248">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="46b64-249">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="46b64-249">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="46b64-250">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="46b64-250">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="46b64-251">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="46b64-251">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="46b64-252">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="46b64-252">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="46b64-253">IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="46b64-253">If the IIS installation requires a restart, restart the system.</span></span>

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="46b64-255">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="46b64-255">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="46b64-256">Yükleme *.NET Core barındırma paket* barındıran sistemde.</span><span class="sxs-lookup"><span data-stu-id="46b64-256">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="46b64-257">.NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-257">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="46b64-258">Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="46b64-258">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="46b64-259">Sistem, Internet bağlantısı yoksa, alma ve yükleme [Microsoft Visual C++ 2015 yeniden dağıtılabilir](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core barındırma paketini yüklemeden önce.</span><span class="sxs-lookup"><span data-stu-id="46b64-259">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="46b64-260">Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="46b64-260">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="46b64-261">IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="46b64-261">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="46b64-262">Doğrudan indirme (geçerli sürüm)</span><span class="sxs-lookup"><span data-stu-id="46b64-262">Direct download (current version)</span></span>

<span data-ttu-id="46b64-263">Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:</span><span class="sxs-lookup"><span data-stu-id="46b64-263">Download the installer using the following link:</span></span>

[<span data-ttu-id="46b64-264">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="46b64-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="46b64-265">Yükleyici önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="46b64-265">Earlier versions of the installer</span></span>

<span data-ttu-id="46b64-266">Yükleyici önceki bir sürümünü almak için:</span><span class="sxs-lookup"><span data-stu-id="46b64-266">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="46b64-267">Gidin [.NET indirme arşivleri](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="46b64-267">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="46b64-268">Altında **.NET Core**, .NET Core sürümünüzü seçin.</span><span class="sxs-lookup"><span data-stu-id="46b64-268">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="46b64-269">İçinde **uygulamaları - çalışma zamanı çalıştırma** sütun, istenen .NET Core çalışma zamanı sürümü satırını bulur.</span><span class="sxs-lookup"><span data-stu-id="46b64-269">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="46b64-270">Kullanarak yükleyiciyi indirin **çalışma zamanı & paketi barındırma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="46b64-270">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="46b64-271">Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="46b64-271">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="46b64-272">Daha fazla bilgi için [Destek İlkesi](https://www.microsoft.com/net/download/dotnet-core/2.0).</span><span class="sxs-lookup"><span data-stu-id="46b64-272">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="46b64-273">Barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="46b64-273">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="46b64-274">Sunucuda yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="46b64-274">Run the installer on the server.</span></span> <span data-ttu-id="46b64-275">Yükleyiciyi bir yönetici komut isteminden çalıştırırken aşağıdaki anahtarlar kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="46b64-275">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="46b64-276">`OPT_NO_ANCM=1` &ndash; ASP.NET Core modülü yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="46b64-276">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="46b64-277">`OPT_NO_RUNTIME=1` &ndash; .NET Core çalışma zamanı yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="46b64-277">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="46b64-278">`OPT_NO_SHAREDFX=1` &ndash; ASP.NET paylaşılan Framework (ASP.NET çalışma zamanı) yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="46b64-278">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="46b64-279">`OPT_NO_X86=1` &ndash; X86 yükleme atlanıyor çalışma zamanları.</span><span class="sxs-lookup"><span data-stu-id="46b64-279">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="46b64-280">32-bit uygulamaları barındırma gerekmez, bildiğiniz durumlarda bu anahtarı kullanın.</span><span class="sxs-lookup"><span data-stu-id="46b64-280">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="46b64-281">Hem 32 bit hem de 64-bit uygulamaları gelecekte barındıracak ihtimali varsa, yoksa bu anahtarı kullanın ve her iki çalışma zamanları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="46b64-281">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="46b64-282">Sistemi yeniden başlatın veya yürütme **net stop olan /y** ardından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="46b64-282">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="46b64-283">Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.</span><span class="sxs-lookup"><span data-stu-id="46b64-283">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="46b64-284">Windows barındırma Paket Yükleyici IIS yüklemesini tamamlamak için bir sıfırlama gerektiren algılarsa, yükleyici IIS sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="46b64-284">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="46b64-285">Yükleyici IIS sıfırlama tetiklenirse, tüm Web siteleri ve IIS uygulama havuzları yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="46b64-285">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="46b64-286">IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="46b64-286">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="46b64-287">Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin</span><span class="sxs-lookup"><span data-stu-id="46b64-287">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="46b64-288">Uygulamaları olan sunuculara dağıtırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="46b64-288">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="46b64-289">Web Dağıtımı'nı yüklemek için kullanın [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyicisini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="46b64-289">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="46b64-290">Tercih edilen yöntem, Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="46b64-290">The preferred method is to use WebPI.</span></span> <span data-ttu-id="46b64-291">Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="46b64-291">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="46b64-292">IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="46b64-292">Create the IIS site</span></span>

1. <span data-ttu-id="46b64-293">Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="46b64-293">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="46b64-294">Bir uygulamanın dağıtım Düzen açıklanan [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="46b64-294">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="46b64-295">Yeni bir klasör içinde oluşturun bir *günlükleri* stdout günlüğe kaydetme etkinleştirilmişse ASP.NET Core modülü stdout günlükleri tutmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="46b64-295">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="46b64-296">Uygulama ile dağıtılırsa bir *günlükleri* yükü klasöründe bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="46b64-296">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="46b64-297">Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak projeyi yerel olarak yapılandırıldığında klasörü görmek [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="46b64-297">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="46b64-298">Yalnızca uygulama başlatma hataları gidermek için stdout günlük kullanın.</span><span class="sxs-lookup"><span data-stu-id="46b64-298">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="46b64-299">Hiçbir stdout günlük kaydı için rutin uygulama günlüğü kullanın.</span><span class="sxs-lookup"><span data-stu-id="46b64-299">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="46b64-300">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="46b64-300">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="46b64-301">Uygulama havuzu günlüklerin yazılacağı konumuna yazma erişimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="46b64-301">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="46b64-302">Tüm Günlük konumu yolu klasörlerde mevcut olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="46b64-302">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="46b64-303">Stdout günlüğü hakkında daha fazla bilgi için bkz. [günlük oluşturma ve yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="46b64-303">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="46b64-304">ASP.NET Core uygulaması, günlüğe kaydetme hakkında daha fazla bilgi için bkz: [günlüğü](xref:fundamentals/logging/index) konu.</span><span class="sxs-lookup"><span data-stu-id="46b64-304">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="46b64-305">İçinde **IIS Yöneticisi'ni**, sunucunun düğümünde açın **bağlantıları** paneli.</span><span class="sxs-lookup"><span data-stu-id="46b64-305">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="46b64-306">Sağ **siteleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="46b64-306">Right-click the **Sites** folder.</span></span> <span data-ttu-id="46b64-307">Seçin **Web sitesi Ekle** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="46b64-307">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="46b64-308">Sağlayan bir **Site adı** ayarlayıp **fiziksel yolu** uygulamanın dağıtım klasörüne.</span><span class="sxs-lookup"><span data-stu-id="46b64-308">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="46b64-309">Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="46b64-309">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="46b64-311">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="46b64-311">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="46b64-312">Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46b64-312">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="46b64-313">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="46b64-313">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="46b64-314">Joker karakterler yerine açık bir ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="46b64-314">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="46b64-315">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="46b64-315">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="46b64-316">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="46b64-316">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="46b64-317">Sunucu düğümü altında seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="46b64-317">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="46b64-318">Sitenin uygulama havuzu sağ tıklayıp **temel ayarları** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="46b64-318">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="46b64-319">İçinde **uygulama havuzunu Düzenle** penceresinde **.NET CLR sürümü** için **yönetilen kod yok**:</span><span class="sxs-lookup"><span data-stu-id="46b64-319">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="46b64-321">ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="46b64-321">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="46b64-322">ASP.NET Core, Masaüstü CLR yüklemede de içermez.</span><span class="sxs-lookup"><span data-stu-id="46b64-322">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="46b64-323">Ayarı **.NET CLR sürümü** için **yönetilen kod yok** isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="46b64-323">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="46b64-324">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="46b64-324">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="46b64-325">Varsayılan uygulama havuzu kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimlik için doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve diğer gerekli kaynakları erişmek için gerekli izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="46b64-325">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="46b64-326">Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.</span><span class="sxs-lookup"><span data-stu-id="46b64-326">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="46b64-327">**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="46b64-327">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="46b64-328">Daha fazla bilgi için [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="46b64-328">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="46b64-329">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="46b64-329">Deploy the app</span></span>

<span data-ttu-id="46b64-330">Uygulamayı barındıran sistemde oluşturduğunuz klasöre dağıtın.</span><span class="sxs-lookup"><span data-stu-id="46b64-330">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="46b64-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtımı için önerilen mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="46b64-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="46b64-332">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="46b64-332">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="46b64-333">Bkz: [uygulama dağıtımı ASP.NET Core için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu için Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="46b64-333">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="46b64-334">Barındırma sağlayıcısı oluşturmak için bir yayımlama profili veya destek sağlar, bunların profilini indirin ve Visual Studio kullanarak içe aktarın **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="46b64-334">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="46b64-336">Web dağıtımı Visual Studio'nun dışında</span><span class="sxs-lookup"><span data-stu-id="46b64-336">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="46b64-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio'nun dışında komut satırından da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="46b64-338">Daha fazla bilgi için [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="46b64-338">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="46b64-339">Alternatif Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="46b64-339">Alternatives to Web Deploy</span></span>

<span data-ttu-id="46b64-340">Uygulama barındırma sistemin el ile kopyalama, Xcopy, Robocopy ve PowerShell gibi taşımak için birkaç yöntemden herhangi birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="46b64-340">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="46b64-341">IIS'ye ASP.NET Core dağıtımı hakkında daha fazla bilgi için bkz. [IIS Yöneticiler için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümü.</span><span class="sxs-lookup"><span data-stu-id="46b64-341">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="46b64-342">Web sitesine Gözat</span><span class="sxs-lookup"><span data-stu-id="46b64-342">Browse the website</span></span>

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="46b64-344">Kilitli dağıtım dosyaları</span><span class="sxs-lookup"><span data-stu-id="46b64-344">Locked deployment files</span></span>

<span data-ttu-id="46b64-345">Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı.</span><span class="sxs-lookup"><span data-stu-id="46b64-345">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="46b64-346">Dağıtım sırasında kilitli dosyalar üzerine yazılamaz.</span><span class="sxs-lookup"><span data-stu-id="46b64-346">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="46b64-347">Uygulama havuzunu kullanan bir dağıtımda kilitli dosyalar serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="46b64-347">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="46b64-348">Web dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="46b64-348">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="46b64-349">Bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-349">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="46b64-350">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="46b64-350">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="46b64-351">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="46b64-351">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="46b64-352">El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.</span><span class="sxs-lookup"><span data-stu-id="46b64-352">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="46b64-353">Silmek için PowerShell kullanma *app_offline.html* (PowerShell 5 veya üzeri gerekir):</span><span class="sxs-lookup"><span data-stu-id="46b64-353">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="46b64-354">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="46b64-354">Data protection</span></span>

<span data-ttu-id="46b64-355">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="46b64-355">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="46b64-356">Veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma dağıtım betiği ile veya bir kalıcı oluşturmak için kullanıcı kodunda yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="46b64-356">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="46b64-357">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="46b64-357">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="46b64-358">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="46b64-358">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="46b64-359">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="46b64-359">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="46b64-360">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="46b64-360">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="46b64-361">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-361">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="46b64-362">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="46b64-362">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="46b64-363">Veri koruma anahtarı halka kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="46b64-363">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="46b64-364">**Veri koruma kayıt defteri anahtarları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="46b64-364">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="46b64-365">ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="46b64-365">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="46b64-366">Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="46b64-366">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="46b64-367">Tek başına, webfarm olmayan IIS yüklemeleri [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) ile ASP.NET Core uygulaması kullanılan her bir uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-367">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="46b64-368">Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="46b64-368">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="46b64-369">Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="46b64-369">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="46b64-370">Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-370">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="46b64-371">Varsayılan olarak, veri koruma anahtarları şifreli değildir.</span><span class="sxs-lookup"><span data-stu-id="46b64-371">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="46b64-372">Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="46b64-372">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="46b64-373">X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-373">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="46b64-374">Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun.</span><span class="sxs-lookup"><span data-stu-id="46b64-374">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="46b64-375">Bkz: [ASP.NET Core veri koruma yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="46b64-375">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="46b64-376">**Kullanıcı profili yüklemek için IIS uygulama havuzu yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="46b64-376">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="46b64-377">Bu ayar **işlem modeli** bölümüne **Gelişmiş ayarlar** uygulama havuzu için.</span><span class="sxs-lookup"><span data-stu-id="46b64-377">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="46b64-378">Ayarlanan kullanıcı profilini Yükle `True`.</span><span class="sxs-lookup"><span data-stu-id="46b64-378">Set Load User Profile to `True`.</span></span> <span data-ttu-id="46b64-379">Bu kullanıcı profili dizini altında anahtarlarını depolar ve bunları koruyan DPAPI ile uygulama havuzu tarafından kullanılan kullanıcı hesabı için belirli bir anahtar kullanarak.</span><span class="sxs-lookup"><span data-stu-id="46b64-379">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="46b64-380">**Dosya sistemi anahtarı halkası deposu olarak kullanın**</span><span class="sxs-lookup"><span data-stu-id="46b64-380">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="46b64-381">Uygulama kodu ayarlamak [dosya sistemi bir anahtar halkası deposu olarak kullanırsınız](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="46b64-381">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="46b64-382">Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="46b64-382">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="46b64-383">Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.</span><span class="sxs-lookup"><span data-stu-id="46b64-383">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="46b64-384">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="46b64-384">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="46b64-385">Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="46b64-385">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="46b64-386">X X509 dağıtma her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="46b64-386">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="46b64-387">Yapılandırma [kod veri koruması](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="46b64-387">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="46b64-388">**Veri koruma için makine genelinde bir ilke ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="46b64-388">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="46b64-389">Veri koruma sisteminde bir varsayılan ayarı desteği sınırlıdır [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'lerini kullanan tüm uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="46b64-389">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="46b64-390">Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="46b64-390">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="46b64-391">Alt uygulama yapılandırma</span><span class="sxs-lookup"><span data-stu-id="46b64-391">Sub-application configuration</span></span>

<span data-ttu-id="46b64-392">Kök uygulama altında eklenen alt uygulamalar, ASP.NET Core modülü bir işleyici içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="46b64-392">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="46b64-393">Modül bir alt uygulamasının işleyici olarak eklenip eklenmediğini *web.config* dosyası bir *iç sunucu hatası 500.19* hatalı yapılandırma dosyasına başvuran alındığında alt uygulama göz atmak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="46b64-393">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="46b64-394">Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosyası için bir ASP.NET Core alt uygulama:</span><span class="sxs-lookup"><span data-stu-id="46b64-394">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

<span data-ttu-id="46b64-395">ASP.NET Core uygulaması altında ASP.NET Core sub-uygulama barındırma, devralınan işleyici alt uygulamada açıkça Kaldır *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="46b64-395">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

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

<span data-ttu-id="46b64-396">ASP.NET Core modülü yapılandırma hakkında daha fazla bilgi için bkz. [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) konu ve [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="46b64-396">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="46b64-397">Web.config ile IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="46b64-397">Configuration of IIS with web.config</span></span>

<span data-ttu-id="46b64-398">IIS yapılandırması tarafından etkilenir  **\<system.webServer >** bölümünü *web.config* IIS özelliklerden bir ters proxy yapılandırmasını uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="46b64-398">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="46b64-399">IIS dinamik sıkıştırması kullanmak için sunucu düzeyinde yapılandırılmışsa  **\<urlCompression >** uygulamanın öğesinde *web.config* dosya devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46b64-399">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="46b64-400">Daha fazla bilgi için [yapılandırma başvurusu için \<system.webServer >](/iis/configuration/system.webServer/), [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module), ve [ASP.NET içeren IIS modülleri Çekirdek](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="46b64-400">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="46b64-401">(IIS 10.0 veya üzeri desteklenir) yalıtılmış uygulama havuzlarında çalışmakta olan tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgeleri.</span><span class="sxs-lookup"><span data-stu-id="46b64-401">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="46b64-402">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="46b64-402">Configuration sections of web.config</span></span>

<span data-ttu-id="46b64-403">ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma için ASP.NET Core uygulamaları tarafından kullanılmaz:</span><span class="sxs-lookup"><span data-stu-id="46b64-403">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="46b64-404">**\<System.Web >**</span><span class="sxs-lookup"><span data-stu-id="46b64-404">**\<system.web>**</span></span>
* <span data-ttu-id="46b64-405">**\<appSettings >**</span><span class="sxs-lookup"><span data-stu-id="46b64-405">**\<appSettings>**</span></span>
* <span data-ttu-id="46b64-406">**\<connectionStrings >**</span><span class="sxs-lookup"><span data-stu-id="46b64-406">**\<connectionStrings>**</span></span>
* <span data-ttu-id="46b64-407">**\<Konum >**</span><span class="sxs-lookup"><span data-stu-id="46b64-407">**\<location>**</span></span>

<span data-ttu-id="46b64-408">ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="46b64-408">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="46b64-409">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="46b64-409">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="46b64-410">Uygulama havuzları</span><span class="sxs-lookup"><span data-stu-id="46b64-410">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="46b64-411">Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="46b64-411">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="46b64-412">İşlemdeki barındırma &ndash; uygulamalarını ayrı uygulama havuzlarında çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="46b64-412">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="46b64-413">Barındırma işlemi çıkış &ndash; her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="46b64-413">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="46b64-414">IIS **Web sitesi Ekle** iletişim varsayılan olarak bir uygulama başına tek bir uygulama havuzu.</span><span class="sxs-lookup"><span data-stu-id="46b64-414">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="46b64-415">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="46b64-415">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="46b64-416">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="46b64-416">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="46b64-417">Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="46b64-417">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="46b64-418">IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="46b64-418">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="46b64-419">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="46b64-419">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="46b64-420">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="46b64-420">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="46b64-421">Uygulama havuzu kimliği</span><span class="sxs-lookup"><span data-stu-id="46b64-421">Application Pool Identity</span></span>

<span data-ttu-id="46b64-422">Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="46b64-422">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="46b64-423">IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="46b64-423">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="46b64-424">IIS Yönetim konsolunda altında **Gelişmiş ayarlar** emin olmak için uygulama havuzu, **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="46b64-424">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="46b64-426">IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="46b64-426">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="46b64-427">Bu kimlik kullanarak kaynakları güvenliği sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-427">Resources can be secured using this identity.</span></span> <span data-ttu-id="46b64-428">Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.</span><span class="sxs-lookup"><span data-stu-id="46b64-428">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="46b64-429">IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="46b64-429">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="46b64-430">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="46b64-430">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="46b64-431">Sağ tıklatın ve dizin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="46b64-431">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="46b64-432">Altında **güvenlik** sekmesinde **Düzenle** düğmesine ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="46b64-432">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="46b64-433">Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="46b64-433">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="46b64-434">ENTER **IIS uygulama havuzu\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alan.</span><span class="sxs-lookup"><span data-stu-id="46b64-434">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="46b64-435">Seçin **Adları Denetle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="46b64-435">Select the **Check Names** button.</span></span> <span data-ttu-id="46b64-436">İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="46b64-436">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="46b64-437">Zaman **Adları Denetle** düğmesi seçili değerini **DefaultAppPool** nesne adları alanında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="46b64-437">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="46b64-438">Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="46b64-438">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="46b64-439">Kullanım **IIS uygulama havuzu\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.</span><span class="sxs-lookup"><span data-stu-id="46b64-439">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="46b64-441">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="46b64-441">Select **OK**.</span></span>

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "Adları denetle"'i seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="46b64-443">Okuma &amp; Yürütme izinleri varsayılan verilmesi.</span><span class="sxs-lookup"><span data-stu-id="46b64-443">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="46b64-444">Gerektiğinde ek izinler sağlar.</span><span class="sxs-lookup"><span data-stu-id="46b64-444">Provide additional permissions as needed.</span></span>

<span data-ttu-id="46b64-445">Erişim de verilebilir bir komut istemi kullanarak **ICACLS** aracı.</span><span class="sxs-lookup"><span data-stu-id="46b64-445">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="46b64-446">Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:</span><span class="sxs-lookup"><span data-stu-id="46b64-446">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="46b64-447">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls) konu.</span><span class="sxs-lookup"><span data-stu-id="46b64-447">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="46b64-448">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="46b64-448">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="46b64-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki IIS dağıtım senaryolarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="46b64-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="46b64-450">İşlem içi</span><span class="sxs-lookup"><span data-stu-id="46b64-450">In-process</span></span>
  * <span data-ttu-id="46b64-451">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="46b64-451">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="46b64-452">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="46b64-452">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="46b64-453">İşlem dışı</span><span class="sxs-lookup"><span data-stu-id="46b64-453">Out-of-process</span></span>
  * <span data-ttu-id="46b64-454">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="46b64-454">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="46b64-455">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="46b64-455">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="46b64-456">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="46b64-456">TLS 1.2 or later connection</span></span>

<span data-ttu-id="46b64-457">Bir HTTP/2 bağlantı kurulduğunda, işlem içi dağıtımı için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="46b64-457">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="46b64-458">Bir HTTP/2 bağlantı kurulduğunda, bir işlem dışı dağıtım için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="46b64-458">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="46b64-459">İşlem içi ve dışı işlem barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module> konu ve <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="46b64-459">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="46b64-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki temel gereksinimlere işlem dışı dağıtımları için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="46b64-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="46b64-461">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="46b64-461">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="46b64-462">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="46b64-462">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="46b64-463">Hedef çerçeve: uygulanamaz işlem dışı dağıtımlar için bu yana HTTP/2 bağlantı tamamen IIS tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="46b64-463">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="46b64-464">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="46b64-464">TLS 1.2 or later connection</span></span>

<span data-ttu-id="46b64-465">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="46b64-465">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="46b64-466">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="46b64-466">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="46b64-467">Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner.</span><span class="sxs-lookup"><span data-stu-id="46b64-467">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="46b64-468">IIS dağıtımları olan HTTP/2 yapılandırma hakkında daha fazla bilgi için bkz. [HTTP/2 IIS'de](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="46b64-468">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="46b64-469">IIS Yöneticiler için dağıtım kaynakları</span><span class="sxs-lookup"><span data-stu-id="46b64-469">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="46b64-470">IIS IIS belgelerinde kapsamlı hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="46b64-470">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="46b64-471">IIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="46b64-471">IIS documentation</span></span>](/iis)

<span data-ttu-id="46b64-472">.NET Core uygulaması dağıtım modelleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="46b64-472">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="46b64-473">.NET core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="46b64-473">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="46b64-474">ASP.NET Core modülü Kestrel web sunucusu, bir ters proxy sunucusu olarak IIS veya IIS Express kullanacak şekilde nasıl olanak tanıdığını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="46b64-474">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="46b64-475">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="46b64-475">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="46b64-476">ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="46b64-476">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="46b64-477">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="46b64-477">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="46b64-478">Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="46b64-478">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="46b64-479">Dizin yapısı</span><span class="sxs-lookup"><span data-stu-id="46b64-479">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="46b64-480">ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri keşfedin.</span><span class="sxs-lookup"><span data-stu-id="46b64-480">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="46b64-481">IIS modülleri</span><span class="sxs-lookup"><span data-stu-id="46b64-481">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="46b64-482">ASP.NET Core uygulamaları IIS dağıtımları sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="46b64-482">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="46b64-483">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="46b64-483">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="46b64-484">Sık karşılaşılan barındırırken IIS üzerinde ASP.NET Core uygulamaları ayırmak.</span><span class="sxs-lookup"><span data-stu-id="46b64-484">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="46b64-485">Azure App Service ve IIS için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="46b64-485">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="46b64-486">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="46b64-486">Additional resources</span></span>

* [<span data-ttu-id="46b64-487">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="46b64-487">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="46b64-488">Resmi Microsoft IIS sitesi</span><span class="sxs-lookup"><span data-stu-id="46b64-488">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="46b64-489">Windows Server Teknik İçerik Kitaplığı</span><span class="sxs-lookup"><span data-stu-id="46b64-489">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="46b64-490">IIS HTTP/2</span><span class="sxs-lookup"><span data-stu-id="46b64-490">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
